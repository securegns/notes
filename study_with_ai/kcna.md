# KCNA 2026 – COMPLETE BLUEPRINT MASTER SHEET (IMPROVED)

---

## EXAM MECHANICS

| Detail | Value |
|--------|-------|
| Format | 60 Multiple Choice Questions |
| Duration | 90 Minutes (~1.5 min per question) |
| Passing Score | ~75% (may vary; commonly reported threshold) |
| Environment | Closed-book, online proctored, no documentation access |
| Attempts | 2 attempts included with purchase |
| Validity | Certification valid for 2 years |

### Strategy
- **Pass 1:** Answer all confident questions immediately
- **Flag** anything uncertain and move on
- **Pass 2:** Return to flagged questions — eliminate distractors
- **Watch for absolute words:** "always", "never", "only", "must" → usually wrong
- **Watch for "best" / "most appropriate"** → there may be multiple correct answers, pick the BEST one
- **Read ALL options** before selecting — some questions have subtle differences
- **Time check:** At 45 min, you should be ~30 questions done

---

## DOMAIN 1: KUBERNETES FUNDAMENTALS (44%)

> **Heaviest domain. Master this first. (48% weight reported by some sources)**

### 1.0 Foundational Concepts

#### VMs vs Containers vs Microservices

| Concept | Description |
|---------|-------------|
| **Virtual Machines** | Do not make the best use of space. Apps are not fully isolated — can cause config conflicts, security problems, or resource hogging. |
| **Containers** | Allow you to run multiple apps that are **virtually isolated** from each other. Lightweight, portable, consistent runtime. |
| **Microservices** | Multiple small apps each responsible for **one thing**. Functionality is isolated and **stateless**. Ideal for container orchestration. |

- Kubernetes is an **open-source container orchestration system** for deployment, scaling, and management
- K8s is ideal for **microservice architecture**
- K8s unique component is the **Pod** — a group of one or more containers with shared storage and network resources

#### In-Tree vs Out-of-Tree

| Concept | Description |
|---------|-------------|
| **In-Tree** | Plugins, components, or functionality that are **provided by default** or reside in the main K8s repository |
| **Out-of-Tree** | Plugins and components that **must be installed manually** and replace or extend default behavior |

> Example: In-tree volume plugins vs Out-of-tree CSI drivers

### 1.1 Cluster Architecture

#### Key Cluster Concepts

| Concept | Description |
|---------|-------------|
| **Cluster** | A logical grouping of all components — control plane + worker nodes |
| **Namespace** | A named logical grouping of Kubernetes components within a cluster |
| **Node** | A virtual machine or underlying server. Two types: Control Plane nodes and Worker nodes |

#### Control Plane Components

| Component | Role |
|-----------|------|
| **kube-apiserver** | Front-end / gateway to the cluster. ALL communication goes through it. RESTful API. Validates & processes requests. |
| **etcd** | Distributed key-value store. Stores ALL cluster state and configuration. Only the API server talks to etcd directly. |
| **kube-scheduler** | Watches for unscheduled Pods, assigns them to nodes based on resource requirements, affinity, taints, etc. |
| **kube-controller-manager** | Runs controller loops (Node controller, ReplicaSet controller, Endpoint controller, ServiceAccount controller, etc.). Ensures desired state = actual state. |
| **cloud-controller-manager** | Interfaces with cloud provider APIs (load balancers, routes, volumes). Separates cloud logic from core K8s. |

#### Worker Node Components

| Component | Role |
|-----------|------|
| **kubelet** | Agent on every node. Ensures containers in Pods are running. Talks to API server. Manages Pod lifecycle. Reports node status. |
| **kube-proxy** | Maintains network rules on nodes. Implements Service abstraction (iptables/IPVS). Routes traffic to correct Pods. |
| **Container Runtime** | Runs containers. Must implement CRI (Container Runtime Interface). Examples: **containerd** (default), CRI-O. Docker was removed in K8s 1.24. |

#### Kubelet (Deep Dive)
- Node agent that runs on **all nodes** (including control plane)
- Watches for Pod changes
- Configures container runtime to pull images and create namespaces
- Runs containers and reports node status
- Responsible for Pod internal API communication via the API server

#### Kube-Proxy (Deep Dive)
- Network proxy that runs on each node in the cluster
- Designed to **load balance traffic** to Pods
- Maintains network rules on nodes using the OS packet filtering layer
- Three operating modes:
  - **iptables** — Current default. Uses Linux iptables rules for routing.
  - **IPVS** — Future default. Uses hash tables, scales better (iptables bottlenecked at ~5000 nodes).
  - **Userspace** — Legacy mode. Kube-proxy forwards traffic itself.

#### Core Concepts

- **Desired State vs Current State** — K8s continuously reconciles to match `spec` (desired) with `status` (current)
- **Reconciliation Loop** — Controllers watch → compare → act to fix drift
- **Declarative Model** — You declare WHAT you want (YAML), K8s figures out HOW
- **Imperative Model** — You tell K8s exactly what to do step by step (kubectl run, kubectl create)
- **Declarative is preferred** in production (kubectl apply)

### 1.2 Kubernetes Objects & Workloads

#### Pod
- **Smallest deployable unit** in Kubernetes
- Contains one or more containers
- All containers in a Pod share: **network namespace** (same IP), **storage volumes**, **IPC**
- Pods are **ephemeral** — they are not restarted, they are replaced
- Each Pod gets a **unique IP address**

#### Labels, Selectors & Annotations

| Concept | Purpose |
|---------|---------|
| **Labels** | Key-value pairs attached to objects. Used for **selection and grouping**. Example: `app: frontend`, `env: prod` |
| **Selectors** | Filter objects by labels. Used by Services, Deployments, etc. Types: **equality-based** (`=`, `!=`) and **set-based** (`in`, `notin`, `exists`). Also: **Label selectors**, **Field selectors**, **Node selectors** |
| **Annotations** | Key-value metadata for **non-identifying info**. Used by tools, libraries, external systems. Not used for selection. Often used by Ingress. Example: build info, git commit hash. |

#### Controllers / Workload Resources

| Resource | Purpose |
|----------|---------|
| **ReplicaSet** | Ensures N identical Pod replicas are running. Uses label selectors. Rarely created directly. |
| **Deployment** | Manages ReplicaSets. Provides rolling updates, rollbacks, scaling. **Most common workload**. |
| **StatefulSet** | For stateful applications. Provides: stable network identity, stable persistent storage, ordered deployment/scaling. Pods start in same order, terminate in reverse order. Each Pod gets a unique name, index, and persistent volume. Examples: databases, ZooKeeper. |
| **DaemonSet** | Ensures exactly ONE Pod runs on every node (or selected nodes). Use cases: log collectors, monitoring agents, kube-proxy. |
| **Job** | Runs a Pod to completion (run-to-completion semantics). Supports parallelism. |
| **CronJob** | Creates Jobs on a schedule (cron syntax). Example: nightly backup. |

#### Workload Types
- **Stateless** — No persistent identity, any replica can handle any request (Deployments, ReplicaSets)
- **Stateful** — Requires persistent identity and storage, session data stored in memory on VM (StatefulSets)

#### Endpoints & Endpoint Slices

| Concept | Description |
|---------|-------------|
| **Endpoints** | Track IP addresses of Pods assigned to a Kubernetes Service. View with `kubectl get endpoints` |
| **Endpoint Slices** | Break up endpoints into smaller manageable segments. Each slice has a limit of **100 pods**. Essential for solving scaling problems. |

#### RPC & gRPC

| Concept | Description |
|---------|-------------|
| **RPC (Remote Procedure Call)** | Enables a program to communicate with another program on a remote machine. A framework of communication in distributed systems. |
| **gRPC** | Modern, open-source, high-performance RPC framework. **Kubernetes uses gRPC for Pod communication.** Think of it as a method instead of REST or GraphQL. |

#### kubectl Syntax

```
kubectl [command] [TYPE] [Name] [Flags]
```

| Component | Examples |
|-----------|----------|
| **Commands** | apply, get, logs, describe, exec |
| **Types** | deploy, pods, ns, pv, pvc, secret |
| **Name** | Case-sensitive resource name |
| **Flags** | Different per command, starts with `--` |

### 1.3 Services & Networking

#### Service
- Provides a **stable virtual IP (ClusterIP)** and **DNS name** for a set of Pods
- Selects backend Pods via **label selectors**
- Performs **load balancing** across Pods
- Decouples consumers from Pod IP changes

#### Service Types

| Type | Description |
|------|-------------|
| **ClusterIP** | Default. Internal-only virtual IP. Only reachable within the cluster. |
| **NodePort** | Exposes Service on each node's IP at a static port (30000-32767). Builds on ClusterIP. |
| **LoadBalancer** | Provisions an external cloud load balancer. Builds on NodePort. |
| **ExternalName** | Maps a Service to an external DNS name (CNAME record). No proxying. No selectors. |

#### Headless Service
- Set `clusterIP: None`
- No load balancing or virtual IP
- DNS returns individual Pod IPs directly
- Used with **StatefulSets** for stable DNS per Pod

