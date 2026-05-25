
## Kubernetes API Versioning

`v2beta3` means:
- **v2** — second major iteration of this API (breaking changes from v1)
- **beta** — feature is well-tested but not yet stable; schema may still change
- **3** — third beta release in this v2 generation

---

## API Version Quick Reference

| Version Pattern | Stability | Guarantees | Example |
|---|---|---|---|
| `v1` | **Stable (GA)** | No breaking changes; supported long-term | `v1` (Pods, Services) |
| `v2` | **Stable (GA)** | Same as v1 but second major revision | `autoscaling/v2` (HPA) |
| `v1alpha1` | **Alpha** | May be removed anytime; off by default | `batch/v1alpha1` |
| `v1alpha2` | **Alpha** | Second alpha iteration, same instability | `resource.k8s.io/v1alpha2` |
| `v1beta1` | **Beta** | Enabled by default; schema may change | `networking.k8s.io/v1beta1` |
| `v1beta2` | **Beta** | Second beta iteration, more refined | `autoscaling/v2beta2` (old HPA) |
| `v2beta1` | **Beta** | Beta on second major revision | `autoscaling/v2beta1` (old HPA) |
| `v2beta3` | **Beta** | Third beta of second major revision | (hypothetical/internal use) |

---

## The Lifecycle Rule

$$\text{alpha} \rightarrow \text{beta} \rightarrow \text{stable (GA)}$$

| Stage | Risk | Default enabled? | Can be deleted? |
|---|---|---|---|
| `alpha` | High | No | Yes, anytime |
| `beta` | Medium | Yes | Yes, with deprecation notice |
| stable (no suffix) | Low | Yes | Never without major version bump |

**Key memory trick:** The number after `v` is the **major API revision**, and the number after `alpha`/`beta` is the **iteration count** within that stage — like `v1beta1 → v1beta2 → v1 (GA)`.



## Kubernetes API Groups — Complete Reference

Kubernetes organizes all its resources under **API groups**. There are two tiers:

---

### Tier 1 — Core Group (`/api/v1`)
No group name. These are the foundational primitives:

| Resource | Abbrev |
|---|---|
| Pod | `po` |
| Service | `svc` |
| ConfigMap | `cm` |
| Secret | — |
| Node | `no` |
| Namespace | `ns` |
| PersistentVolume | `pv` |
| PersistentVolumeClaim | `pvc` |
| ServiceAccount | `sa` |
| Endpoints | `ep` |
| ResourceQuota | `quota` |
| LimitRange | — |

> **Memory trick:** If you can `kubectl get <it>` without thinking twice, it's probably core (`/api/v1`).

---

### Tier 2 — Named Groups (`/apis/<group>/<version>`)

#### Workloads
| Group | Resources |
|---|---|
| `apps` | Deployment, ReplicaSet, StatefulSet, DaemonSet |
| `batch` | Job, CronJob |

#### Networking
| Group | Resources |
|---|---|
| `networking.k8s.io` | Ingress, IngressClass, NetworkPolicy |
| `discovery.k8s.io` | EndpointSlice |

#### Storage
| Group | Resources |
|---|---|
| `storage.k8s.io` | StorageClass, VolumeAttachment, CSIDriver, CSINode |

#### Scaling & Metrics
| Group | Resources | Provider |
|---|---|---|
| `autoscaling` | HorizontalPodAutoscaler | core k8s |
| `metrics.k8s.io` | NodeMetrics, PodMetrics | **metrics-server** (add-on) |
| `custom.metrics.k8s.io` | Any custom metric (e.g. `http_requests`) | **Prometheus Adapter** |
| `external.metrics.k8s.io` | Cloud/external metrics (e.g. SQS queue depth) | Cloud adapters (Datadog, etc.) |

> **cAdvisor** is NOT a k8s API group. It's embedded in `kubelet` and exposed at:
> `GET /api/v1/nodes/{node}/proxy/metrics/cadvisor`

#### Security & RBAC
| Group | Resources |
|---|---|
| `rbac.authorization.k8s.io` | Role, ClusterRole, RoleBinding, ClusterRoleBinding |
| `authentication.k8s.io` | TokenReview |
| `authorization.k8s.io` | SubjectAccessReview, LocalSubjectAccessReview |
| `certificates.k8s.io` | CertificateSigningRequest |

#### Policy & Scheduling
| Group | Resources |
|---|---|
| `policy` | PodDisruptionBudget |
| `scheduling.k8s.io` | PriorityClass |
| `flowcontrol.apiserver.k8s.io` | FlowSchema, PriorityLevelConfiguration |

