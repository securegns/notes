# KCNA Study Guide: Full Cheatsheet + 100% Coverage Labs

> Goal: **100% coverage for KCNA**. This guide lists **all topics and subtopics** commonly covered by the KCNA exam and provides **labs organized by syllabus categories** so you can practice every objective.

---

## 1) Kubernetes Fundamentals (Core Concepts)

### Topics & Subtopics
- **Kubernetes Basics**
  - What Kubernetes solves: orchestration, scheduling, self-healing
  - Cluster vs. node vs. pod vs. container
  - Declarative vs. imperative management
- **Core API Objects**
  - **Pod**: lifecycle, restart policies, init containers, sidecars
  - **ReplicaSet**: ensuring desired replicas
  - **Deployment**: rollout/rollback, strategies
  - **StatefulSet**: stable identity, ordered rollout
  - **DaemonSet**: one-per-node workloads
  - **Job/CronJob**: batch workloads
  - **Service**: ClusterIP, NodePort, LoadBalancer, ExternalName
  - **Namespace**: isolation and resource separation
  - **Label/Selector/Annotation**: grouping and metadata
- **Configuration**
  - **ConfigMap**: configuration data
  - **Secret**: sensitive data
  - **Env vars** vs. volume mounts
- **Storage**
  - **Volume** types: emptyDir, hostPath, configMap/secret, persistent
  - **PersistentVolume (PV)** & **PersistentVolumeClaim (PVC)**
  - **StorageClass** (dynamic provisioning)
- **Scheduling & Placement**
  - Node selectors, affinities, taints/tolerations
- **Health & Lifecycle**
  - Liveness/Readiness/Startup probes
  - Pod lifecycle phases

### Labs (Fundamentals)
1. **Pod Lifecycle Lab**
   - Create a Pod; add liveness/readiness probes; observe restarts.
2. **Deployments & Rollbacks**
   - Create a Deployment; update image; rollback to previous revision.
3. **Services & Networking Basics**
   - Expose a Deployment via ClusterIP/NodePort; test connectivity.
4. **ConfigMaps & Secrets**
   - Create a ConfigMap and Secret; mount them as env and volume.
5. **Stateful vs. Stateless**
   - Deploy a StatefulSet with PVCs; compare with Deployment.
6. **Jobs & CronJobs**
   - Run a Job; schedule a CronJob; verify completion history.

---

## 2) Kubernetes Architecture

### Topics & Subtopics
- **Control Plane Components**
  - kube-apiserver, etcd, kube-scheduler, kube-controller-manager
- **Node Components**
  - kubelet, kube-proxy, container runtime (containerd)
- **Kubernetes API**
  - Resources, API groups/versions, manifests, schema
- **Admission Control & Controllers**
  - Mutating/Validating admission
  - Reconciliation loops
- **Cluster Networking**
  - Pod-to-Pod, Service, Ingress
  - CNI plugins (high-level concept)

### Labs (Architecture)
1. **Control Plane Understanding**
   - Use `kubectl get componentstatuses`/`kubectl get --raw` to explore.
2. **API Exploration**
   - Discover API groups/versions; create resources using manifests.
3. **kubelet & Node View**
   - Inspect node status, conditions, and kubelet configuration.

---

## 3) Cloud Native Ecosystem & CNCF

### Topics & Subtopics
- **CNCF Overview**
  - CNCF mission, landscape categories
- **Cloud Native Principles**
  - Microservices, immutable infrastructure, declarative APIs
- **Containerization**
  - Images, registries, container runtime
- **Service Mesh (Conceptual)**
  - Sidecars, traffic management, observability
- **GitOps Concepts**
  - Desired state in Git, reconciliation

### Labs (Cloud Native & CNCF)
1. **Container Basics**
   - Build a simple container image; run and inspect it.
2. **GitOps Simulation**
   - Store Kubernetes manifests in Git; apply updates and revert changes.

---

## 4) Kubernetes Security

### Topics & Subtopics
- **RBAC**
  - Roles, ClusterRoles, RoleBindings
- **Authentication & Authorization**
  - ServiceAccounts, tokens, certificates
- **Pod Security**
  - Pod Security Standards (baseline/restricted)
- **Network Security**
  - NetworkPolicies (deny/allow)
- **Secrets Management**
  - Best practices for Secrets

### Labs (Security)
1. **RBAC Lab**
   - Create a ServiceAccount; bind a Role; verify permissions.
2. **Pod Security**
   - Apply Pod Security standards; observe enforcement.
3. **NetworkPolicy**
   - Deny all traffic; allow only specific namespaces/pods.

---

## 5) Kubernetes Networking

### Topics & Subtopics
- **Networking Model**
  - Flat network, Pod IPs, Services
- **Service Types**
  - ClusterIP, NodePort, LoadBalancer
- **Ingress**
  - Ingress resources, Ingress controllers (concept)
- **DNS**
  - CoreDNS basics, service discovery

### Labs (Networking)
1. **Service Types Practice**
   - Deploy service types and test routing.
2. **Ingress Basics**
   - Install an Ingress controller; configure simple routing.
3. **DNS Discovery**
   - Resolve service names from within pods.

---

## 6) Kubernetes Storage

### Topics & Subtopics
- **Volumes & Persistent Storage**
  - Ephemeral vs persistent
- **Storage Classes & Dynamic Provisioning**
- **PVC Lifecycle**

### Labs (Storage)
1. **Dynamic Provisioning**
   - Create StorageClass; request PVC; mount in Pod.
2. **Data Persistence**
   - Write data to PVC; delete pod; verify data persists.

---

## 7) Observability & Troubleshooting

### Topics & Subtopics
- **Logging**
  - `kubectl logs`, sidecar logging
- **Monitoring**
  - Metrics basics, kube-state-metrics (conceptual)
- **Troubleshooting**
  - `kubectl describe`, `kubectl exec`, events

### Labs (Observability)
1. **Logs & Events**
   - Retrieve logs, view events, fix a crashing pod.
2. **Troubleshoot Services**
   - Break a Service selector; diagnose and fix.

---

## 8) Kubernetes Installation & Configuration (High-Level)

### Topics & Subtopics
- **Cluster Setup Basics**
  - Managed vs. self-managed clusters
  - kubeadm high-level steps
- **Configuration & Contexts**
  - kubeconfig files, contexts, namespaces

### Labs (Installation/Config)
1. **kubeconfig Practice**
   - Create contexts and switch between namespaces.
2. **Managed Cluster Setup (Optional)**
   - Provision a managed cluster (GKE/EKS/AKS) and connect with kubectl.

---

# KCNA 100% Coverage Lab Index (Categorized)

### Kubernetes Fundamentals
- Pod Lifecycle Lab
- Deployments & Rollbacks
- Services & Networking Basics
- ConfigMaps & Secrets
- Stateful vs. Stateless
- Jobs & CronJobs

### Architecture
- Control Plane Understanding
- API Exploration
- kubelet & Node View

### Cloud Native & CNCF
- Container Basics
- GitOps Simulation

### Security
- RBAC Lab
- Pod Security
- NetworkPolicy

### Networking
- Service Types Practice
- Ingress Basics
- DNS Discovery

### Storage
- Dynamic Provisioning
- Data Persistence

### Observability & Troubleshooting
- Logs & Events
- Troubleshoot Services

### Installation & Configuration
- kubeconfig Practice
- Managed Cluster Setup (Optional)

---

# Daily Study Workflow (for 100% readiness)

1. **Read** the topic list from each category.
2. **Do** the labs in the matching category.
3. **Review** failed areas and repeat labs.

---

If you want, I can expand each lab with step-by-step commands and expected outputs.