#### Services Traffic Policies

| Policy | Description |
|--------|-------------|
| **External Traffic Policy: Cluster** | Route external traffic to **all ready endpoints** (default) |
| **External Traffic Policy: Local** | Only route to ready **node-local endpoints** |
| **Internal Traffic Policy** | How traffic from internal sources is routed |

- If traffic policy is **Local** and there are no node-local endpoints, kube-proxy does **not forward any traffic**

#### Ingress
- **HTTP/HTTPS routing** to Services based on host/path rules
- **NOT a Service type** — it is a separate API resource
- Requires an **Ingress Controller** to function (e.g., NGINX Ingress Controller, Traefik, HAProxy)
- Supports TLS termination, name-based virtual hosting, path-based routing

#### Kubernetes Networking Model
- Every Pod gets its **own IP address**
- **Flat network** — all Pods can communicate with all other Pods without NAT
- All nodes can communicate with all Pods without NAT
- Pod-to-Pod communication across nodes is handled by the **CNI plugin**
- **DNS-based Service discovery** — CoreDNS resolves Service names to ClusterIPs
  - Format: `<service-name>.<namespace>.svc.cluster.local`
  - Each Pod has a `resolv.conf` file to help with DNS resolving

#### Four Types of Network Communication

| Type | Description |
|------|-------------|
| **Container-to-Container** | Containers in the same Pod share IP & port space. Communicate via **localhost** on different ports. |
| **Pod-to-Pod** | Root network namespace uses a **bridge** to allow all Pod network namespaces to talk. Routing enables cross-node communication. |
| **Pod-to-Service** | A Service creates a **virtualized IP** and uses iptables/IPVS for load balancing to Pods. Provides static IP when Pods die. |
| **External-to-Service** | External traffic reaches Pods via NodePort, LoadBalancer, or Ingress. |

#### NAT (Network Address Translation)
- Translates private IPs to public IPs and vice versa
- K8s networking model states Pods communicate **without NAT** — but NAT can still be used for external communication
- Conserves IP addresses and simplifies routing

#### Virtual Ethernet Devices (Veths)
- Act as **tunnels between network namespaces**
- Create a bridge to physical network devices in another namespace
- Packets on one device in the pair are **immediately received** on the other device
- Always created in **interconnected pairs**

#### Netfilter
- Linux kernel framework enabling:
  - Packet filtering
  - Network address & port translation
  - Packet logging & queuing
- Projects built on top: **iptables** (current default), **nftables**, **IPVS**

#### CNI (Container Network Interface)
- Plugin-based networking for Kubernetes
- Specification for writing plugins to configure networking interfaces for Linux containers
- Responsible for: assigning Pod IPs, setting up routes, network isolation
- Examples: **Calico**, **Cilium**, **Flannel**, **Weave Net**
- K8s does NOT implement networking itself — it delegates to CNI plugins

#### CoreDNS
- Default DNS server for Kubernetes (replaced kube-dns)
- Ensures Pods and Services have **Fully Qualified Domain Names (FQDN)**
- Plugin-based architecture:
  - **Internal Plugins**: acl, any, cache, health, log
  - **External Plugins**: git, alias
- CNCF **Graduated** project

#### Proxy Types in Kubernetes

| Proxy | Purpose |
|-------|---------|
| **kubectl proxy** | Localhost address to K8s API server |
| **Apiserver proxy** | Connects users outside cluster with cluster IPs |
| **kube-proxy** | Routes traffic to reach Services |
| **Proxy/Load Balancer** | Acts as load balancer if several API servers |
| **Cloud Load Balancer** | External traffic to reach Pods |

#### Forward Proxy vs Reverse Proxy

| Type | Description |
|------|-------------|
| **Forward Proxy** | Default proxy. Servers egressing traffic must pass through the proxy first. |
| **Reverse Proxy** | Ingress traffic trying to reach a collection of servers. Used by Ingress Controllers. |

#### NetworkPolicy
- Controls **ingress and egress traffic** to/from Pods
- Rules based on: Pod selectors, namespace selectors, IP blocks
- **Default: all traffic is allowed** (no NetworkPolicy = open)
- Requires a CNI plugin that supports NetworkPolicy (e.g., Calico, Cilium)
- If a NetworkPolicy selects a Pod, **only explicitly allowed traffic is permitted**

### 1.4 Configuration & API Model

#### ConfigMap
- Stores **non-sensitive** configuration data as key-value pairs
- Can be consumed as: environment variables, command-line arguments, or mounted as files/volumes
- Decouples configuration from container images

#### Secret
- Stores **sensitive data** (passwords, tokens, keys)
- Base64 encoded **by default** — this is **NOT encryption**
- Can be **encrypted at rest** using EncryptionConfiguration
- Best practices: use external secret managers (Vault, AWS Secrets Manager), enable encryption at rest, use RBAC to restrict access

#### Declarative API Model

| Field | Meaning |
|-------|---------|
| `apiVersion` | API group + version (e.g., `apps/v1`, `v1`, `batch/v1`) |
| `kind` | Type of resource (e.g., Pod, Deployment, Service) |
| `metadata` | Name, namespace, labels, annotations |
| `spec` | **Desired state** — what you want |
| `status` | **Current state** — what it actually is (managed by K8s) |

#### API Groups & Versioning

| Group | Resources |
|-------|-----------|
| **Core (v1)** | Pod, Service, ConfigMap, Secret, Namespace, Node, PV, PVC |
| **apps/v1** | Deployment, StatefulSet, DaemonSet, ReplicaSet |
| **batch/v1** | Job, CronJob |
| **networking.k8s.io/v1** | Ingress, NetworkPolicy |
| **rbac.authorization.k8s.io/v1** | Role, ClusterRole, RoleBinding, ClusterRoleBinding |

#### API Version Stability

| Stage | Meaning |
|-------|---------|
| **Alpha** (v1alpha1) | Experimental, may change/break, disabled by default |
| **Beta** (v1beta1) | Well-tested, enabled by default, may still change |
| **Stable** (v1) | GA, production-ready, backward-compatible |

### 1.5 Custom Resources & Operators

#### Custom Resource Definitions (CRDs)
- **Extend the Kubernetes API** with your own resource types
- No need to modify K8s source code
- Once created, custom resources can be managed via kubectl (get, describe, delete)
- Example: `kind: PostgresCluster` for a database operator

#### Operators
- **CRD + Custom Controller = Operator**
- Encodes **operational knowledge** (install, upgrade, backup, failover) into software
- Follows the controller pattern: watch → compare → act
- Examples: Prometheus Operator, cert-manager, database operators
- Discovery: **OperatorHub.io**

### 1.6 Namespaces & Multi-Tenancy

- **Logical isolation** within a cluster — NOT a strong security boundary
- Default namespaces: `default`, `kube-system`, `kube-public`, `kube-node-lease`
- Use with:
  - **RBAC** — control who can do what in which namespace
  - **ResourceQuota** — limit total resources (CPU, memory, object count) per namespace
  - **LimitRange** — set default/min/max resource requests/limits per Pod/container in a namespace
  - **NetworkPolicy** — control traffic between namespaces
- Some resources are **cluster-scoped** (nodes, PVs, ClusterRoles) — they don't belong to namespaces

### 1.7 Scheduling

#### Scheduler
- Watches for Pods with no assigned node
- Evaluates: resource requirements, affinity/anti-affinity, taints/tolerations, topology constraints
- Assigns Pod to best-fit node

#### Requests vs Limits

| Concept | Purpose |
|---------|---------|
| **Requests** | Guaranteed resources. Used for **scheduling decisions**. |
| **Limits** | Maximum cap. Container killed (OOMKilled) or throttled if exceeded. |
| No requests set | Pod may be scheduled anywhere. Risk of eviction under pressure. |

- **QoS Classes** (based on requests/limits):
  - **Guaranteed** — requests = limits for all containers
  - **Burstable** — at least one container has requests < limits
  - **BestEffort** — no requests or limits set (first to be evicted)

#### Placement Controls

| Mechanism | Purpose |
|-----------|---------|
| **nodeSelector** | Simple label-based node selection |
| **Node Affinity** | Expressive rules (required/preferred) for node selection |
| **Pod Affinity / Anti-Affinity** | Co-locate or spread Pods relative to other Pods |
| **Taints & Tolerations** | Nodes repel Pods unless Pod tolerates the taint. Used for dedicated nodes, special hardware. |

#### Scaling

| Type | Description |
|------|-------------|
| **Manual Scaling** | `kubectl scale deployment --replicas=N` |
| **HPA (Horizontal Pod Autoscaler)** | Scales **replica count** based on CPU/memory/custom metrics. Increases/decreases number of Pods. |
| **VPA (Vertical Pod Autoscaler)** | Adjusts resource **requests/limits** (CPU, memory) of existing Pods (awareness) |
| **Cluster Autoscaler** | Adds/removes **nodes** based on pending Pods and utilization. Also known as **Karpenter** (AWS alternative). |
| **KEDA** | Kubernetes Event-Driven Autoscaling. Scales based on external event sources (awareness) |