#### Extensibility
| Group | Resources |
|---|---|
| `apiextensions.k8s.io` | **CustomResourceDefinition (CRD)** |
| `admissionregistration.k8s.io` | MutatingWebhookConfiguration, ValidatingWebhookConfiguration |

#### Cluster Infrastructure
| Group | Resources |
|---|---|
| `coordination.k8s.io` | Lease (used for leader election) |
| `node.k8s.io` | RuntimeClass |
| `events.k8s.io` | Event (newer structured events) |

#### Dynamic Resource Allocation (newer, 1.26+)
| Group | Resources |
|---|---|
| `resource.k8s.io` | ResourceClaim, ResourceClass, ResourceClaimTemplate |

---

### How to Remember the Structure

```
kubectl api-resources --api-group=<group>   # list resources in a group
kubectl api-versions                         # list all groups + versions
```

**The naming pattern:**
```
<domain>.<category>.k8s.io/<version>
    │         │
    │         └── what it governs (networking, storage, rbac...)
    └── who provides it (empty = core k8s team)
```

**For metrics specifically (HPA scale chain):**
```
HPA
 ├── CPU/Memory  →  metrics.k8s.io          (metrics-server)
 ├── App metrics →  custom.metrics.k8s.io   (Prometheus Adapter)
 └── Cloud queue →  external.metrics.k8s.io (cloud adapters)
```


## Kubernetes Scaling — Complete Overview

### The Full Landscape

Kubernetes scaling operates at **3 levels**: Pod, Node, and Workload scheduling.

---

### Level 1: Pod-Level Scaling (What runs inside a node)

| Type | Acronym | What it does |
|---|---|---|
| **Horizontal Pod Autoscaler** | HPA | Adds/removes **pod replicas** based on CPU/memory/custom metrics |
| **Vertical Pod Autoscaler** | VPA | Adjusts **CPU & memory requests/limits** on existing pods |
| **KEDA** | KEDA | Extends HPA — scales pods based on **external events** (Kafka lag, SQS queue depth, Redis, etc.) |
| **Multidimensional Pod Autoscaler** | MPA | Google GKE-specific — combines VPA + HPA together |

---

### Level 2: Node-Level Scaling (The machines that run pods)

| Type | Acronym | What it does |
|---|---|---|
| **Cluster Autoscaler** | CA | Adds/removes **nodes** when pods are unschedulable or nodes are underutilized |
| **Karpenter** | — | AWS-native node provisioner — faster, more flexible than CA; provisions nodes in seconds |
| **Node Auto Provisioner** | NAP | GKE's Karpenter equivalent |

---

### Level 3: Scheduling & Throughput Scaling

