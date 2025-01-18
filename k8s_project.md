# Network policy
```
#!/usr/bin/env bash
#
# create-netpol.sh
# ----------------
# Creates 3 namespaces: dev, uat, prod
# Labels them
# Applies NetworkPolicies to block direct dev <-> prod traffic,
# but allow dev <-> uat and prod <-> uat.
#
# Usage:
#   chmod +x create-netpol.sh
#   ./create-netpol.sh
#

set -e

echo "=== Creating namespaces dev, uat, prod (if not present) ==="
kubectl create namespace dev  || true
kubectl create namespace uat  || true
kubectl create namespace prod || true

echo
echo "=== Labeling namespaces ==="
kubectl label namespace dev name=dev   --overwrite
kubectl label namespace uat name=uat   --overwrite
kubectl label namespace prod name=prod --overwrite

echo
echo "=== Applying NetworkPolicies to block dev <-> prod ==="
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-only-dev-and-uat
  namespace: dev
spec:
  podSelector:
    matchLabels: {}
  policyTypes:
  - Ingress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: dev
      - namespaceSelector:
          matchLabels:
            name: uat
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-only-prod-and-uat
  namespace: prod
spec:
  podSelector:
    matchLabels: {}
  policyTypes:
  - Ingress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: prod
      - namespaceSelector:
          matchLabels:
            name: uat
EOF

echo
echo "=== Summary: ==="
echo "1. Namespaces 'dev', 'uat', 'prod' created (if not already)."
echo "2. 'dev' netpol allows only from dev+uat => blocks prod->dev."
echo "3. 'prod' netpol allows only from prod+uat => blocks dev->prod."
echo "4. 'uat' has no netpol => open to/from dev and prod."
echo
echo "Done! dev <-> prod are now blocked, dev <-> uat and prod <-> uat remain open."
```
# Istio
```
kubectl create namespace dev
kubectl create namespace uat
kubectl create namespace prod

kubectl label namespace dev istio-injection=enabled
kubectl label namespace uat istio-injection=enabled
kubectl label namespace prod istio-injection=enabled

kubectl create deployment httpd-dev --image=httpd -n dev
kubectl create deployment httpd-uat --image=httpd -n uat
kubectl create deployment httpd-prod --image=httpd -n prod

# Expose the deployments as services
kubectl expose deployment httpd-dev --port=80 -n dev --name=httpd-service
kubectl expose deployment httpd-uat --port=80 -n uat --name=httpd-service
kubectl expose deployment httpd-prod --port=80 -n prod --name=httpd-service

# Create curl pods in each namespace
kubectl run -n dev curl-dev --image=curlimages/curl --command -- sleep infinity
kubectl run -n uat curl-uat --image=curlimages/curl --command -- sleep infinity
kubectl run -n prod curl-prod --image=curlimages/curl --command -- sleep infinity


# Enable mTLS in strict mode for each namespace
kubectl apply -n dev -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF

kubectl apply -n uat -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF

kubectl apply -n prod -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF

# Apply AuthorizationPolicies to control traffic

# In 'dev' namespace, allow ingress only from 'uat'
kubectl apply -n dev -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-uat
spec:
  rules:
  - from:
    - source:
        namespaces: ["uat"]
EOF

# In 'prod' namespace, allow ingress only from 'uat'
kubectl apply -n prod -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-uat
spec:
  rules:
  - from:
    - source:
        namespaces: ["uat"]
EOF
```


# Python program