#### Scale vs Autoscale Commands

```bash
# Scale: Manually update replicas in deployment
kubectl scale --replicas=3 deploy/<app-name>

# Autoscale: Create a HorizontalPodAutoscaler
kubectl autoscale rc --min=1 --max=5 --cpu-percent=80
```

#### KEDA Event Sources (Awareness)
- Active MQ, Apache Kafka
- AWS CloudWatch / Kinesis / SQS Queue
- Azure App Insights / Blob Storage / Event Hubs / Log Analytics / Monitor / Pipelines
- New Relic, Prometheus, Datadog

#### PodDisruptionBudget (PDB)
- Limits the number of Pods that can be voluntarily disrupted at a time
- Ensures availability during maintenance, upgrades, node drains
- Defines `minAvailable` or `maxUnavailable`

### 1.8 Containerization

#### Image vs Container
- **Image** = Immutable template / blueprint (layers, read-only)
- **Container** = Running instance of an image (read-write layer on top)

#### Tags vs Digests
- **Tag** = Human-readable label (e.g., `v1.0`, `latest`). **Mutable** — can point to different images over time.
- **Digest** = SHA256 hash. **Immutable** — always refers to exact same image.
- Best practice: use **digests** in production for reproducibility

#### Container Registry
- Stores and distributes container images
- Examples: Docker Hub, **Harbor** (CNCF graduated), Azure Container Registry, Amazon ECR, Google Artifact Registry
- Pull policies: `Always`, `IfNotPresent`, `Never`

#### OCI (Open Container Initiative)
- Defines industry standards for containers:
  - **Image Specification** — how images are built and structured
  - **Runtime Specification** — how containers are run
  - **Distribution Specification** — how images are distributed
- Ensures portability across runtimes

#### OCI Container Runtimes

| Type | Examples | Description |
|------|----------|-------------|
| **Native** | **runC**, Crun | Low-level runtimes that create and run containers directly |
| **Virtual (Sandboxed)** | **gVisor**, nabla-containers, **kata-containers** | Provide stronger isolation between containers |

- **runC** is the reference implementation, used alongside containerd or CRI-O

#### Container Runtime Interface (CRI)
- Standard interface used for push/pull images, supervising containers, and more
- Two main CRI implementations:

| Runtime | Description |
|---------|-------------|
| **containerd** | Industry-standard runtime with emphasis on simplicity, robustness, and portability. Includes Docker's functionality for executing containers, low-level storage & image management. CNCF **Graduated**. |
| **CRI-O** | Alternative to Docker. Supports runC and kata-containers as runtimes. Lightweight, built specifically for K8s. |

- Images built with Docker aren't really "Docker images" — they are built in the standardized **OCI format**
- containerd makes it easier for K8s to access low-level container elements

#### CGroups (Control Groups)
- Linux kernel feature to apply resource limitations to processes:
  - **Resource limiting** — set CPU/memory caps
  - **Prioritization** — allocate more resources to certain groups
  - **Accounting** — measure resource usage
  - **Control** — freeze/restart groups of processes

#### Userspace vs Kernel Space
- OS segregates virtual memory into **kernel space** and **user space**
- **Kernel Space**: Privileged kernel operations
- **User Space**: Application software and other drivers

#### Multi-Container Pod Patterns

| Pattern | Description |
|---------|-------------|
| **Init Container** | Runs first, must complete before main containers start. Use: setup, dependency checking. |
| **Sidecar Container** | Runs alongside main container for the entire Pod lifecycle. Use: logging, proxying, syncing. |
| **Ambassador** | Proxy that simplifies access to external services. |
| **Adapter** | Transforms output from main container (e.g., log format normalization). |

#### Container Lifecycle Hooks
- **postStart** — executed immediately after container starts (no guarantee it runs before ENTRYPOINT)
- **preStop** — executed before container is terminated. Used for graceful shutdown.

#### Security Baseline
- Run as **non-root** user
- Use **read-only root filesystem** where possible
- **Immutable infrastructure** — don't patch running containers; replace them
- Drop all capabilities, add only what's needed
- Scan images for vulnerabilities

### 1.9 Kubernetes Implementations & Providers

#### Local Development Clusters

| Tool | Description | Use Case |
|------|-------------|----------|
| **Minikube** | Creates **single-node clusters**. Runs VM with control plane + worker processes. Uses Docker as container layer. | Development |
| **Kind** (Kubernetes in Docker) | Runs K8s clusters locally and in **CI pipelines** using Docker containers as "nodes". Primarily for testing. | Development / Testing |
| **K3s** | Lightweight K8s for production-level workloads. For low-resourced, remotely located **IoT & Edge devices**, bare metal. Created by **Rancher**. CNCF **Sandbox** project. | Production (Edge/IoT) |
| **K3D** | Lightweight wrapper that runs **K3s in a Docker container**. | Development |
| **MicroK8s** | Created by **Canonical** (installed via `snap`). Fast, self-healing, HA clusters. Modular design with enableable addons. Ideal for cloud, local dev, Edge/IoT. | Development / Production |

#### K3s vs K8s Key Differences
- K3s does not use kubelet — uses host machine mechanisms to run containers
- K3s uses kube-proxy to proxy **node** network connections (vs K8s proxying **container** connections)
- K3s has **more security** deployment features than K8s

#### Managed Kubernetes Providers
- **Google Kubernetes Engine (GKE)**
- **Amazon Elastic Kubernetes Service (EKS)**
- **Azure Kubernetes Service (AKS)**
- **IBM Cloud Kubernetes Service**
- **Oracle Container Engine for Kubernetes**
- **DigitalOcean Kubernetes (DOKS)**
- **Civo** (focused on just K8s)

#### Management Layers

Allow you to extend your control plane to multiple platforms:

| Platform | Description |
|----------|-------------|
| **RedHat OpenShift** | Platform as a Service for K8s. Extended kubectl via `oc` CLI. |
| **Rancher Kubernetes Engine (RKE)** | Runs within Docker containers. Works on bare metal. |
| **VMware Tanzu** | Enterprise K8s management platform |
| **Azure Arc** | Extends Azure management to any infrastructure |
| **Google Anthos** | Multi-cloud K8s management |
| **Platform9** | Managed K8s across any infrastructure |

---

## DOMAIN 2: CONTAINER ORCHESTRATION (28%)

### 2.1 Networking
- **Flat Pod networking model** — every Pod has its own IP, all Pods can reach each other
- **Service abstraction** — stable endpoint for a set of Pods
- **DNS discovery** — CoreDNS is the default DNS in Kubernetes
- **Ingress** — L7 HTTP/HTTPS routing with rules
- **NetworkPolicy** — firewall rules for Pod traffic (ingress/egress)
- **CNI plugins** — implement the network model (Calico, Cilium, Flannel)
- **Service Mesh** — dedicated infrastructure layer for service-to-service communication

### 2.2 Service Mesh

| Concept | Detail |
|---------|--------|
| **What** | Dedicated infrastructure layer for managing service-to-service communication |
| **How** | Sidecar proxy injected into each Pod (data plane) |
| **Data Plane** | Proxies (e.g., **Envoy**) handle actual traffic between services |
| **Control Plane** | Manages and configures proxies (policy, telemetry, certs) |
| **Features** | mTLS, traffic management, observability, retries, circuit breaking |
| **Tools** | **Istio**, **Linkerd** (CNCF graduated), Consul Connect, **Kuma** |

#### Envoy (Deep Dive)
- Self-contained process designed to run **alongside every application server**
- Can be installed as a VM or as a container (sidecar)
- CNCF **Graduated** project — L7 proxy and service mesh data plane
- In practice, you don't manually configure Envoy — the **service mesh control plane** installs and configures it
- Service mesh may provide a UI for Envoy configuration

### 2.3 Storage

#### Volume Types

| Type | Description |
|------|-------------|
| **Persistent Volumes** | Attach external storage to a Pod. Data **persists after Pod termination**. |
| **Ephemeral Volumes** | Only exist as long as the Pod exists. Intended for **temporary storage**. (emptyDir) |
| **Projected Volumes** | Map several existing volume sources into the **same directory**. |
| **Volume Snapshots** | Archive a volume's configuration and data for **rollbacks or backup**. |

#### Ephemeral Storage
- **emptyDir** — created when Pod is assigned to a node, deleted when Pod is removed. Use: scratch space, cache, inter-container sharing.
- **hostPath** — mounts a file/directory from the host node's filesystem. Use: access node-level logs. **Security risk** — avoid in production.
- ConfigMap / Secret volumes — mount config/secrets as files

#### Persistent Storage

| Resource | Purpose |
|----------|---------|
| **PersistentVolume (PV)** | Cluster-level storage resource. Provisioned by admin or dynamically. |
| **PersistentVolumeClaim (PVC)** | User request for storage. Binds to a PV. |
| **StorageClass** | Defines provisioner and parameters for **dynamic provisioning** of PVs. |

#### Access Modes