| Type | What it does |
|---|---|
| **Proportional Autoscaler** | Scales system components (like `kube-dns`) proportionally to cluster size |
| **Scheduled Scaling** | Cron-based scaling (via KEDA's cron scaler or external tools like Argo Rollouts) |

---

### How to Remember Them

Use the mnemonic **"HVCK — Pod, Node, Schedule"**:

```
H — HPA       (Horizontal: more pods)
V — VPA       (Vertical: bigger pods)
C — KEDA      (event-driven: smarter pods)  ← K sounds like C
K — Cluster Autoscaler + Karpenter (nodes)
```

Or think of it as **3 questions**:

```
1. "How many pods?"   → HPA, KEDA
2. "How big is each pod?"  → VPA
3. "How many nodes?"  → Cluster Autoscaler, Karpenter
```

---

### Quick Comparison: When to Use What

```
External event (queue, stream)?  → KEDA
Traffic/CPU spike?               → HPA
OOM errors / resource waste?     → VPA
Pods pending, no space?          → Cluster Autoscaler
Need nodes in <30s?              → Karpenter
```

---

### HPA vs KEDA vs VPA vs CA — One-liner each

- **HPA** — "Scale out when busy" (reactive, metric-based)
- **VPA** — "Scale up when cramped" (right-size resources)
- **KEDA** — "Scale from zero on events" (event-driven, supports scale-to-zero)
- **CA** — "Add machines when pods can't fit" (infrastructure layer)
- **Karpenter** — "Add the *right* machine instantly" (smarter CA)

---

### Important Conflict Note

> **VPA + HPA cannot both manage CPU/memory on the same pod** — they fight each other. Use VPA for resource sizing in staging, HPA for scaling in production. Or use KEDA with custom metrics + VPA together safely.


## kubectl Commands — Grouped by Relation

---

### 1. Create / Apply Resources
| Command | What it does |
|---|---|
| `kubectl create -f file.yaml` | Creates a resource from a file |
| `kubectl apply -f file.yaml` | Creates **or updates** a resource (idempotent) |
| `kubectl run my-pod --image=nginx` | Creates a pod directly |

**Memory tip:** Think **"birth"** — `create` gives birth once, `apply` gives birth or updates.

---

### 2. View / Inspect Resources
| Command | What it does |
|---|---|
| `kubectl get pods` | List pods |
| `kubectl get all` | List all resources |
| `kubectl describe pod my-pod` | Detailed info about a resource |
| `kubectl explain deployment` | Show API fields for a resource type |
| `kubectl logs my-pod` | View logs from a pod |
| `kubectl top pod` | Show CPU/memory usage |

**Memory tip:** Think **"eyes"** — get, describe, explain, logs — all for *looking*.

---

### 3. Update / Modify Resources
| Command | What it does |
|---|---|
| `kubectl set image deployment/my-app c=img:v2` | Update container image in-place |
| `kubectl set env deployment/my-app KEY=val` | Update environment variables |
| `kubectl set resources deployment/my-app --limits=cpu=200m` | Update resource limits |
| `kubectl edit deployment my-app` | Open resource in editor to modify |
| `kubectl patch deployment my-app -p '{...}'` | Apply a partial JSON/YAML patch |
| `kubectl scale deployment my-app --replicas=5` | Change replica count |
| `kubectl label pod my-pod env=prod` | Add/update a label |
| `kubectl annotate pod my-pod note=test` | Add/update an annotation |

**Memory tip:** Think **"surgery"** — `set`, `edit`, `patch`, `scale` all *modify without rebuilding*.

---

### 4. Replace / Delete Resources
| Command | What it does |
|---|---|
| `kubectl replace -f file.yaml` | Replaces a resource (must already exist) |
| `kubectl replace --force -f file.yaml` | Deletes then re-creates (destructive) |
| `kubectl delete pod my-pod` | Delete a resource |
| `kubectl delete -f file.yaml` | Delete resources defined in a file |

**Memory tip:** Think **"demolish"** — `replace --force` and `delete` tear things down.

---

### 5. Rollout Management
| Command | What it does |
|---|---|
| `kubectl rollout status deployment/my-app` | Watch rollout progress |
| `kubectl rollout history deployment/my-app` | View revision history |
| `kubectl rollout undo deployment/my-app` | Roll back to previous version |
| `kubectl rollout undo deployment/my-app --to-revision=2` | Roll back to a specific version |
| `kubectl rollout restart deployment/my-app` | Restart pods with a rolling update |
| `kubectl rollout pause deployment/my-app` | Pause a rollout |
| `kubectl rollout resume deployment/my-app` | Resume a paused rollout |

**Memory tip:** Think **"time machine"** — `rollout` controls *when and how changes roll forward or back*.

---

### 6. Execute / Debug
| Command | What it does |
|---|---|
| `kubectl exec -it my-pod -- bash` | Open a shell inside a pod |
| `kubectl exec my-pod -- ls /app` | Run a single command in a pod |
| `kubectl cp my-pod:/app/file ./file` | Copy files to/from a pod |
| `kubectl port-forward pod/my-pod 8080:80` | Forward local port to pod port |

**Memory tip:** Think **"enter the building"** — you're going *inside* the pod.

---

### 7. Namespaces & Context
| Command | What it does |
|---|---|
| `kubectl config get-contexts` | List all contexts (clusters) |
| `kubectl config use-context my-ctx` | Switch active context |
| `kubectl config set-context --current --namespace=dev` | Set default namespace |
| `kubectl get pods -n kube-system` | Run command in a specific namespace |
| `kubectl get pods --all-namespaces` | List pods in every namespace |

**Memory tip:** Think **"GPS"** — context and namespace tell kubectl *where* to operate.

---

### 8. Expose / Networking
| Command | What it does |
|---|---|
| `kubectl expose deployment my-app --port=80` | Create a Service for a deployment |
| `kubectl get svc` | List services |
| `kubectl get ingress` | List ingress rules |

**Memory tip:** Think **"open the door"** — expose makes a deployment reachable.

---

## Master Memory Strategy

Use this sentence to remember the *lifecycle order*:

> **"Create → View → Update → Rollout → Debug → Expose"**

| Phase | Commands |
|---|---|
| **C**reate | `create`, `apply`, `run` |
| **V**iew | `get`, `describe`, `logs`, `top` |
| **U**pdate | `set`, `edit`, `patch`, `scale` |
| **R**ollout | `rollout status/undo/restart` |
| **D**ebug | `exec`, `cp`, `port-forward` |
| **E**xpose | `expose`, `get svc` |

A pod's life follows **CVRDE** — like a car (Create, View, Revise, Debug, Expose).