```
# This version do not deploy httpd pods in the port forwarding situation


#!/usr/bin/env python3

import subprocess
import json
import time
import sys
import os

BRIDGED_POD_IMAGE = "alpine/socat"
SOCAT_COMMAND = "socat TCP-LISTEN:8888,fork TCP:{target_ip}:80"
HTTPD_IMAGE = "httpd:alpine"
CURL_IMAGE = "curlimages/curl:latest"


def run_cmd(cmd):
    proc = subprocess.Popen(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    out, err = proc.communicate()
    combined = (out + err).decode("utf-8", errors="replace").strip()
    return proc.returncode, combined


def list_namespaces():
    code, raw_json = run_cmd(["kubectl", "get", "ns", "-o", "json"])
    if code != 0:
        return []
    data = json.loads(raw_json)
    ns_list = []
    for item in data.get("items", []):
        n = item["metadata"]["name"]
        if n not in ["kube-system", "kube-public", "kube-node-lease", "default"]:
            ns_list.append(n)
    return ns_list


def wait_for_pod(ns, pod_name, timeout=60):
    start_time = time.time()
    while time.time() - start_time < timeout:
        code, output = run_cmd([
            "kubectl", "get", "pod", pod_name, "-n", ns,
            "-o", "jsonpath={.status.phase}"
        ])
        if code == 0 and output.strip() == "Running":
            return 0, ""
        time.sleep(5)
    return 1, f"{pod_name} pod did not enter running state within {timeout}s"


def create_pod_manifest(ns, pod_name, image, ports=None, labels=None, command=None):
    manifest = {
        "apiVersion": "v1",
        "kind": "Pod",
        "metadata": {
            "name": pod_name,
            "namespace": ns,
            "labels": labels if labels else {}
        },
        "spec": {
            "containers": [
                {
                    "name": pod_name,
                    "image": image,
                    "ports": ports if ports else [],
                    "command": command if command else []
                }
            ]
        }
    }
    return json.dumps(manifest, indent=2)


def deploy_pod(ns, pod_name, image, ports=None, labels=None, command=None):
    manifest_content = create_pod_manifest(ns, pod_name, image, ports, labels, command)
    manifest_path = f"/tmp/{pod_name}.json"
    with open(manifest_path, "w") as f:
        f.write(manifest_content)
    run_cmd(["kubectl", "create", "-f", manifest_path])
    os.remove(manifest_path)
    wait_for_pod(ns, pod_name)


def cleanup_pod(ns, pod_name):
    run_cmd(["kubectl", "delete", "pod", pod_name, "-n", ns])


def get_pod_ip(ns, pod_name):
    code, ip = run_cmd([
        "kubectl", "get", "pod", pod_name, "-n", ns,
        "-o", "jsonpath={.status.podIP}"
    ])
    if code == 0 and ip:
        return ip
    return None


def test_connectivity(src_ns, dst_ns):
    dst_pod = f"httpd-{dst_ns}"
    dst_ip = get_pod_ip(dst_ns, dst_pod)
    if not dst_ip:
        return False

    cmd = [
        "kubectl", "exec", f"curl-{src_ns}", "-n", src_ns, "--",
        "curl", "--max-time", "5", f"http://{dst_ip}"
    ]

    code, _ = run_cmd(cmd)
    return (code == 0)


def setup_port_forward(bridge_ns, target_ns):
    bridge_pod = f"bridge-{bridge_ns}"
    target_pod = f"httpd-{target_ns}"

    target_ip = get_pod_ip(target_ns, target_pod)
    if not target_ip:
        return None

    deploy_pod(bridge_ns, bridge_pod, BRIDGED_POD_IMAGE, command=["sh", "-c", f"{SOCAT_COMMAND.format(target_ip=target_ip)}", "&"])

    bridge_ip = get_pod_ip(bridge_ns, bridge_pod)
    if not bridge_ip:
        return None

    return bridge_ip


def test_port_forwarded(src_ns, bridge_ip):
    cmd = [
        "kubectl", "exec", f"curl-{src_ns}", "-n", src_ns, "--",
        "curl", "--max-time", "5", f"http://{bridge_ip}:8888"
    ]
    code, _ = run_cmd(cmd)
    return (code == 0)


def option_list_namespaces():
    print("\nList of available namespaces:\n")
    ns_list = list_namespaces()
    if not ns_list:
        print("No custom namespaces found.\n")
    else:
        for ns in ns_list:
            print(f"  - {ns}")
        print()
    sys.exit(0)


def option_test_two_ns():
    print()
    ns1 = input("Enter the first namespace: ").strip()
    ns2 = input("Enter the second namespace: ").strip()

    print(f"\n[!] We are deploying pods (apache2, curl) in {ns1} and {ns2}...\n")
    
    deploy_pod(ns1, f"httpd-{ns1}", HTTPD_IMAGE, ports=[{"containerPort": 80}], labels={"app": "httpd"})
    deploy_pod(ns1, f"curl-{ns1}", CURL_IMAGE, command=["sleep", "infinity"])
    deploy_pod(ns2, f"httpd-{ns2}", HTTPD_IMAGE, ports=[{"containerPort": 80}], labels={"app": "httpd"})
    deploy_pod(ns2, f"curl-{ns2}", CURL_IMAGE, command=["sleep", "infinity"])


    print("[!] Checking connectivity...\n")
    can_communicate = test_connectivity(ns1, ns2)
    if can_communicate:
        print(f"[x] Namespace {ns1} and Namespace {ns2} can communicate. "
              f"Check network policies if isolation is intended.\n")
    else:
        print(f"[+] Namespace {ns1} and Namespace {ns2} cannot communicate. "
              f"The network policies (or other restrictions) are in place.\n")

    print(f"[!] Cleaning up resources in {ns1} and {ns2}...\n")
    cleanup_pod(ns1, f"httpd-{ns1}")
    cleanup_pod(ns1, f"curl-{ns1}")
    cleanup_pod(ns2, f"httpd-{ns2}")
    cleanup_pod(ns2, f"curl-{ns2}")
    print()
    sys.exit(0)


def option_test_port_forward():
    print()
    ns1 = input("Enter the first namespace: ").strip()
    ns2 = input("Enter the second namespace: ").strip()
    bridge_ns = input("Enter the bridging namespace: ").strip()

    print(f"\n[!] We are deploying pods (apache2, curl) in {ns1}, {ns2}, "
          f"and bridging namespace {bridge_ns}...\n")

    deploy_pod(ns1, f"curl-{ns1}", CURL_IMAGE, command=["sleep", "infinity"])
    deploy_pod(ns2, f"httpd-{ns2}", HTTPD_IMAGE, ports=[{"containerPort": 80}], labels={"app": "httpd"})

    print(f"[+] Attempting port forwarding via {bridge_ns}...\n")
    bridge_ip = setup_port_forward(bridge_ns, ns2)
    if not bridge_ip:
        print(f"[+] Could not set up port forwarding in {bridge_ns}. "
              f"Communication remains blocked.\n")
    else:
        print(f"[!] Testing port-forwarded connection from {ns1} -> {bridge_ip}:8888...\n")
        pf_ok = test_port_forwarded(ns1, bridge_ip)
        if pf_ok:
            print(f"[x] With port forwarding via {bridge_ns}, "
                  f"{ns1} can now reach {ns2}. Network path opened.\n")
        else:
            print(f"[+] Even with port forwarding, {ns1} cannot reach {ns2}. "
                  f"Check rules or RBAC.\n")

    print(f"[!] Cleaning up resources in {ns1}, {ns2}, and {bridge_ns}...\n")
    cleanup_pod(ns1, f"curl-{ns1}")
    cleanup_pod(ns2, f"httpd-{ns2}")
    cleanup_pod(bridge_ns, f"bridge-{bridge_ns}")
    print()
    sys.exit(0)


def main():
    while True:
        print("\n[MENU] Choose an option:")
        print("1. List namespaces")
        print("2. Test if 2 namespaces can communicate")
        print("3. Test port forwarding namespace")
        print("4. Exit")

        choice = input("Enter your choice (1/2/3/4): ").strip()
        if choice == "1":
            option_list_namespaces()
        elif choice == "2":
            option_test_two_ns()
        elif choice == "3":
            option_test_port_forward()
        elif choice == "4":
            print("\nExiting.\n")
            sys.exit(0)
        else:
            print("\nInvalid selection. Please try again.")


if __name__ == "__main__":
    main()
```