| Mode | Description |
|------|-------------|
| **ReadWriteOnce (RWO)** | Mounted read-write by a single node |
| **ReadOnlyMany (ROX)** | Mounted read-only by many nodes |
| **ReadWriteMany (RWX)** | Mounted read-write by many nodes |
| **ReadWriteOncePod (RWOP)** | Mounted read-write by a single Pod (K8s 1.27+) |

#### Reclaim Policies
- **Retain** — PV is kept after PVC is deleted (manual cleanup)
- **Delete** — PV is deleted when PVC is deleted
- **Recycle** — deprecated

#### CSI (Container Storage Interface)
- Standard interface for storage vendors to integrate with K8s
- Standardizes how orchestration systems access various storage providers
- Enables third-party storage plugins without modifying K8s core
- Examples: AWS EBS CSI, Azure Disk CSI, Ceph CSI

#### Kubernetes Backing Store & etcd
- Components of a K8s cluster each need a level of protection in case of disaster
- K8s resources are stored in **etcd** (can be backed by MariaDB)
- etcd is a strongly consistent, distributed key-value store for data accessed by distributed systems
- Things that use etcd: **Kubernetes**, **CoreDNS**, **Rook**

#### Key Storage Projects

| Project | Description |
|---------|-------------|
| **Rook** | Automates tasks of storage administrators. Turns distributed storage into self-managing, self-scaling, self-healing storage services. CNCF **Graduated**. |
| **MinIO** | High-performance, S3-compatible **object storage**. Native to K8s. Only object storage suite available on every cloud. Open-sourced under GNU license. |

### 2.4 Security

#### 4C's of Cloud Native Security

Each layer builds upon the next outermost layer:

| Layer | Key Concerns |
|-------|-------------|
| **Cloud** | Base layer. Managed infrastructure (CSPs) or self-managed (co-located servers, corporate datacenter). |
| **Cluster** | Controlling access to K8s API and kubelet. Protecting cluster components. RBAC, authentication, secrets management, Pod Security Standards, Network Policies, TLS for Ingress. |
| **Container** | Vulnerability scanning, image signing, disallowing privileged users, using runtimes with stronger isolation. |
| **Code** | Access over TLS only, limiting port ranges, 3rd-party dependency security, static code analysis, dynamic probing attacks. |

#### Infrastructure Security
- **API Server access**: Not publicly accessible on the internet. Controlled by restricted IP lists.
- **Node access**: Only accept connections from the control plane on specified ports and from NodePort/LoadBalancer services.
- **Cloud Provider API**: Grant different permissions to control plane vs nodes.
- **etcd access**: Limited to control plane only. Use etcd over TLS. Encrypt all storage at rest.

#### AAA (Authentication, Authorization, Accounting)

| Component | Description |
|-----------|-------------|
| **Authentication** (Identity) | Static passwords, one-time passwords, digital certificates, Basic Auth |
| **Authorization** (Permission) | RBAC (primary mode), ABAC, Webhook, Node authorization |
| **Accounting** (Audit Trail) | Audit policies define what to log. Audit backends define where logs are stored. |

#### Authentication (AuthN) — "Who are you?"
- Methods: X.509 client certificates, bearer tokens, OIDC, webhook token
- **ServiceAccounts** — identity for Pods/processes inside the cluster
  - Automatically mounted token (can be disabled)
  - Use specific ServiceAccounts per workload (not default)

#### Authorization (AuthZ) — "What can you do?"
- **RBAC** is the primary authorization mode
- Other modes: ABAC, Webhook, Node authorization

#### RBAC

| Resource | Scope | Purpose |
|----------|-------|---------|
| **Role** | Namespace | Defines permissions within a namespace |
| **ClusterRole** | Cluster-wide | Defines permissions across the cluster |
| **RoleBinding** | Namespace | Binds a Role/ClusterRole to users/groups/ServiceAccounts in a namespace |
| **ClusterRoleBinding** | Cluster-wide | Binds a ClusterRole to users/groups/ServiceAccounts cluster-wide |

- RBAC rules define: **verbs** (get, list, create, update, delete) on **resources** (pods, services, secrets) in **apiGroups**
- **Principle of Least Privilege** — grant only the minimum permissions necessary
- With K8s RBAC there are **only allow rules** — everything is **deny by default**
- Enable RBAC: `kube-apiserver --authorization-mode=RBAC`

#### RBAC Role Configuration Example
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]     # core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

#### Admission Controllers
- Intercept requests to the API server **after authentication and authorization, before persistence**
- **Validating** — accept or reject requests
- **Mutating** — modify requests (e.g., inject sidecar, add default labels)
- Key built-in controllers: LimitRanger, ResourceQuota, PodSecurity
- Webhook-based: **OPA/Gatekeeper**, Kyverno

#### Pod Security Standards (PSS) / Pod Security Admission (PSA)
- Replaced PodSecurityPolicy (removed in K8s 1.25)
- Three levels:
  - **Privileged** — no restrictions (unrestricted)
  - **Baseline** — minimally restrictive, prevents known privilege escalations
  - **Restricted** — heavily restricted, follows security best practices
- Applied via namespace labels

#### OPA (Open Policy Agent) / Gatekeeper
- **OPA** — general-purpose policy engine (CNCF graduated)
- **Gatekeeper** — K8s-native integration of OPA via admission webhooks
- Enforces policies: require labels, deny privileged containers, restrict image registries

#### Supply Chain Security
- **Image signing** — verify image integrity and provenance (Cosign, Notary/TUF)
- **Vulnerability scanning** — scan images for known CVEs (Trivy, Grype)
- **SBOM** (Software Bill of Materials) — inventory of components in an image
- **Trusted registries** — only pull from approved registries
- CNCF projects: **TUF** (The Update Framework, graduated), **Notary**, **in-toto**

#### Encryption
- **In transit** — mTLS between services (service mesh), TLS for API server
  - **TLS** (Transport Layer Security) / **SSL** (Secure Socket Layer)
- **At rest** — encrypt etcd data using EncryptionConfiguration, encrypt Secrets at rest
  - **AES** (Advanced Encryption Standard) / **RSA** (Rivest-Shamir-Adleman)

#### PKI (Public Key Infrastructure)
- Set of roles, hardware, software & procedures to create, manage, distribute, use, store, and revoke digital certificates
- **x.509** certificates used in SSL/TLS, HTTPS
- K8s provides `certificates.k8s.io` API for provisioning TLS certificates signed by the CA
- K8s requires PKI for several operations — stored in `/etc/kubernetes/pki`

#### Calico (CNI with Network Policy)
- Open-source network and network security solution for containers, VMs, and native host-based workloads
- Supports: Kubernetes, OpenShift, Mirantis Kubernetes Engine, OpenStack, Bare Metal
- Choice of dataplane: **Linux eBPF**, Standard Linux, Windows HNS

#### K8s Security Best Practices
1. Enable Kubernetes RBAC
2. Use third-party authentication for API server
3. Protect etcd with TLS, firewall, and encryption
4. Isolate Kubernetes nodes
5. Monitor network traffic to limit communication
6. Use process whitelisting
7. Turn on audit logging
8. Keep Kubernetes version up to date
9. Lock down kubelet

### 2.5 Troubleshooting

#### Pod Phases

| Phase | Meaning |
|-------|---------|
| **Pending** | Accepted but not running. Waiting for scheduling or image pull. |
| **Running** | Pod bound to a node, at least one container running. |
| **Succeeded** | All containers terminated successfully (exit code 0). |
| **Failed** | All containers terminated, at least one with failure. |
| **Unknown** | State cannot be determined (node communication issue). |

#### Common Failure Conditions

| Status | Likely Cause |
|--------|-------------|
| **CrashLoopBackOff** | Container crashes repeatedly. Check: app errors, misconfiguration, missing dependencies, OOMKilled. |
| **ImagePullBackOff** | Cannot pull container image. Check: image name/tag, registry authentication, network. |
| **Pending** | No suitable node. Check: insufficient resources, unsatisfied nodeSelector/affinity, unbound PVC, taints. |
| **OOMKilled** | Container exceeded memory limit. Increase limits or fix memory leak. |
| **CreateContainerConfigError** | Missing ConfigMap or Secret referenced by Pod. |
| **ErrImagePull** | One-time pull failure (becomes ImagePullBackOff if repeated). |

#### kubectl Troubleshooting Commands

| Command | Purpose |
|---------|---------|
| `kubectl get` | List resources, show status |
| `kubectl describe` | Detailed info + events for a resource |
| `kubectl logs` | View container logs (`-f` to follow, `--previous` for crashed container) |
| `kubectl exec` | Execute command inside a running container |
| `kubectl get events` | Cluster-wide events (scheduling, pulling, errors) |
| `kubectl top` | Resource usage (CPU/memory) — requires Metrics Server |
| `kubectl port-forward` | Forward local port to a Pod/Service for debugging |

#### BusyBox for Debugging
- BusyBox combines tiny versions of many common UNIX utilities into a single small executable
- Can be used to **interactively debug services** inside a cluster
- Commonly used as an ephemeral debugging Pod

