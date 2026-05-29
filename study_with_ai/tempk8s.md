# Kubernetes KCNA Cheatsheet — Q51–Q100

| Q# | Answer | One-Line Explanation |
|----|--------|----------------------|
| 51 | **A** — Deployments manage ReplicaSets | A Deployment is the boss; it creates and controls ReplicaSets, which in turn control Pods. |
| 52 | **A** — Memory requests, node taints, Pod affinity | Scheduler looks at what the Pod needs vs. what the node allows/offers (taints, affinity rules). |
| 53 | **D** — Gauge | A Gauge can go up AND down (e.g., CPU usage). A Counter only goes up. |
| 54 | **B** — Labels | Labels are key-value tags attached to objects; they are the main way to group and identify things. |
| 55 | **B** — Service | A Service is the K8s resource that exposes an application (stable IP/DNS in front of Pods). |
| 56 | **A** — DaemonSet runs a Pod copy on specific nodes | DaemonSet = "one Pod per node" (or per a set of nodes), useful for log collectors, agents, etc. |
| 57 | **D** — Traces | A Trace is the end-to-end journey of a request through distributed services (made of Spans). |
| 58 | **B** — CRI-O | CRI-O is a container runtime; it's what actually *runs* the containers on a node. |
| 59 | **D** — REST | Pods and Services are REST API objects — you interact with them via the Kubernetes REST API. |
| 60 | **A** — kube-proxy | kube-proxy handles all network traffic rules inside/outside the cluster using iptables/IPVS. |
| 61 | **C** — kube-apiserver | The API server is the front door of the cluster — all commands/tools talk to it. |
| 62 | **B** — Services without selectors | If a Service has no selector, K8s doesn't know which Pods to route to, so you create Endpoints manually. |
| 63 | **A** — kubectl explain | `kubectl explain <resource>` prints the field docs/schema for any resource. |
| 64 | **B** — Linkerd | Linkerd is a service mesh — it sits between services to manage traffic, policy, and telemetry transparently. |
| 65 | **C** — ConfigMap | ConfigMaps support `immutable: true` to prevent accidental changes. |
| 66 | **B** — Pods communicate without NAT | The K8s network model guarantees every Pod can reach every other Pod directly without NAT. |
| 67 | **A** — Pod | A Pod is the basic scheduling unit that wraps one or more containers. |
| 68 | **B** — Yes, but each port must be named | Multi-port Services are allowed; when using multiple ports, each port needs a unique name. |
| 69 | **A** — Site Reliability Engineers (SRE) | SREs own the incident management process — they define, test, and run it. |
| 70 | **A** — Rolling update | K8s default strategy replaces Pods gradually (rolling) so there is no downtime. |
| 71 | **B** — kubectl explain deployment.spec.replicas | Dot-notation drills into a specific field's documentation. |
| 72 | **C** — Outline "terms of engagement" | The governance board sets the rules of how the community operates — its norms and processes. |
| 73 | **B** — Classify Pods as isolated vs non-isolated | A NetworkPolicy makes a Pod *isolated* (traffic restricted) or leaves it *non-isolated* (open). |
| 74 | **B** — Network throughput and disk I/O | etcd writes data to disk on every commit and replicates over the network — both must be fast. |
| 75 | **C** — Create a manifest, apply with kubectl | Without extra tools, you write YAML manifests and `kubectl apply -f` them. |
| 76 | **A** — kubectl exec -- | `kubectl exec <pod> -- <command>` runs a command inside a running container. |
| 77 | **B** — .spec.clusterIP: None | Setting clusterIP to `None` makes the Service headless (no virtual IP; DNS returns Pod IPs directly). |
| 78 | **A** — User includes StorageClass in PVC | Dynamic provisioning: user references a StorageClass in a PVC; the cloud creates the volume automatically. |
| 79 | **A** — Schedule, scale, and manage health | Orchestration tools schedule where containers run, scale them, and keep them healthy. |
| 80 | **C** — Mix of public & private clouds + on-prem | Hybrid Cloud = public cloud + private cloud/on-premises data centers working together. |
| 81 | **D** — IP addresses of Pods assigned to a Service | Endpoints object holds the real Pod IPs that a Service routes traffic to. |
| 82 | **B** — Removes constraints to rapid innovation | Cloud-Native lets teams ship faster by removing infrastructure bottlenecks. |
| 83 | **C** — Pod | Pod is the smallest deployable unit in K8s (not a Container — K8s doesn't schedule bare containers). |
| 84 | **C** — kubectl top | `kubectl top nodes` / `kubectl top pods` shows live CPU and memory usage. |
| 85 | **A** — Environment variables and DNS | K8s injects Service info as env vars at Pod start, and CoreDNS resolves Service names automatically. |
| 86 | **D** — NET_BIND_SERVICE | Restricted policy only allows adding `NET_BIND_SERVICE` (lets containers bind to ports < 1024). |
| 87 | **C** — kubectl scale AND kubectl edit | You can scale by `kubectl scale deployment ... --replicas=N` or by editing the replica count with `kubectl edit`. |
| 88 | **C** — Container next to another container in same Pod | A sidecar is an extra container in the same Pod that supports the main container (e.g., log shipper). |
| 89 | **C** — containerd | containerd is the OCI-compliant, industry-standard runtime emphasising simplicity and portability. |
| 90 | **C** — Adding/removing resources to the app | Vertical scaling = give the app more CPU/RAM (bigger machine). Horizontal = more instances. |
| 91 | **B** — Gauge | Gauge can go up and down (same as Q53 — know this!). |
| 92 | **A** — Backend services on an as-used basis | Serverless = you pay/run only when code executes; no server management required. |
| 93 | **C** — Interface for pluggable container runtimes | CRI (Container Runtime Interface) lets K8s work with any runtime (containerd, CRI-O, etc.). |
| 94 | **B** — CronJob | CronJob = scheduled recurring job (like a cron on Linux). Job = one-time task. |
| 95 | **A** — Open Container Initiative (OCI) | OCI defines the standard for container image formats and runtimes. |
| 96 | **C** — v1alpha1, v2beta3, v2 | Valid K8s API version format: `v<N>alpha<N>`, `v<N>beta<N>`, `v<N>` (stable). |
| 97 | **A** — kubectl logs -p -c ruby web-1 | `-p` = previous (terminated) container; `-c ruby` = container name; `web-1` = Pod name. |
| 98 | **C** — Service | Classic K8s definition: *"A Service defines a logical set of Pods and a policy to access them."* |
| 99 | **D** — 6 hosts (3 control plane + 3 etcd) | External etcd HA topology: 3 separate etcd nodes + 3 control plane nodes = 6 total. |
| 100 | **D** — New Pod created with no assigned node | The scheduler only acts when a Pod exists but hasn't been placed on a node yet. |

---

## Quick Memory Hooks

- **Gauge vs Counter** — Gauge = goes up & down (temperature). Counter = only up (request count).
- **Trace vs Span** — A Trace is the whole journey; Spans are individual steps inside it.
- **Deployment → ReplicaSet → Pod** — Three layers, each manages the next.
- **Headless Service** — `clusterIP: None` → no VIP, DNS returns Pod IPs directly.
- **DaemonSet** — One Pod per node (great for agents/log collectors).
- **CronJob vs Job** — CronJob = recurring schedule. Job = run once to completion.
- **kubectl explain** = docs. **kubectl describe** = live state. **kubectl top** = resource usage.
- **OCI** = container standards. **CRI** = K8s runtime plugin interface. **CNI** = network plugin interface.
- **Hybrid Cloud** = public + private + on-prem combined.
- **Sidecar** = helper container sharing the same Pod as the main app container.
- **HA external etcd** = 3 control plane + 3 etcd = **6 hosts**.