#### Probes

| Probe | Purpose | Action on Failure |
|-------|---------|-------------------|
| **Liveness** | Is the container alive? | **Restart** the container |
| **Readiness** | Can the container serve traffic? | **Remove from Service** endpoints (stop sending traffic) |
| **Startup** | Has the container started successfully? | **Kill and restart** (protects slow-starting containers from liveness checks) |

- Probe types: **HTTP GET**, **TCP Socket**, **Exec command**, **gRPC**
- Key settings: `initialDelaySeconds`, `periodSeconds`, `failureThreshold`

---

## DOMAIN 3: CLOUD NATIVE APPLICATION DELIVERY (16%)

### 3.1 Application Delivery Fundamentals

#### CI/CD

| Stage | What | Key Point |
|-------|------|-----------|
| **CI (Continuous Integration)** | Build, test on every commit | Automated build + unit/integration tests |
| **CD (Continuous Delivery)** | Package ready for deployment, manual approval to prod | Artifact is always deployable |
| **CD (Continuous Deployment)** | Fully automated to production | No manual gate |

- CI/CD tools awareness: Jenkins, GitHub Actions, GitLab CI, Tekton (CNCF)

#### Deployment Strategies

| Strategy | Description | Risk |
|----------|-------------|------|
| **Rolling Update** | Gradually replace old Pods with new. **Default** in K8s Deployments. Zero-downtime. Deploys can be slow. | Breaking change affects partial traffic. Rollbacks are slow. |
| **Blue-Green** | Two identical environments. Switch traffic from blue (old) to green (new). Zero downtime. Instant rollback to previous infrastructure. | Doubles infrastructure cost. Slower to deploy but faster than canary. |
| **Canary** | Route small % of traffic to new version. Increase gradually if healthy. Fast rollout. No drop in availability. | More complex routing needed. Slow rollback. |
| **Recreate** | Kill all old, then create all new. **Downtime occurs.** Fast & simple. Rollback is not possible (re-deploy needed). | Simplest but causes outage. Ideal for non-production workloads. |
| **A/B Testing (Red/Black)** | Uses canary or Blue-Green method but serves new app to a **subset of users based on load balancing rules**. Intended for long periods. | Complex routing and user segmentation needed. |
| **Dark Launches** | Deploy new features to a **subset of users**. Toggle features on and off using feature flags. | Requires feature flag infrastructure. |

#### Deployment Strategy YAML

```yaml
# Rolling Update (default)
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2        # Max Pods above desired count during update
      maxUnavailable: 0  # Max Pods that can be unavailable

# Recreate
spec:
  replicas: 3
  strategy:
    type: Recreate
```

#### Rollout Commands
```bash
kubectl rollout history deploy/<name>   # View rollout history
kubectl rollout status deploy/<name>    # Check rollout status
kubectl rollout undo deploy/<name>      # Rollback to previous version
```

### 3.2 Helm

> **CRITICAL — Helm is heavily tested on KCNA**

| Concept | Detail |
|---------|--------|
| **What** | Package manager for Kubernetes. CNCF **Graduated** project. |
| **Chart** | Package of pre-configured K8s resource templates |
| **Release** | An instance of a chart deployed to a cluster |
| **Values** | Configuration parameters that customize a chart (`values.yaml`) |
| **Repository** | Where charts are stored and shared (e.g., Artifact Hub) |
| **Template** | K8s manifests with Go template syntax and values injection |

#### Key Helm Commands (concept awareness)
- `helm install` — deploy a chart as a release
- `helm upgrade` — update an existing release
- `helm rollback` — roll back to a previous release revision
- `helm uninstall` — remove a release
- `helm repo add/update` — manage chart repositories
- `helm search` — find charts in repos or Artifact Hub

#### Chart Structure
```
mychart/
  Chart.yaml          # Metadata (name, version, dependencies)
  values.yaml         # Default configuration values
  templates/          # K8s manifest templates
  charts/             # Dependency charts
```

### 3.3 Kustomize

| Concept | Detail |
|---------|--------|
| **What** | Template-free configuration customization for K8s manifests. Built into kubectl. |
| **Base** | Common set of resources shared across environments |
| **Overlay** | Environment-specific patches applied on top of base |
| **kustomization.yaml** | Declares resources, patches, and transformations |

- `kubectl apply -k <directory>` — apply Kustomize configuration
- No templating language — uses patches, strategic merge patches, JSON patches
- Helm uses templates; Kustomize uses overlays. **Different approaches, both valid.**

### 3.4 Infrastructure as Code (IaC)

- Scripts that **automate create, update, or destroy** cloud infrastructure
- IaC is a **blueprint of your infrastructure**
- Allows easy share, version, or inventory cloud infrastructure

#### IaC Approaches

| Approach | Tools |
|----------|-------|
| **Declarative** (describe desired state) | **Terraform**, Azure Blueprints, AWS CloudFormation |
| **Imperative** (describe steps to execute) | AWS Cloud Development Kit (CDK), **Pulumi** |

#### IaC for Kubernetes

| Scope | Tool |
|-------|------|
| Managing infra **of** the cluster (nodes, networking, cloud resources) | **Terraform** |
| Managing infra **in** the cluster (apps, configs, deployments) | **Helm** |

### 3.5 GitOps

| Principle | Detail |
|-----------|--------|
| **Git as Single Source of Truth** | All desired state is stored in Git |
| **Declarative** | System state is described declaratively |
| **Automated Reconciliation** | Agents continuously ensure cluster matches Git |
| **Pull-based Model** | Agent in cluster pulls desired state from Git (more secure, no external cluster access needed) |
| **Push-based Model** | CI/CD tool pushes changes to cluster (traditional approach) |

#### GitOps Tools

| Tool | Detail |
|------|--------|
| **ArgoCD** | CNCF **Graduated** project. Declarative GitOps CD for Kubernetes. Web UI, app-of-apps pattern. |
| **Flux** | CNCF **Graduated** project. GitOps toolkit for Kubernetes. Composable controllers. |

- Both ArgoCD and Flux implement the **pull-based** GitOps model
- They watch Git repos and reconcile cluster state automatically

### 3.6 Debugging Lifecycle

1. **Reproduce** — confirm the issue exists and is repeatable
2. **Isolate** — narrow down the root cause (which component, which Pod, which container)
3. **Fix** — apply the correction
4. **Rollback** — if fix fails, roll back to last known good state (Helm rollback, Deployment rollback)
5. **Verify** — confirm fix resolved the issue

#### Observability Signals for Debugging

| Signal | Description |
|--------|-------------|
| **Logs** | Discrete events with timestamps |
| **Metrics** | Numeric measurements over time |
| **Traces** | End-to-end request path across services |

- **Observability ≠ Monitoring** — Monitoring is a subset. Observability = ability to understand internal state from external outputs.

---

## DOMAIN 4: CLOUD NATIVE ARCHITECTURE (12%)

### 4.1 Cloud Native Principles

> **Cloud Native**: An architectural approach that emphasizes application workloads that are **Portable**, **Modular**, and **Isolatable** between different cloud providers.

#### Four Key Principles of Cloud Native
1. **Microservices** — small, independently deployable services
2. **Containerization** — lightweight, portable, consistent runtime environments
3. **Continuous Delivery** — automated build-test-deploy pipelines
4. **DevOps** — collaboration between development and operations teams

#### Cloud Service Providers Characteristics
- A **collection of cloud services**
- Strong application integration
- **Metered billing**
- Single unified API

#### Additional Cloud Native Principles
- **Loosely coupled systems** — services communicate via well-defined APIs
- **Microservices** — small, independently deployable services (vs monolith)
- **Containers** — lightweight, portable, consistent runtime environments
- **Immutable infrastructure** — replace, don't patch. Containers are rebuilt, not updated in-place.
- **Declarative APIs** — describe what, not how
- **Resilience** — design for failure (retries, circuit breakers, graceful degradation)
- **Elasticity** — scale up/down based on demand

#### 12-Factor App (Know These)
> Methodology for building cloud-native SaaS applications:

| Factor | Principle |
|--------|-----------|
| 1. Codebase | One codebase tracked in VCS, many deploys |
| 2. Dependencies | Explicitly declare and isolate dependencies |
| 3. Config | Store config in the environment (not in code) |
| 4. Backing Services | Treat backing services as attached resources |
| 5. Build, Release, Run | Strictly separate build and run stages |
| 6. Processes | Execute app as stateless processes |
| 7. Port Binding | Export services via port binding |
| 8. Concurrency | Scale out via the process model |
| 9. Disposability | Fast startup and graceful shutdown |
| 10. Dev/Prod Parity | Keep dev, staging, production as similar as possible |
| 11. Logs | Treat logs as event streams |
| 12. Admin Processes | Run admin/management tasks as one-off processes |

#### Key Architecture Concepts

| Concept | Description |
|---------|-------------|
| **Container Orchestration** | Automated management of containerized applications (scheduling, scaling, healing). Kubernetes is the standard. |
| **Service Mesh** | Infrastructure layer for secure, observable service-to-service communication (Istio, Linkerd) |
| **Serverless** | Run code without managing servers. Cloud provider handles scaling and infrastructure. Classification is **on a scale** (not boolean yes/no). |
| **Knative** | Kubernetes-based platform for serverless workloads (CNCF Incubating). Serving + Eventing. |
| **FaaS** | Function as a Service — event-triggered code execution (AWS Lambda, Azure Functions) |
| **API Gateway** | Single entry point for external clients. Handles routing, auth, rate limiting. |

#### Serverless Characteristics
- Highly scalable, highly available, highly durable
- Secure by default
- Billed based on business tasks
- **Can scale to zero** — you don't pay for idle servers
- Pay for value

#### FaaS Tools

| Tool | Description |
|------|-------------|
| **OpenFaaS** | Runs serverless functions **anywhere Docker runs** |
| **Faasd** | Lightweight version of OpenFaaS — doesn't need K8s, runs on a single under-powered machine |
| **Apache OpenWhisk** | Deploy to Kubernetes, Mesos, Docker Swarm |

#### Knative (Deep Dive)
- Defines its own K8s objects as **Custom Resource Definitions (CRDs)**
- Two main components:
  - **Knative Serving**: Take containerized code and deploy with ease. Scale to zero cost.
  - **Knative Eventing**: Trigger serverless functions based on K8s API events.
- Knative components: Service, Route, Configuration, Revision
- Uses its own CLI called **`kn`** alongside `kubectl`
- Considerations: Not a complete serverless solution. Does not offer full FaaS offerings.

> **Know the difference:** Knative vs OpenFaaS

### 4.2 Observability

> **Observability = ability to understand the internal state of a system by examining its outputs**

#### Three Pillars of Observability

| Pillar | Description | Tool Examples |
|--------|-------------|---------------|
| **Logs** | Timestamped records of discrete events | Fluentd, Fluent Bit, Loki |
| **Metrics** | Numeric measurements aggregated over time | **Prometheus**, Thanos |
| **Traces** | End-to-end path of a request across services | **Jaeger**, Zipkin, Tempo |

#### OpenTelemetry (OTel)
- **CNCF Incubating** project (second most active after K8s)
- Vendor-neutral standard for generating, collecting, and exporting **telemetry data** (logs, metrics, traces)
- Provides: SDKs, APIs, Collector, auto-instrumentation
- Replaces OpenTracing and OpenCensus (both deprecated)
- **Wire Protocol**: Refers to a way of getting data from point-to-point
- **Instrumentation**: Act of embedding a monitoring library into existing applications to capture monitoring data
- **OpenTelemetry Collector**: Agent installed on the target machine as a dedicated server. Vendor-agnostic way to **receive, process, and export** telemetry data.
  - Local collector agent is the **default location** to which instrumentation libraries export their data

#### Prometheus (Deep Dive)
- **CNCF Graduated** project — de facto monitoring standard
- Originally built at **SoundCloud**
- **Pull-based model** — scrapes metrics endpoints from targets
- Stores **time-series data**
- Values **reliability** — you can always view what statistics are available
- **Not the correct choice** if you need 100% accuracy (trades accuracy for availability)
- **PromQL** — query language for Prometheus data
- **Alertmanager** — handles alerts from Prometheus (routing, deduplication, grouping)
- **Metric types**: Counter (monotonic increase), Gauge (goes up/down), Histogram (distribution), Summary (quantiles)

#### Grafana
- Open-source analytics and interactive **visualization web application**
- Commonly used alongside time-series databases: **InfluxDB**, **Prometheus**, **Graphite**
- Not a CNCF project (but widely used with CNCF tools)

#### Traces & Spans (Deep Dive)

| Concept | Description |
|---------|-------------|
| **Trace** | Data/execution path through the system. Can be thought of as a **Directed Acyclic Graph (DAG)** of spans. |
| **Span** | A logical unit of work in Jaeger with an **operation name**, **start time**, and **duration**. |

#### Other Observability Tools

| Tool | Purpose | CNCF Status |
|------|---------|-------------|
| **Grafana** | Dashboards and visualization for metrics | Not a CNCF project (but widely used) |
| **Fluentd** | Log aggregation and forwarding | **CNCF Graduated** |
| **Fluent Bit** | Lightweight log processor | **CNCF Graduated** |
| **Jaeger** | Distributed tracing | **CNCF Graduated** |
| **Thanos** | Long-term scalable Prometheus storage | CNCF Incubating |
| **Cortex** | Multi-tenant Prometheus | CNCF Graduated |

#### SLI, SLO, SLA

| Term | Definition |
|------|------------|
| **SLI** (Service Level Indicator) | A quantitative metric of service quality (e.g., request latency, error rate, availability %) |
| **SLO** (Service Level Objective) | A target value for an SLI (e.g., 99.9% availability) |
| **SLA** (Service Level Agreement) | A contract with consequences if SLOs are not met (e.g., refund credits) |

### 4.3 Kubernetes System Logs & Testing

#### Kubernetes System Logs
- Include: HTTP access logs, Pod state changes, controller actions, scheduler decisions
- View with: `kubectl logs <pod-name> --all-containers=true`

#### Klog
- **Klog** is the Kubernetes logging library
- Generates log messages for K8s system components

#### Chaos Testing
- Building systems to **withstand and tolerate any kind of failure** by purposely introducing random failures in production
- Tools: **ChaosKube**, **TestKube**, **ChaosMonkey**

### 4.4 Cost Management

| Strategy | Description |
|----------|-------------|
| **Label Resources** | Visualize costs using Prometheus & Grafana |
| **Find Idle Resources** | Visualize idle CPU, memory, and storage |
| **Workload Rightsizing** | VPA (adjust CPU/memory) + HPA (add/remove Pods) |
| **Cluster Downsizing** | Cluster Autoscaler adds/removes nodes to meet demand |
| **Cost Tools** | Use free trials of tools like **KubeCost** |
| **Estimate Future Costs** | Load-testing tools: **SpeedScale**, **K6**, **JMeter**, **Gatling** |
| **Scale to Zero** | Eliminate utilization waste — scale down if no traffic |

### 4.5 CNCF Ecosystem

#### CNCF (Cloud Native Computing Foundation)
- **Linux Foundation** project founded in **2015**
- Operates **independent** of Linux Foundation
- Has its own **board members**, **conferences**, **certifications**, and collection of projects
- **Vendor-neutral** foundation for cloud-native open source projects
- Hosts Kubernetes and **150+ other projects**

#### CNCF Governance Bodies

| Body | Responsibility |
|------|----------------|
| **Governing Board** | Marketing, business oversight, budget decisions |
| **Technical Oversight Committee (TOC)** | Defining & maintaining technical vision. SIGs are part of TOC. |
| **End User Community (EUC)** | Providing feedback from startups to improve ecosystem. End User Groups & User Groups. |

#### CNCF Membership Tiers
- Platinum Members
- Gold Members
- Silver Members
- End User Members
- Academic & Non-Profit Members

#### CNCF Charter
- Big document defining and guiding CNCF: mission, organizational structure, policies, etc.

#### CNCF Values
- Faster is better than slow
- Open
- Fair
- Strong technical identity
- Clear boundaries
- Scalable
- Platform agnostic

#### End User Technology Radar
- Get feedback from community to find recommended and preferred tools
- Three levels: **Assess** (tried it, looks promising) → **Trial** (can have closer look) → **Adopt** (clearly recommend it)

#### Project Maturity Levels

| Level | Description | Examples |
|-------|-------------|---------|
| **Sandbox** | Early stage, experimental | New projects getting started |
| **Incubating** | Growing adoption, maturing | OpenTelemetry, Knative, Backstage, KEDA |
| **Graduated** | Proven production-ready, strong governance | Kubernetes, Prometheus, Envoy, containerd, CoreDNS, Fluentd, Jaeger, Helm, Harbor, Argo, Linkerd, OPA, TUF, Rook, Vitess, etcd, Flux |

#### Key CNCF Projects to Know

| Project | Purpose | Status |
|---------|---------|--------|
| **Kubernetes** | Container orchestration | Graduated |
| **Prometheus** | Monitoring and alerting | Graduated |
| **Envoy** | L7 proxy, service mesh data plane | Graduated |
| **CoreDNS** | DNS server for Kubernetes | Graduated |
| **containerd** | Container runtime | Graduated |
| **Fluentd** | Log aggregation | Graduated |
| **Jaeger** | Distributed tracing | Graduated |
| **Helm** | Package manager | Graduated |
| **Harbor** | Container registry | Graduated |
| **ArgoCD / Argo** | GitOps / workflows | Graduated |
| **Linkerd** | Service mesh | Graduated |
| **OPA** | Policy engine | Graduated |
| **etcd** | Key-value store | Graduated |
| **Flux** | GitOps toolkit | Graduated |
| **Falco** | Runtime security | Graduated |
| **TUF** | Secure software updates | Graduated |
| **Cilium** | eBPF-based networking and security | Graduated |
| **OpenTelemetry** | Observability framework | Incubating |
| **Knative** | Serverless for K8s | Incubating |
| **KEDA** | Event-driven autoscaling | Graduated |
| **Backstage** | Developer portal | Incubating |

#### CNCF Trail Map (Cloud Native Journey)
1. **Containerization** — package apps in containers
2. **CI/CD** — automate build, test, and deployment
3. **Orchestration** — Kubernetes
4. **Observability & Analysis** — monitoring, logging, tracing
5. **Service Mesh** — manage service communication
6. **Networking & Policy** — CNI, NetworkPolicy
7. **Distributed Database & Storage** — Vitess, Rook
8. **Messaging & Streaming** — NATS, CloudEvents
9. **Container Registry & Runtime** — Harbor, containerd
10. **Software Distribution** — Helm, Artifact Hub

#### Cloud Native Community
- **Open-source collaboration** — anyone can contribute
- **Governance** — TOC, SIGs (Special Interest Groups), Working Groups
- **KubeCon + CloudNativeCon** — flagship CNCF conference
- **Contributor Ladder** — member → reviewer → approver → maintainer
- **CNCF Landscape** — comprehensive interactive map of cloud native tools and projects (landscape.cncf.io)

#### Open Standards

Key open standards in cloud native ecosystem:

| Standard | Purpose |
|----------|---------|
| **OCI** (Open Container Initiative) | Image, Runtime, and Distribution specifications for containers |
| **CNI** (Container Network Interface) | Specification for writing plugins to configure networking |
| **CSI** (Container Storage Interface) | Standard interface for storage vendors to integrate with K8s |
| **CRI** (Container Runtime Interface) | Standard for container runtime communication with kubelet |
| **OpenTelemetry (OTel)** | Standardize telemetry data (metrics, logs, traces) generation and collection |

---

## EXAM TRAP SHEET — Common Trick Questions

| Trap | Correct Answer |
|------|---------------|
| "etcd stores container images" | **NO** — etcd stores cluster state/config. Images are in registries. |
| "kubelet runs on the control plane only" | **NO** — kubelet runs on **every node** (including control plane). |
| "Secrets are encrypted by default" | **NO** — base64 **encoded**, not encrypted. Encryption at rest must be configured. |
| "Ingress is a Service type" | **NO** — Ingress is a separate API resource. Service types are ClusterIP, NodePort, LoadBalancer, ExternalName. |
| "NetworkPolicy denies all traffic by default" | **NO** — default is ALL traffic allowed. NetworkPolicy must be explicitly created. |
| "Pods are rescheduled to another node" | **NO** — Pods are **replaced**, not rescheduled. Controllers create new Pods. |
| "Prometheus uses push-based model" | **NO** — Prometheus uses **pull-based** (scrapes endpoints). Pushgateway exists for short-lived jobs. |
| "DaemonSet maintains N replicas" | **NO** — DaemonSet runs **exactly one Pod per node**, not N replicas. |
| "kubectl create is declarative" | **NO** — `kubectl create` is imperative. `kubectl apply` is declarative. |
| "HPA scales nodes" | **NO** — HPA scales **Pod replicas**. Cluster Autoscaler scales nodes. |
| "ConfigMap data can't be updated" | **NO** — ConfigMaps can be updated, but Pods may need to be restarted to pick up changes. |
| "PodSecurityPolicy is the current standard" | **NO** — PSP was removed in K8s 1.25. Pod Security Standards/Admission replaced it. |
| "StatefulSet Pods can share the same PVC" | **NO** — each StatefulSet Pod gets its own PVC via volumeClaimTemplates. |
| "Service mesh requires Kubernetes" | **NO** — Service meshes can work outside K8s, but are most commonly used with it. |
| "Kube-proxy is a proxy that forwards traffic" | **TRICKY** — kube-proxy sets up network rules (iptables/IPVS) rather than acting as a traditional proxy. |
| "Liveness probe failure removes Pod from Service" | **NO** — Liveness failure **restarts** the container. **Readiness** failure removes from Service. |
| "Docker is required for Kubernetes" | **NO** — Kubernetes removed Docker support (dockershim) in 1.24. Uses containerd/CRI-O. |
| "Helm charts are only stored locally" | **NO** — Charts can be stored in **repositories** (Artifact Hub, OCI registries). |
| "ArgoCD pushes changes to the cluster" | **NO** — ArgoCD uses the **pull-based** GitOps model — it pulls from Git and applies. |
| "OpenTelemetry replaces Prometheus" | **NO** — OTel provides telemetry collection/export. Prometheus is a metrics backend. They complement each other. |
| "K3s uses kubelet" | **NO** — K3s does not use kubelet; it uses the host machine and its mechanisms to run containers. |
| "iptables is the future default for kube-proxy" | **NO** — **IPVS** is designed to be the future default. iptables is bottlenecked at ~5000 nodes. |
| "Endpoint Slices have no pod limit" | **NO** — Each Endpoint Slice has a limit of **100 pods**. |
| "Recreate strategy allows rollback" | **NO** — Recreate terminates all Pods first. **Rollback requires re-deployment.** Rolling Update is needed for easy rollback. |
| "Prometheus gives 100% accuracy" | **NO** — Prometheus values **reliability over accuracy**. Not the correct choice if 100% accuracy is required. |
| "Knative is a complete FaaS offering" | **NO** — Knative is not a complete serverless solution. It does not offer full FaaS capabilities like AWS Lambda. |
| "RBAC allows deny rules" | **NO** — K8s RBAC has **only allow rules**. Everything is deny by default. You add permissions, not remove them. |
| "Containers in Pod have different IPs" | **NO** — Containers in the same Pod share the **same IP address and port space**. They communicate via localhost. |

---

## FINAL CONFIDENCE CHECKLIST

Before the exam, you should be able to confidently explain each of these. Check off each one:

### Kubernetes Fundamentals
- [ ] VMs vs Containers vs Microservices fundamentals
- [ ] In-Tree vs Out-of-Tree plugins
- [ ] All control plane components and their exact roles
- [ ] All worker node components and their exact roles
- [ ] Kube-proxy modes (iptables, IPVS, Userspace)
- [ ] Kubelet deep dive (watches, configures, pulls, creates, runs)
- [ ] Pod vs Deployment vs ReplicaSet relationship
- [ ] StatefulSet vs Deployment (when to use which, ordering, unique names)
- [ ] DaemonSet vs Deployment
- [ ] Job and CronJob semantics
- [ ] Labels, Selectors (label/field/node), Annotations
- [ ] Endpoints and Endpoint Slices (100 pod limit)
- [ ] RPC and gRPC (K8s Pod communication)
- [ ] kubectl syntax (command, type, name, flags)
- [ ] All Service types including ExternalName and Headless
- [ ] Service Traffic Policies (External: Cluster/Local, Internal)
- [ ] Ingress vs Service (and that Ingress needs a Controller)
- [ ] ConfigMap vs Secret (and Secret encoding vs encryption)
- [ ] CRDs and Operators (what they are, why they matter)
- [ ] Namespaces + RBAC + ResourceQuota + LimitRange
- [ ] Requests vs Limits vs QoS classes
- [ ] nodeSelector vs Affinity vs Taints/Tolerations
- [ ] HPA vs VPA vs Cluster Autoscaler vs Karpenter
- [ ] KEDA event sources
- [ ] Scale vs Autoscale commands
- [ ] Init containers vs Sidecar vs Ambassador vs Adapter
- [ ] Container image tags vs digests
- [ ] Imperative vs Declarative management
- [ ] API groups, versions, and stability levels
- [ ] OCI standards and OCI runtime types (native vs virtual)
- [ ] CRI, containerd, CRI-O differences
- [ ] CGroups and Userspace vs Kernel space
- [ ] Minikube vs Kind vs K3s vs K3D vs MicroK8s
- [ ] Managed K8s providers (GKE, EKS, AKS, etc.)
- [ ] Management layers (OpenShift, Rancher, Tanzu, Arc)

### Container Orchestration
- [ ] Flat Pod networking model + CNI
- [ ] Four types of network communication (C2C, P2P, P2S, E2S)
- [ ] NAT, Veths, Netfilter, iptables vs IPVS
- [ ] Proxy types (kubectl proxy, apiserver proxy, kube-proxy, cloud LB)
- [ ] Forward proxy vs Reverse proxy
- [ ] CoreDNS (FQDN, plugins, resolv.conf)
- [ ] NetworkPolicy default behavior and usage
- [ ] Calico (eBPF, Standard Linux, Windows HNS dataplanes)
- [ ] PV vs PVC vs StorageClass lifecycle
- [ ] Volume types (Persistent, Ephemeral, Projected, Snapshots)
- [ ] Access modes (RWO, ROX, RWX, RWOP)
- [ ] CSI concept
- [ ] etcd backing store, Rook, MinIO
- [ ] 4C's of Cloud Native Security (Cloud, Cluster, Container, Code)
- [ ] AAA (Authentication, Authorization, Accounting)
- [ ] AuthN vs AuthZ
- [ ] RBAC (Role, ClusterRole, RoleBinding, ClusterRoleBinding) — deny by default
- [ ] ServiceAccounts
- [ ] Admission Controllers (Validating, Mutating)
- [ ] Pod Security Standards (Privileged, Baseline, Restricted)
- [ ] OPA/Gatekeeper
- [ ] PKI and x.509 certificates in K8s
- [ ] Supply chain security (signing, scanning, SBOM)
- [ ] In-transit encryption (TLS/SSL) vs At-rest encryption (AES/RSA)
- [ ] K8s Security Best Practices (9 items)
- [ ] Pod phases and common failure statuses
- [ ] CrashLoopBackOff / ImagePullBackOff causes
- [ ] Liveness vs Readiness vs Startup probes
- [ ] Service Mesh (data plane vs control plane, Envoy, Istio, Linkerd, Kuma)
- [ ] Envoy deep dive (sidecar, L7 proxy, auto-configured by control plane)
- [ ] BusyBox for debugging

### Cloud Native Application Delivery
- [ ] CI vs CD (Delivery) vs CD (Deployment)
- [ ] Rolling, Blue-Green, Canary, Recreate strategies
- [ ] A/B Testing (Red/Black) and Dark Launches
- [ ] Deployment strategy YAML (maxSurge, maxUnavailable)
- [ ] Rollout commands (history, status, undo)
- [ ] Helm (charts, releases, values, repos, commands)
- [ ] Kustomize (base, overlay, no templating)
- [ ] Infrastructure as Code (Terraform vs Helm scope)
- [ ] Declarative IaC (Terraform, CloudFormation) vs Imperative (CDK, Pulumi)
- [ ] GitOps principles (pull-based vs push-based)
- [ ] ArgoCD and Flux (CNCF Graduated, pull-based)
- [ ] Debug lifecycle (reproduce, isolate, fix, rollback)

### Cloud Native Architecture
- [ ] Cloud Native 4 key principles (Microservices, Containers, CD, DevOps)
- [ ] Cloud Service Provider characteristics
- [ ] Three pillars of observability (logs, metrics, traces)
- [ ] Prometheus (pull model, PromQL, Alertmanager, metric types, built at SoundCloud)
- [ ] Grafana (visualization with InfluxDB, Prometheus, Graphite)
- [ ] OpenTelemetry (Collector, Wire Protocol, Instrumentation)
- [ ] Jaeger (tracing, traces & spans, DAG), Fluentd (logging)
- [ ] SLI, SLO, SLA definitions
- [ ] Kubernetes System Logs and Klog
- [ ] Chaos Testing (ChaosKube, TestKube, ChaosMonkey)
- [ ] Cost Management strategies (KubeCost, rightsizing, scale to zero)
- [ ] 12-Factor App methodology
- [ ] Cloud native principles (microservices, immutable infrastructure, etc.)
- [ ] Serverless characteristics (scale to zero, pay for value)
- [ ] FaaS tools (OpenFaaS, Faasd, Apache OpenWhisk)
- [ ] Knative deep dive (Serving, Eventing, CRDs, kn CLI)
- [ ] CNCF project maturity levels (Sandbox → Incubating → Graduated)
- [ ] Key CNCF projects and their purposes
- [ ] CNCF governance bodies (Governing Board, TOC, EUC)
- [ ] CNCF membership tiers, charter, values
- [ ] End User Technology Radar (Assess → Trial → Adopt)
- [ ] Open Standards (OCI, CNI, CSI, CRI, OTel)
- [ ] CNCF community (TOC, SIGs, KubeCon, Contributor Ladder)

---

> **Study Priority Order:** Kubernetes Fundamentals (44%) → Container Orchestration (28%) → Application Delivery (16%) → Architecture (12%)
>
> **Time allocation:** Spend ~50% of study time on Domain 1, ~25% on Domain 2, ~15% on Domain 3, ~10% on Domain 4.


# Mindmap

### Storage
```                                            KUBERNETES STORAGE
                                                    │
              ┌─────────────────┬───────────────────┼───────────────────┬──────────────────┐
              │                 │                    │                  │                  │
        CORE OBJECTS      PROVISIONING        ACCESS MODES       RECLAIM POLICIES    STORAGE TYPES
              │                 │                    │                  │                  │
       ┌──────┼──────┐    ┌────┴─────┐        ┌────┼────┐        ┌──────┼──────┐           │
       │      │      │    │          │        │    │    │        │      │      │           │
       PV    PVC     SC  Static   Dynamic    RWO  RWX  ROX    Retain Delete Recycle        │
                     │    │          │        │    │    │        │      │      │           │
                     │   Admin     Auto       │    │    │        │      │  (deprecated)    │
                     │  creates  created      │    │    │      Keep   Delete               │
                     │  PV       via SC       │    │    │      data    PV                  │
                     │  manually              │    │    │      after   + data              │
                     ├── provisioner          │    │    │      PVC     when PVC            │
                     ├── parameters           │    │    │      delete  deleted             │
                     ├── reclaimPolicy       Read Read Read                                │
                     ├── volumeBindingMode   Write Many Only                               │
                     │     ├─ Immediate      Once (multi (multi                            │
                     │     └─ WaitForFirst   (single node) node)                           │
                     │        Consumer        node)                                        │
                     └── allowVolume                                                       │
                           Expansion                                                       │
                           (true/false)                                                    │
                                                                                           │
       PersistentVolumeClaim                                                               │
              │                                                                            │
              ├── accessModes                                         ┌────────────────────┘
              ├── resources.requests                                  │
              │     └── storage: 5Gi                       STORAGE TYPES / VOLUME PLUGINS
              ├── storageClassName                                   │
              ├── selector                              ┌────────────┼────────────────────────────┐
              │     ├── matchLabels                     │            │                            │
              │     └── matchExpressions          LOCAL / NODE  NETWORK / CLOUD          SPECIAL PURPOSE
              └── volumeMode                            │            │                            │
                    ├── Filesystem (default)            │            │                            │
                    └── Block (raw block)               │            │                            │
                                                        │            │                            │
       PersistentVolume                                 │            │                            │
              │                                         │            │                            │
              ├── capacity                     emptyDir (pod    awsElasticBlockStore     configMap (inject
              ├── accessModes                   lifetime,        (EBS)                    config as files)
              ├── persistentVolumeReclaimPolicy  tmpfs          gcePersistentDisk         secret (inject
              ├── storageClassName               optional)       (GCE PD)                 secrets as files)
              ├── mountOptions                 hostPath (node   azureDisk                downwardAPI (pod/
              ├── nodeAffinity                  directory       azureFile                  container info)
              └── volumeMode                    mounted)       nfs                       projected (combine
                                               local (PV       iscsi                      multiple sources)
                                                backed by      cephfs
                                                local disk)    glusterfs (deprecated)
                                                               fc (Fibre Channel)
                                                                        │
                                                               CSI (Container Storage Interface)
                                                                        │
                                                               ├── Standard plugin interface
                                                               ├── Any vendor can implement
                                                               ├── Replaces in-tree plugins
                                                               └── Examples:
                                                                     ├── ebs.csi.aws.com
                                                                     ├── pd.csi.storage.gke.io
                                                                     ├── disk.csi.azure.com
                                                                     └── csi.vsphere.vmware.com


──────────────────────────────────────────────────────────────────────────────────────────────────

  VOLUME LIFECYCLE                    BINDING LOGIC                      PROVISIONER TYPES
        │                                  │                                     │
  ┌─────┼──────┐              ┌────────────┼─────────────┐              ┌────────┴────────┐
  │     │      │              │            │             │              │                 │
Provision Bind Reclaim     By Size    By Access    By StorageClass    In-Tree           CSI
  │     │      │            PV >=     Mode Match    Name Match        Plugins          Drivers
  │     │      │           PVC size   (RWO=RWO)    (SC name must       │                 │
Static/ PV↔PVC Retain/    (1Gi PV    (RWX=RWX)     match or both    AWS EBS        ebs.csi.aws.com
Dynamic matched Delete/    fits                      empty)          GCE PD     pd.csi.storage.gke.io
               Recycle    500Mi PVC)                                 Azure      disk.csi.azure.com


──────────────────────────────────────────────────────────────────────────────────────────────────

  EPHEMERAL VOLUMES                   VOLUME EXPANSION                   VOLUME SNAPSHOTS
        │                                    │                               (CSI only)
  ┌─────┼──────┐                    ┌────────┴────────┐                       │
  │     │      │                    │                 │              ┌────────┼──────────┐
emptyDir configMap secret         Online           Offline           │        │          │
  │      │      │              (expand while   (must delete    VolumeSnapshot │    VolumeSnapshot
(dies  (readonly (readonly      pod running)    pod first)      Class         │     Content
 with   mounted)  mounted)          │                │           │       VolumeSnapshot
 pod)                        allowVolumeExpansion: true          │            │
                             (set in StorageClass)               │            │
                                                                 ├── Point-in-time copy
                                                                 ├── Restore to new PVC
                                                                 └── Requires CSI driver
```
