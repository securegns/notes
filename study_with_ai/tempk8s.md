# KCNA Exam — Categorised Study Guide

> **270 of 300 questions captured** (Q271–Q300 were not available in source material)  
> Each question shows all options and the correct answer.

---

## Quick Index

| # | Category | Count | Questions |
|---|----------|-------|-----------|
| 1 | [Kubernetes Architecture & Core Components](#1-kubernetes-architecture--core-components) | 20 | Q3 Q4 Q40 Q44 Q46 Q58 Q61 Q74 Q103 Q121 Q129 Q142 Q148 Q156 Q163 Q169 Q170 Q187 Q220 Q234 |
| 2 | [Workloads](#2-workloads) | 30 | Q2 Q18 Q21 Q23 Q37 Q51 Q56 Q67 Q70 Q83 Q88 Q94 Q107 Q114 Q120 Q122 Q138 Q140 Q153 Q161 Q167 Q176 Q180 Q182 Q188 Q190 Q193 Q210 Q227 Q238 |
| 3 | [Services & Networking](#3-services--networking) | 34 | Q7 Q10 Q29 Q38 Q39 Q48 Q50 Q55 Q60 Q62 Q66 Q68 Q73 Q77 Q81 Q85 Q98 Q104 Q113 Q130 Q134 Q154 Q178 Q199 Q209 Q212 Q213 Q221 Q225 Q233 Q246 Q250 Q264 Q269 |
| 4 | [Storage](#4-storage) | 10 | Q32 Q78 Q102 Q132 Q150 Q155 Q219 Q236 Q258 Q263 |
| 5 | [Configuration, Secrets & Labels](#5-configuration-secrets--labels) | 11 | Q9 Q54 Q65 Q106 Q164 Q165 Q203 Q204 Q228 Q265 Q270 |
| 6 | [Security & RBAC](#6-security--rbac) | 17 | Q5 Q14 Q26 Q86 Q116 Q135 Q137 Q157 Q183 Q195 Q202 Q208 Q215 Q243 Q244 Q251 Q268 |
| 7 | [Observability & Monitoring](#7-observability--monitoring) | 14 | Q36 Q53 Q57 Q91 Q110 Q111 Q136 Q147 Q166 Q192 Q196 Q200 Q214 Q262 |
| 8 | [Scheduling & Autoscaling](#8-scheduling--autoscaling) | 19 | Q6 Q13 Q52 Q90 Q100 Q112 Q119 Q128 Q146 Q152 Q194 Q216 Q217 Q226 Q235 Q241 Q242 Q249 Q259 |
| 9 | [kubectl Commands & Troubleshooting](#9-kubectl-commands--troubleshooting) | 28 | Q20 Q45 Q49 Q63 Q71 Q75 Q76 Q84 Q87 Q97 Q124 Q126 Q159 Q168 Q201 Q206 Q211 Q229 Q230 Q231 Q232 Q237 Q240 Q245 Q253 Q257 Q261 Q267 |
| 10 | [Helm](#10-helm) | 6 | Q19 Q117 Q145 Q223 Q239 Q248 |
| 11 | [GitOps & CI/CD](#11-gitops--cicd) | 13 | Q8 Q22 Q101 Q127 Q131 Q144 Q158 Q171 Q198 Q254 Q256 Q260 Q266 |
| 12 | [Service Mesh](#12-service-mesh) | 9 | Q28 Q31 Q64 Q151 Q173 Q197 Q205 Q207 Q255 |
| 13 | [Container Runtime & Images](#13-container-runtime--images) | 14 | Q1 Q27 Q47 Q89 Q93 Q105 Q109 Q115 Q133 Q139 Q218 Q222 Q247 Q252 |
| 14 | [Cloud Native, CNCF & Open Source](#14-cloud-native-cncf--open-source) | 31 | Q12 Q15 Q16 Q17 Q25 Q30 Q34 Q41 Q42 Q69 Q72 Q79 Q80 Q82 Q92 Q95 Q108 Q123 Q125 Q141 Q143 Q149 Q160 Q162 Q172 Q175 Q181 Q184 Q185 Q189 Q224 |
| 15 | [Kubernetes API, Extensions & Versions](#15-kubernetes-api-extensions--versions) | 8 | Q11 Q24 Q33 Q59 Q96 Q174 Q179 Q191 |
| 16 | [Cluster Management & Namespaces](#16-cluster-management--namespaces) | 6 | Q35 Q43 Q99 Q118 Q177 Q186 |

---

## 1. Kubernetes Architecture & Core Components

> Control plane components (API server, etcd, scheduler, controller managers) and node components (kubelet, kube-proxy, container runtime).

---

**Q3.** A CronJob is scheduled to run every one hour. What happens when it's time to run?

A. Kubelet watches API Server for CronJob objects and runs the Pod directly.  
B. Kube-scheduler watches API Server for CronJob objects.  
C. CronJob controller creates a Pod and waits until it finishes.  
D. CronJob controller creates a Job. Then the Job controller creates a Pod and waits until it finishes.

**✅ Answer: D**

---

**Q4.** What is the purpose of the kubelet component within a Kubernetes cluster?

A. A dashboard for Kubernetes Clusters.  
B. A network proxy that runs on each node, implementing part of the Kubernetes Service concept.  
C. A component that watches for newly created Pods with no assigned node.  
D. An agent that runs on each node. It makes sure that containers are running in a Pod.

**✅ Answer: D**

---

**Q40.** When trying to create a Service of type LoadBalancer, the external-ip is stuck in "Pending" state. Which Kubernetes component is failing?

A. Cloud Controller Manager  
B. Load Balancer Manager  
C. Cloud Architecture Manager  
D. Cloud Load Balancer Manager

**✅ Answer: A**

---

**Q44.** What component enables end users, different parts of the Kubernetes cluster, and external components to communicate with one another?

A. kubectl  
B. AWS Management Console  
C. Kubernetes API  
D. Google Cloud SDK

**✅ Answer: C**

---

**Q46.** Which of these components is part of the Kubernetes Control Plane?

A. coredns  
B. cloud-controller-manager  
C. kube-proxy  
D. kubelet

**✅ Answer: B**

---

**Q58.** In the Kubernetes platform, which component is responsible for running containers?

A. etcd  
B. CRI-O  
C. cloud-controller-manager  
D. kube-controller-manager

**✅ Answer: B**

---

**Q61.** What Kubernetes control plane component exposes the programmatic interface used to create, manage and interact with Kubernetes objects?

A. kube-controller-manager  
B. kube-proxy  
C. kube-apiserver  
D. etcd

**✅ Answer: C**

---

**Q74.** What are the most important resources to guarantee the performance of an etcd cluster?

A. CPU and disk capacity.  
B. Network throughput and disk I/O.  
C. CPU and RAM memory.  
D. Network throughput and CPU.

**✅ Answer: B**

---

**Q103.** Which key-value store is used to persist Kubernetes cluster data?

A. etcd  
B. ZooKeeper  
C. ControlPlaneStore  
D. Redis

**✅ Answer: A**

---

**Q121.** What element allows Kubernetes to run Pods across the fleet of nodes?

A. The node server.  
B. The etcd static pods.  
C. The API server.  
D. The kubelet.

**✅ Answer: D**

---

**Q129.** Which component of the node is responsible to run workloads?

A. The kubelet.  
B. The kubeproxy.  
C. The kube-apiserver.  
D. The container runtime.

**✅ Answer: D**

---

**Q142.** What is the main purpose of etcd in Kubernetes?

A. etcd stores all cluster data in a key value store.  
B. etcd stores the containers running in the cluster for disaster recovery.  
C. etcd stores copies of the Kubernetes config files that live in /etc/.  
D. etcd stores the YAML definitions for all the cluster components.

**✅ Answer: A**

---

**Q148.** Which item is a Kubernetes node component?

A. kube-scheduler  
B. kubectl  
C. kube-proxy  
D. etcd

**✅ Answer: C**

---

**Q156.** Which component of the Kubernetes architecture is responsible for integration with the CRI container runtime?

A. kubeadm  
B. kubelet  
C. kube-apiserver  
D. kubectl

**✅ Answer: B**

---

**Q163.** What is the correct hierarchy of Kubernetes components?

A. Containers → Pods → Cluster → Nodes  
B. Nodes → Cluster → Containers → Pods  
C. Cluster → Nodes → Pods → Containers  
D. Pods → Cluster → Containers → Nodes

**✅ Answer: C**

---

**Q169.** Which component in Kubernetes is responsible to watch newly created Pods with no assigned node, and selects a node for them to run on?

A. etcd  
B. kube controller-manager  
C. kube proxy  
D. kube scheduler

**✅ Answer: D**

---

**Q170.** Which control plane component is responsible for updating the node Ready condition if a node becomes unreachable?

A. The kube-proxy.  
B. The node controller.  
C. The kubelet.  
D. The kube-apiserver.

**✅ Answer: B**

---

**Q187.** What is the default eviction timeout when the Ready condition of a node is Unknown or False?

A. Thirty seconds.  
B. Thirty minutes.  
C. One minute.  
D. Five minutes.

**✅ Answer: D**

---

**Q220.** In Kubernetes, what is the primary responsibility of the kubelet running on each worker node?

A. To provide internal DNS resolution and route service traffic between Pods and nodes.  
B. To allocate persistent storage volumes and manage distributed data replication for Pods.  
C. To ensure that containers defined in Pod specifications are running and remain healthy on the node.  
D. To manage cluster state information and handle all scheduling decisions for workloads.

**✅ Answer: C**

---

**Q234.** Which statement best describes the role of kubelet on a Kubernetes worker node?

A. kubelet monitors cluster-wide resource usage and assigns Pods to the most suitable nodes.  
B. kubelet acts as the primary API component that stores and manages cluster state information.  
C. kubelet manages the container runtime and ensures that all Pods scheduled to the node are running as expected.  
D. kubelet configures networking rules on each node to handle traffic routing for Services.

**✅ Answer: C**

---

## 2. Workloads

> Pod, Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob — running and managing applications.

---

**Q2.** Which API object is the recommended way to run a scalable, stateless application on your cluster?

A. ReplicaSet  
B. Deployment  
C. DaemonSet  
D. Pod

**✅ Answer: B**

---

**Q18.** Which of the following workloads require a headless service while deploying into the namespace?

A. StatefulSet  
B. CronJob  
C. Deployment  
D. DaemonSet

**✅ Answer: A**

---

**Q21.** How to load and generate data required before the Pod startup?

A. Use an init container with shared file storage.  
B. Use a PVC volume.  
C. Use a sidecar container with shared volume.  
D. Use another pod with a PVC.

**✅ Answer: A**

---

**Q23.** Which Kubernetes resource workload ensures that all (or some) nodes run a copy of a Pod?

A. ReplicaSet  
B. StatefulSet  
C. DaemonSet  
D. Deployment

**✅ Answer: C**

---

**Q37.** Which Kubernetes feature would you use to guard against split brain scenarios with your distributed application?

A. Replication controllers  
B. Consensus protocols  
C. Rolling updates  
D. StatefulSet

**✅ Answer: D**

---

**Q51.** How are ReplicaSets and Deployments related?

A. Deployments manage ReplicaSets and provide declarative updates to Pods.  
B. ReplicaSets manage stateful applications, Deployments manage stateless applications.  
C. Deployments are runtime instances of ReplicaSets.  
D. ReplicaSets are subsets of Jobs and CronJobs which use imperative Deployments.

**✅ Answer: A**

---

**Q56.** What is a DaemonSet?

A. It's a type of workload that ensures a specific set of nodes run a copy of a Pod.  
B. It's a type of workload responsible for maintaining a stable set of replica Pods running in any node.  
C. It's a type of workload that needs to be run periodically on a given schedule.  
D. It's a type of workload that provides guarantees about ordering, uniqueness, and identity of a set of Pods.

**✅ Answer: A**

---

**Q67.** What is the resource type used to package sets of containers for scheduling in a cluster?

A. Pod  
B. ContainerSet  
C. ReplicaSet  
D. Deployment

**✅ Answer: A**

---

**Q70.** What is the default deployment strategy in Kubernetes?

A. Rolling update  
B. Blue/Green deployment  
C. Canary deployment  
D. Recreate deployment

**✅ Answer: A**

---

**Q83.** Which Kubernetes component is the smallest deployable unit of computing?

A. StatefulSet  
B. Deployment  
C. Pod  
D. Container

**✅ Answer: C**

---

**Q88.** What is a sidecar container?

A. A Pod that runs next to another container within the same Pod.  
B. A container that runs next to another Pod within the same namespace.  
C. A container that runs next to another container within the same Pod.  
D. A Pod that runs next to another Pod within the same namespace.

**✅ Answer: C**

---

**Q94.** Imagine there is a requirement to run a database backup every day. Which Kubernetes resource could be used?

A. Kube-scheduler  
B. CronJob  
C. Task  
D. Job

**✅ Answer: B**

---

**Q107.** What do Deployments and StatefulSets have in common?

A. They manage Pods that are based on an identical container spec.  
B. They support the OnDelete update strategy.  
C. They support an ordered, graceful deployment and scaling.  
D. They maintain a sticky identity for each of their Pods.

**✅ Answer: A**

---

**Q114.** What is the difference between a Deployment and a ReplicaSet?

A. With a Deployment, you can't control the number of pod replicas.  
B. A ReplicaSet does not guarantee a stable set of replica pods running.  
C. A Deployment is basically the same as a ReplicaSet with annotations.  
D. A Deployment is a higher-level concept that manages ReplicaSets.

**✅ Answer: D**

---

**Q120.** What is a Pod?

A. A networked application within Kubernetes.  
B. A storage volume within Kubernetes.  
C. A single container within Kubernetes.  
D. A group of one or more containers within Kubernetes.

**✅ Answer: D**

---

**Q122.** What is the Kubernetes object used for running a recurring workload?

A. Job  
B. Batch  
C. DaemonSet  
D. CronJob

**✅ Answer: D**

---

**Q138.** Which of the following is a feature Kubernetes provides by default as a container orchestration tool?

A. A portable operating system.  
B. File system redundancy.  
C. A container image registry.  
D. Automated rollouts and rollbacks.

**✅ Answer: D**

---

**Q140.** If a Pod was waiting for container images to download on the scheduled node, what state would it be in?

A. Failed  
B. Succeeded  
C. Unknown  
D. Pending

**✅ Answer: D**

---

**Q153.** What sentence is true about CronJobs in Kubernetes?

A. A CronJob creates one or multiple Jobs on a repeating schedule.  
B. A CronJob creates one container on a repeating schedule.  
C. CronJobs are useful on Linux but are obsolete in Kubernetes.  
D. The CronJob schedule format is different in Kubernetes and Linux.

**✅ Answer: A**

---

**Q161.** Which of these is a valid container restart policy?

A. On login  
B. On update  
C. On start  
D. On failure

**✅ Answer: D**

---

**Q167.** What can be used to create a job that will run at specified times/dates or on a repeating schedule?

A. Job  
B. CalenderJob  
C. BatchJob  
D. CronJob

**✅ Answer: D**

---

**Q176.** What happens with a regular pod running in Kubernetes when a node fails?

A. A new pod with the same UID is scheduled to another node after a while.  
B. A new, near-identical pod but with different UID is scheduled to another node.  
C. By default, a pod can only be scheduled to the same node when the node fails.  
D. A new pod is scheduled on a different node only if configured explicitly.

**✅ Answer: B**

---

**Q180.** What is an ephemeral container?

A. A specialized container that runs as root for infosec applications.  
B. A specialized container that runs temporarily in an existing Pod.  
C. A specialized container that extends and enhances the 'main' container in a Pod.  
D. A specialized container that runs before app container in a Pod.

**✅ Answer: B**

---

**Q182.** Which Kubernetes-native deployment strategy supports zero-downtime updates of a workload?

A. Canary  
B. Recreate  
C. BlueGreen  
D. RollingUpdate

**✅ Answer: D**

---

**Q188.** What is the main purpose of a DaemonSet?

A. A DaemonSet ensures that all (or certain) nodes run a copy of a Pod.  
B. A DaemonSet ensures that the kubelet is constantly up and running.  
C. A DaemonSet ensures that there are as many pods running as specified in the replicas field.  
D. A DaemonSet ensures that a process (agent) runs on every node.

**✅ Answer: A**

---

**Q190.** Which two elements are shared between containers in the same pod?

A. Network resources and liveness probes.  
B. Storage and container image registry.  
C. Storage and network resources.  
D. Network resources and Dockerfiles.

**✅ Answer: C**

---

**Q193.** Which of the following resources helps in managing a stateless application workload on a Kubernetes cluster?

A. DaemonSet  
B. StatefulSet  
C. kubectl  
D. Deployment

**✅ Answer: D**

---

**Q210.** There is an application running in a logical chain: Gateway API, Service, EndpointSlice, and Container. What Kubernetes API object is missing from this sequence?

A. Docker  
B. Proxy  
C. Pod  
D. Firewall

**✅ Answer: C**

---

**Q227.** Which of the following actions is supported when working with Pods in Kubernetes?

A. Creating Pods through workload resources like Deployments.  
B. Guaranteeing Pods always stay on the same node once scheduled.  
C. Managing static Pods directly through the API server.  
D. Renaming containers in a Pod using kubectl patch.

**✅ Answer: A**

---

**Q238.** In a Kubernetes cluster, which scenario best illustrates the use case for a StatefulSet?

A. A web application that requires multiple replicas for load balancing.  
B. A service that routes traffic to various microservices in the cluster.  
C. A database that requires persistent storage and stable network identities.  
D. A background job that runs periodically and does not maintain state.

**✅ Answer: C**

---

## 3. Services & Networking

> Service types (ClusterIP, NodePort, LoadBalancer, ExternalName), Ingress, Gateway API, NetworkPolicy, DNS, kube-proxy.

---

**Q7.** What is a Kubernetes service with no cluster IP address called?

A. Headless Service  
B. Nodeless Service  
C. IPLess Service  
D. Specless Service

**✅ Answer: A**

---

**Q10.** What function does kube-proxy provide to a cluster?

A. Implementing the Ingress resource type for application traffic.  
B. Forwarding data to the correct endpoints for Services.  
C. Managing data egress from the cluster nodes to the network.  
D. Managing access to the Kubernetes API.

**✅ Answer: B**

---

**Q29.** Which statement about Ingress is correct?

A. Ingress provides a simple way to track network endpoints within a cluster.  
B. Ingress is a Service type like NodePort and ClusterIP.  
C. Ingress is a construct that allows you to specify how a Pod is allowed to communicate.  
D. Ingress exposes routes from outside the cluster to services in the cluster.

**✅ Answer: D**

---

**Q38.** What feature must a CNI support to control specific traffic flows for workloads running in Kubernetes?

A. Border Gateway Protocol  
B. IP Address Management  
C. Pod Security Policy  
D. Network Policies

**✅ Answer: D**

---

**Q39.** What is the main role of the Kubernetes DNS within a cluster?

A. Acts as a DNS server for virtual machines running outside the cluster.  
B. Provides a DNS as a Service, allowing users to create zones and registries.  
C. Allows Pods running in dual stack to convert IPv6 calls into IPv4 calls.  
D. Provides consistent DNS Names for Pods and Services for workloads that need to communicate with each other.

**✅ Answer: D**

---

**Q48.** What is a key feature of a container network?

A. Proxying REST requests across a set of containers.  
B. Allowing containers running on separate hosts to communicate.  
C. Allowing containers on the same host to communicate.  
D. Caching remote disk access.

**✅ Answer: B**

---

**Q50.** What is the goal of load balancing?

A. Automatically measure request performance across instances of an application.  
B. Automatically distribute requests across different versions of an application.  
C. Automatically distribute instances of an application across the cluster.  
D. Automatically distribute requests across instances of an application.

**✅ Answer: D**

---

**Q55.** What is the name of the Kubernetes resource used to expose an application?

A. Port  
B. Service  
C. DNS  
D. Deployment

**✅ Answer: B**

---

**Q60.** What Kubernetes component handles network communications inside and outside of a cluster, using operating system packet filtering if available?

A. kube-proxy  
B. kubelet  
C. etcd  
D. kube-controller-manager

**✅ Answer: A**

---

**Q62.** Which type of Service requires manual creation of Endpoints?

A. LoadBalancer  
B. Services without selectors  
C. NodePort  
D. ClusterIP with selectors

**✅ Answer: B**

---

**Q66.** Which statement about the Kubernetes network model is correct?

A. Pods can only communicate with Pods exposed via a Service.  
B. Pods can communicate with all Pods without NAT.  
C. The Pod IP is only visible inside a Pod.  
D. The Service IP is used for the communication between Services.

**✅ Answer: B**

---

**Q68.** Can a Kubernetes Service expose multiple ports?

A. No, you can only expose one port per each Service.  
B. Yes, but you must specify an unambiguous name for each port.  
C. Yes, the only requirement is to use different port numbers.  
D. No, because the only port you can expose is port number 443.

**✅ Answer: B**

---

**Q73.** What is the role of a NetworkPolicy in Kubernetes?

A. The ability to cryptic and obscure all traffic.  
B. The ability to classify the Pods as isolated and non isolated.  
C. The ability to prevent loopback or incoming host traffic.  
D. The ability to log network security events.

**✅ Answer: B**

---

**Q77.** How to create a headless service?

A. By specifying `.spec.ClusterIP: headless`  
B. By specifying `.spec.clusterIP: None`  
C. By specifying `.spec.ClusterIP: 0.0.0.0`  
D. By specifying `.spec.ClusterIP: localhost`

**✅ Answer: B**

---

**Q81.** What is a Kubernetes Service Endpoint?

A. It is the API Endpoint of our Kubernetes cluster.  
B. It is a name of special Pod in kube-system namespace.  
C. It is an IP address that we can access from the Internet.  
D. It is an object that gets IP addresses of individual Pods assigned to it.

**✅ Answer: D**

---

**Q85.** Which are the two primary modes for Service discovery within a Kubernetes cluster?

A. Environment variables and DNS  
B. API Calls and LDAP  
C. Labels and Radius  
D. Selectors and DHCP

**✅ Answer: A**

---

**Q98.** A Kubernetes ___ is an abstraction that defines a logical set of Pods and a policy by which to access them.

A. Selector  
B. Controller  
C. Service  
D. Job

**✅ Answer: C**

---

**Q104.** What Linux namespace is shared by default by containers running within a Kubernetes Pod?

A. Host Network  
B. Network  
C. Process ID  
D. Process Name

**✅ Answer: B**

---

**Q113.** How many different Kubernetes service types can you define?

A. 2  
B. 3  
C. 4  
D. 5

**✅ Answer: C** *(ClusterIP, NodePort, LoadBalancer, ExternalName)*

---

**Q130.** The IPv4/IPv6 dual stack in Kubernetes:

A. Translates an IPv4 request from a service to an IPv6 service.  
B. Allows you to access the IPv4 address by using the IPv6 address.  
C. Requires NetworkPolicies to prevent services from mixing requests.  
D. Allows you to create IPv4 and IPv6 dual stack services.

**✅ Answer: D**

---

**Q134.** What is a Service?

A. A static network mapping from a Pod to a port.  
B. A way to expose an application running on a set of Pods.  
C. The network configuration for a group of Pods.  
D. An NGINX load balancer that gets deployed for an application.

**✅ Answer: B**

---

**Q154.** What is the purpose of the kube-proxy?

A. The kube-proxy balances network requests to pods.  
B. The kube-proxy maintains network rules on nodes.  
C. The kube-proxy ensures the cluster connectivity with internet.  
D. The kube-proxy maintains the DNS rules of the cluster.

**✅ Answer: B**

---

**Q178.** What is the main purpose of the Ingress in Kubernetes?

A. Access HTTP and HTTPS services running in the cluster based on their IP address.  
B. Access services different from HTTP or HTTPS running in the cluster based on their IP address.  
C. Access services different from HTTP or HTTPS running in the cluster based on their path.  
D. Access HTTP and HTTPS services running in the cluster based on their path.

**✅ Answer: D**

---

**Q199.** In Kubernetes, which abstraction defines a logical set of Pods and a policy by which to access them?

A. Service Account  
B. NetworkPolicy  
C. Service  
D. Custom Resource Definition

**✅ Answer: C**

---

**Q209.** What is an advantage of using the Gateway API compared to Ingress in Kubernetes?

A. To provide clearer role separation between infrastructure providers and application developers.  
B. To expose an application externally by creating only a Service resource.  
C. To configure routing rules through annotations directly on Ingress resources.  
D. To automatically scale workloads based on CPU and memory utilization.

**✅ Answer: A**

---

**Q212.** Which Kubernetes Service type exposes a service only within the cluster?

A. ClusterIP  
B. LoadBalancer  
C. ExternalName  
D. NodePort

**✅ Answer: A**

---

**Q213.** What is the main role of the CoreDNS within a cluster?

A. Provides consistent DNS Names for Pods and Services for workloads that need to communicate with each other.  
B. Allows Pods running in dual stack to convert IPv6 calls into IPv4 calls.  
C. Acts as a DNS server for virtual machines that are running outside the cluster.  
D. Provides a DNS as a Service, allowing users to create zones and registries for domains that they own.

**✅ Answer: A**

---

**Q221.** What is the role of the `ingressClassName` field in a Kubernetes Ingress resource?

A. It indicates which Ingress Controller should implement the rules defined in the Ingress resource.  
B. It specifies the backend Service used by the Ingress Controller to route external requests.  
C. It defines the type of protocol (HTTP or HTTPS) that the Ingress Controller should process.  
D. It determines how routing rules are prioritized when multiple Ingress objects are applied.

**✅ Answer: A**

---

**Q225.** What is the goal of load balancing?

A. To move and reschedule application instances across nodes in the cluster for efficiency.  
B. To distribute traffic between different application versions as part of a deployment strategy.  
C. To distribute requests evenly across instances of an application to improve availability and reliability.  
D. To automatically measure and compare request performance across instances of an application.

**✅ Answer: C**

---

**Q233.** In Kubernetes, what is the primary purpose of creating a Service resource for a Deployment?

A. To automatically adjust the number of Pods based on CPU or memory utilization metrics.  
B. To centrally manage and apply runtime configuration values for application components.  
C. To provide a stable endpoint for accessing Pods even when their IP addresses change.  
D. To define and attach persistent volumes that store application data across Pod restarts.

**✅ Answer: C**

---

**Q246.** What is the Kubernetes abstraction that allows groups of Pods to be exposed inside a Kubernetes cluster?

A. Service  
B. Deployment  
C. Unit  
D. Daemon

**✅ Answer: A**

---

**Q250.** What is the purpose of the Gateway API in Kubernetes?

A. To collect, monitor, and log performance metrics for applications running inside Kubernetes Pods.  
B. To define and manage persistent storage resources that are used by Pods within the cluster.  
C. To provide a standardized interface for managing traffic routing to Services in a Kubernetes environment.  
D. To enable applications to automatically adjust resource usage and scale up or down based on demand.

**✅ Answer: C**

---

**Q264.** Gateway API is a successor to what Kubernetes API?

A. NetworkPolicy  
B. Service  
C. NodePort  
D. Ingress

**✅ Answer: D**

---

**Q269.** Which Kubernetes resource provides standardized load balancing, TLS termination, and multi-context traffic routing across different implementations?

A. Gateway API  
B. CoreDNS  
C. Ingress  
D. kube-proxy

**✅ Answer: A**

---

## 4. Storage

> PersistentVolumes (PV), PersistentVolumeClaims (PVC), StorageClass, CSI, ephemeral storage.

---

**Q32.** Which storage operator in Kubernetes can help the system to self-scale, self-heal, etc?

A. Rook  
B. Kubernetes  
C. Helm  
D. Container Storage Interface (CSI)

**✅ Answer: A**

---

**Q78.** How does dynamic storage provisioning work?

A. A user requests dynamically provisioned storage by including an existing storage class in their PersistentVolumeClaim.  
B. An administrator creates a storage class and includes it in their pod YAML definition file without creating a PVC.  
C. A pod requests dynamically provisioned storage by including a storage class and the pod name in their PVC.  
D. An administrator creates a PersistentVolume and includes the name of the PV in their pod YAML definition file.

**✅ Answer: A**

---

**Q102.** Which resource do you use to attach a volume in a Pod?

A. StorageVolume  
B. PersistentVolume  
C. StorageClass  
D. PersistentVolumeClaim

**✅ Answer: D**

---

**Q132.** What is ephemeral storage?

A. Storage space that need not persist across restarts.  
B. Storage that may grow dynamically.  
C. Storage used by multiple consumers (e.g. multiple Pods).  
D. Storage that is always provisioned locally.

**✅ Answer: A**

---

**Q150.** How is application data maintained in containers?

A. Store data into data folders.  
B. Store data in separate folders.  
C. Store data into sidecar containers.  
D. Store data into volumes.

**✅ Answer: D**

---

**Q155.** Manual reclamation policy of a PVC resource is known as:

A. claimRef  
B. Delete  
C. Retain  
D. Recycle

**✅ Answer: C**

---

**Q219.** Ceph is a highly scalable distributed storage solution. Which open source cloud native storage orchestrator automates deployment and management of Ceph?

A. MinIO  
B. OpenEBS  
C. CubeFS  
D. Rook

**✅ Answer: D**

---

**Q236.** A request for 500 mebibytes of ephemeral storage must be specified in a YAML file. How should this be written?

A. 0.5M  
B. 500m  
C. 500Mi  
D. 500mi

**✅ Answer: C**

---

**Q258.** Which statement is true about the Container Storage Interface (CSI) in Kubernetes?

A. CSI is a Kubernetes-only API built into the kubelet; drivers are compiled in-tree.  
B. CSI introduces a model where applications mount storage by referencing the driver name in the Pod specification instead of using PVs.  
C. CSI provides storage by focusing on NFS and iSCSI backends only.  
D. CSI is a cross-platform specification; in Kubernetes, CSI drivers run as pods with sidecars and are consumed through StorageClass and PVC objects.

**✅ Answer: D**

---

**Q263.** Which statement best explains the Container Storage Interface (CSI) concept in Kubernetes?

A. CSI is a standard API specification that allows Kubernetes (and other orchestrators) to use different storage systems via plugins.  
B. CSI is a networking interface that manages container-to-container communication across nodes.  
C. CSI is a Kubernetes-only API that provides built-in storage drivers for Pods to access persistent volumes.  
D. CSI is a framework inside Kubernetes that automatically provisions local node storage without requiring external storage plugins.

**✅ Answer: A**

---

## 5. Configuration, Secrets & Labels

> ConfigMap, Secrets, Labels, Annotations — storing and referencing configuration data.

---

**Q9.** What default level of protection is applied to the data in Secrets in the Kubernetes API?

A. The values use AES Symmetric Encryption.  
B. The values are stored in plain text.  
C. The values are encoded with SHA256 hashes.  
D. The values are base64 encoded.

**✅ Answer: D**

---

**Q54.** What is the primary mechanism to identify grouped objects in a Kubernetes cluster?

A. Custom Resources  
B. Labels  
C. Label Selector  
D. Pod

**✅ Answer: B**

---

**Q65.** Which Kubernetes resource uses `immutable: true` boolean field?

A. Deployment  
B. Pod  
C. ConfigMap  
D. ReplicaSet

**✅ Answer: C**

---

**Q106.** What does the `nodeSelector` within a PodSpec use to place Pods on the target nodes?

A. Annotations  
B. IP Addresses  
C. Hostnames  
D. Labels

**✅ Answer: D**

---

**Q164.** Which statement about Secrets is correct?

A. A Secret is part of a Pod specification.  
B. Secrets data is encrypted with the cluster private key by default.  
C. Secrets data is base64 encoded and stored unencrypted by default.  
D. A Secret can only be used for confidential data.

**✅ Answer: C**

---

**Q165.** Which mechanism allows extending the Kubernetes API?

A. ConfigMap  
B. CustomResourceDefinition  
C. MutatingAdmissionWebhook  
D. Kustomize

**✅ Answer: B**

---

**Q203.** Kubernetes Secrets are specifically intended to hold confidential data. Which API object should be used to hold non-confidential data?

A. CSI  
B. RBAC  
C. CNI  
D. ConfigMaps

**✅ Answer: D**

---

**Q204.** When a Kubernetes Secret is created, how is the data stored by default in etcd?

A. As Base64-encoded strings that provide simple encoding but no actual encryption.  
B. As plain text values that are directly stored without any obfuscation or additional encoding.  
C. As encrypted records automatically protected using the Kubernetes control plane master key.  
D. As compressed binary objects that are optimized for space but not secured against access.

**✅ Answer: A**

---

**Q228.** In Kubernetes, what is the primary purpose of using annotations?

A. To specify the deployment strategy for applications.  
B. To control the access permissions for users and service accounts.  
C. To provide a way to attach metadata to objects.  
D. To define the specifications for resource limits and requests.

**✅ Answer: C**

---

**Q265.** How are ConfigMaps correctly used in Kubernetes applications?

A. A ConfigMap is used to adjust cluster scheduling rules applied by the Kubernetes scheduler.  
B. A ConfigMap is used to keep encrypted secrets such as passwords for databases or tokens.  
C. A ConfigMap is used to provide configuration values that Pods consume as files or variables.  
D. A ConfigMap is used to hold shared log files that applications produce during execution.

**✅ Answer: C**

---

**Q270.** Which statement describes the purpose of Labels in Kubernetes?

*(Note: options were not fully captured from source. The correct answer is:)*  
Labels are **key/value pairs attached to Kubernetes objects** that are used for identification, grouping, and selection of objects via selectors. They have no semantic meaning to the Kubernetes core system.

**✅ Answer: Labels are key-value pairs for identifying and selecting grouped objects.**

---

## 6. Security & RBAC

> Role-Based Access Control (RBAC), Pod Security Standards, Security Context, ServiceAccounts, OPA.

---

**Q5.** What is the default value for `authorization-mode` in Kubernetes API server?

A. `--authorization-mode=RBAC`  
B. `--authorization-mode=AlwaysAllow`  
C. `--authorization-mode=AlwaysDeny`  
D. `--authorization-mode=ABAC`

**✅ Answer: B**

---

**Q14.** Which of the following statements is correct concerning Open Policy Agent (OPA)?

A. The policies must be written in Python language.  
B. Kubernetes can use it to validate requests and apply policies.  
C. Policies can only be tested when published.  
D. It cannot be used outside Kubernetes.

**✅ Answer: B**

---

**Q26.** What is the order of 4C's in Cloud Native Security, starting with the layer that a user has the most control over?

A. Cloud → Container → Cluster → Code  
B. Container → Cluster → Code → Cloud  
C. Cluster → Container → Code → Cloud  
D. Code → Container → Cluster → Cloud

**✅ Answer: D**

---

**Q86.** Which of the following capabilities are you allowed to add to a container using the Restricted policy?

A. CHOWN  
B. SYS_CHROOT  
C. SETUID  
D. NET_BIND_SERVICE

**✅ Answer: D**

---

**Q116.** Which authorization-mode allows granular control over the operations that different entities can perform on different objects in a Kubernetes cluster?

A. Webhook Mode Authorization Control  
B. Role Based Access Control  
C. Node Authorization Access Control  
D. Attribute Based Access Control

**✅ Answer: B**

---

**Q135.** What's the difference between a security profile and a security context?

A. Security Contexts configure Clusters and Namespaces at runtime. Security profiles are control plane mechanisms.  
B. Security Contexts configure Pods and Containers at runtime. Security profiles are control plane mechanisms to enforce specific settings in the Security Context.  
C. Security Profiles configure Pods and Containers at runtime. Security Contexts are control plane mechanisms.  
D. Security Profiles configure Clusters and Namespaces at runtime. Security Contexts are control plane mechanisms.

**✅ Answer: B**

---

**Q137.** What framework does Kubernetes use to authenticate users with JSON Web Tokens?

A. OpenID Connect  
B. OpenID Container  
C. OpenID Cluster  
D. OpenID CNCF

**✅ Answer: A**

---

**Q157.** Which one of the following is an open source runtime security tool?

A. lxd  
B. containerd  
C. falco  
D. gvisor

**✅ Answer: C**

---

**Q183.** What service account does a Pod use in a given namespace when the service account is not specified?

A. admin  
B. sysadmin  
C. root  
D. default

**✅ Answer: D**

---

**Q195.** Which of the following is a recommended security habit in Kubernetes?

A. Run the containers as the user with group ID 0 (root) and any user ID.  
B. Disallow privilege escalation from within a container as the default option.  
C. Run the containers as the user with user ID 0 (root) and any group ID.  
D. Allow privilege escalation from within a container as the default option.

**✅ Answer: B**

---

**Q202.** An administrator needs to ensure Pods in the test namespace follow Kubernetes Pod Security Standards to prevent unsafe configurations. The goal is to enforce the Restricted policy. Which action should be taken?

A. Apply a PodSecurityAdmission configuration with the Restricted standard to the test namespace.  
B. Update the RoleBindings for the test namespace to limit access to specific users.  
C. Set the PodSecurityStandard annotation on each Pod in the test namespace.  
D. Create a NetworkPolicy that restricts access to Pods in the test namespace.

**✅ Answer: A**

---

**Q208.** Which option best represents the Pod Security Standards ordered from most permissive to most restrictive?

A. Privileged, Restricted, Baseline  
B. Baseline, Restricted, Privileged  
C. Baseline, Privileged, Restricted  
D. Privileged, Baseline, Restricted

**✅ Answer: D**

---

**Q215.** A platform engineer needs to ensure an application can securely access the Kubernetes API without using a developer's personal credentials. What is the correct way to configure this?

A. Creates ServiceAccount and bind it to the Pod for API access.  
B. Generate a certificate for the application to access the API.  
C. Set the application to use the default ServiceAccount in the namespace.  
D. Use a developer's kubeconfig file with restricted permissions.

**✅ Answer: A**

---

**Q243.** How does cert-manager integrate with Kubernetes resources to provide TLS certificates for an application?

A. It injects TLS certificates directly into Pods when the workloads are deployed.  
B. It manages Certificate resources and Secrets that can be used by Ingress objects for TLS.  
C. It updates kube-proxy configuration to ensure encrypted traffic between Services.  
D. It replaces default Kubernetes API certificates with those from external authorities.

**✅ Answer: B**

---

**Q244.** In Kubernetes, what is the primary function of a RoleBinding?

A. To create and define a new Role object that contains a specific set of permissions.  
B. To enforce namespace network rules by binding policies to Pods running in the namespace.  
C. To assign the permissions of a Role to a user, group, or service account within a namespace.  
D. To provide a user or group with permissions across all resources at the cluster level.

**✅ Answer: C**

---

**Q251.** Why are ServiceAccounts used in Kubernetes when assigning RBAC permissions to Pods?

A. They are used to authenticate Pods by creating special user identities for administrators to manage.  
B. They are designed to provide a secure mechanism for storing application credentials inside Pods.  
C. They automatically assign Pods high-level cluster-admin permissions for accessing resources.  
D. They let Pods authenticate to the API server securely without personal user credentials.

**✅ Answer: D**

---

**Q268.** In Kubernetes RBAC, what is the primary difference between a Role and a ClusterRole?

A. A Role is generally weaker in scope and power, while a ClusterRole is always considered stronger.  
B. A Role is namespace-scoped and applies only within a single namespace, while a ClusterRole is cluster-scoped and applies across all namespaces.  
C. A Role defines permissions for modifying resources, while a ClusterRole defines permissions only for viewing resources.  
D. A Role is limited to Pods and Services, while a ClusterRole can grant access to every resource type.

**✅ Answer: B**

---

## 7. Observability & Monitoring

> Metrics, Logs, Traces/Spans, Prometheus, Grafana, health probes, observability pillars.

---

**Q36.** What is a probe within Kubernetes?

A. A monitoring mechanism of the Kubernetes API.  
B. A pre-operational scope issued by the kubectl agent.  
C. A diagnostic performed periodically by the kubelet on a container.  
D. A logging mechanism of the Kubernetes API.

**✅ Answer: C**

---

**Q53.** What is the core metric type in Prometheus used to represent a single numerical value that can go up and down?

A. Summary  
B. Counter  
C. Histogram  
D. Gauge

**✅ Answer: D**

---

**Q57.** What is the telemetry component that represents a series of related distributed events that encode the end-to-end request flow through a distributed system?

A. Metrics  
B. Logs  
C. Spans  
D. Traces

**✅ Answer: D**

---

**Q91.** Which Prometheus metric represents a single value that can go up and down?

A. Counter  
B. Gauge  
C. Summary  
D. Histogram

**✅ Answer: B**

---

**Q110.** Which tools enable Kubernetes HorizontalPodAutoscalers to use custom, application-generated metrics to trigger scaling events?

A. Prometheus and the prometheus-adapter.  
B. Graylog and graylog-autoscaler metrics.  
C. Graylog and the kubernetes-adapter.  
D. Grafana and Prometheus.

**✅ Answer: A**

---

**Q111.** Which of the following is a valid PromQL query?

A. `SELECT * from http_requests_total WHERE job=apiserver`  
B. `http_requests_total WHERE (job="apiserver")`  
C. `SELECT * from http_requests_total`  
D. `http_requests_total{job="apiserver"}` *(shown in exam as closest option)*

**✅ Answer: D**

---

**Q136.** At which layer would distributed tracing be implemented in a cloud native deployment?

A. Network  
B. Application  
C. Database  
D. Infrastructure

**✅ Answer: B**

---

**Q147.** To visualize data from Prometheus you can use expression browser or console templates. What is the other data visualization tool commonly used together with Prometheus?

A. Grafana  
B. Graphite  
C. Nirvana  
D. GraphQL

**✅ Answer: A**

---

**Q166.** Which of the following observability data streams would be most useful when desiring to plot resource consumption and predicted future resource exhaustion?

A. stdout  
B. Traces  
C. Logs  
D. Metrics

**✅ Answer: D**

---

**Q192.** What is the API that exposes resource metrics from the metrics-server?

A. custom.k8s.io  
B. resources.k8s.io  
C. metrics.k8s.io  
D. cadvisor.k8s.io

**✅ Answer: C**

---

**Q196.** What are the 3 pillars of Observability?

A. Metrics, Logs, and Traces  
B. Metrics, Logs, and Spans  
C. Metrics, Data, and Traces  
D. Resources, Logs, and Tracing

**✅ Answer: A**

---

**Q200.** What does the `livenessProbe` in Kubernetes help detect?

A. When a container has started successfully.  
B. When a container is unresponsive.  
C. When a container is ready to serve traffic.  
D. When a container exceeds resource limits.

**✅ Answer: B**

---

**Q214.** A Pod has been created, but `kubectl get pods` shows the ready column as `0/1`. What Kubernetes feature causes this behavior?

A. Readiness Probes  
B. Security Contexts  
C. DNS Policy  
D. Node Selector

**✅ Answer: A**

---

**Q262.** Which statement best describes observability in a cloud native environment?

A. Observability is the practice of collecting and analyzing logs, metrics, and traces to understand system behavior.  
B. Observability is the use of alerts and dashboards to notify administrators about system outages.  
C. Observability is the process of restarting failed Pods to maintain application availability.  
D. Observability is the configuration of health probes to check whether containers are running correctly.

**✅ Answer: A**

---

## 8. Scheduling & Autoscaling

> kube-scheduler, nodeSelector, HPA, VPA, Cluster Autoscaler, KEDA, resource limits/requests, PodDisruptionBudget.

---

**Q6.** An organization needs to process large amounts of data in bursts on a cloud-based Kubernetes cluster. What's the most cost-effective method?

A. Run a group of nodes with the exact required size using taints, tolerations, and nodeSelectors.  
B. Leverage the Kubernetes Cluster Autoscaler to automatically start and stop nodes as they're needed.  
C. Commit to a specific level of spending to get discounted prices.  
D. Use PriorityClasses so that the weekly batch job gets priority.

**✅ Answer: B**

---

**Q13.** Kubernetes ___ allows you to automatically manage the number of nodes in your cluster to meet demand.

A. Node Autoscaler  
B. Cluster Autoscaler  
C. Horizontal Pod Autoscaler  
D. Vertical Pod Autoscaler

**✅ Answer: B**

---

**Q52.** What factors influence the Kubernetes scheduler when it places Pods on nodes?

A. Pod memory requests, node taints, and Pod affinity.  
B. Pod labels, node labels, and request labels.  
C. Node taints, node level, and Pod priority.  
D. Pod priority, container command, and node labels.

**✅ Answer: A**

---

**Q90.** What does vertical scaling an application deployment describe best?

A. The act of adding/removing applications to meet demand.  
B. The act of adding/removing node instances to the cluster to meet demand.  
C. The act of adding/removing resources to applications to meet demand.  
D. The act of adding/removing application instances of the same application to meet demand.

**✅ Answer: C**

---

**Q100.** Which of these events will cause the kube-scheduler to assign a Pod to a node?

A. When the Pod crashes because of an error.  
B. When a new node is added to the Kubernetes cluster.  
C. When the CPU load on the node becomes too high.  
D. When a new Pod is created and has no assigned node.

**✅ Answer: D**

---

**Q112.** Which of the following best describes horizontally scaling an application deployment?

A. The act of adding/removing node instances to the cluster to meet demand.  
B. The act of adding/removing applications to meet demand.  
C. The act of adding/removing application instances of the same application to meet demand.  
D. The act of adding/removing resources to application instances to meet demand.

**✅ Answer: C**

---

**Q119.** How does Horizontal Pod autoscaling work in Kubernetes?

A. The HPA controller adds more CPU or memory to the pods when the load is above the configured threshold.  
B. The HPA controller adds more pods when load is above threshold, but does not reduce pods when load is below.  
C. The HPA controller adds more pods to the specified DaemonSet when the load is above threshold.  
D. The HPA controller adds more pods when load is above threshold, and reduces pods when load is below.

**✅ Answer: D**

---

**Q128.** Which of the following options is true about considerations for large Kubernetes clusters?

A. Kubernetes supports up to 1000 nodes and recommends no more than 1000 containers per node.  
B. Kubernetes supports up to 5000 nodes and recommends no more than 500 pods per node.  
C. Kubernetes supports up to 5000 nodes and recommends no more than 110 pods per node.  
D. Kubernetes supports up to 50 nodes and recommends no more than 1000 containers per node.

**✅ Answer: C**

---

**Q146.** What are the two steps performed by the kube-scheduler to select a node to schedule a pod?

A. Grouping and placing  
B. Filtering and selecting  
C. Filtering and scoring  
D. Scoring and creating

**✅ Answer: C**

---

**Q152.** Kubernetes ___ protect you against voluntary interruptions (such as deleting Pods, draining nodes) to run applications in a highly available manner.

A. Pod Topology Spread Constraints  
B. Pod Disruption Budgets  
C. Taints and Tolerances  
D. Resource Limits and Requests

**✅ Answer: B**

---

**Q194.** Which mechanism can be used to automatically adjust the amount of resources for an application?

A. Horizontal Pod Autoscaler (HPA)  
B. Kubernetes Event-driven Autoscaling (KEDA)  
C. Cluster Autoscaler  
D. Vertical Pod Autoscaler (VPA)

**✅ Answer: D**

---

**Q216.** What are the two essential operations that the kube-scheduler normally performs?

A. Pod eviction or starting  
B. Filtering and Scoring nodes  
C. Resources monitoring and reporting  
D. Starting and terminating containers

**✅ Answer: B**

---

**Q217.** Which set of resources can receive automatic updates from the HorizontalPodAutoscaler?

A. DaemonSet, Deployment  
B. Pod, StatefulSet  
C. DaemonSet, StatefulSet  
D. Deployment, StatefulSet

**✅ Answer: D**

---

**Q226.** What happens if only a limit is specified for a resource and no admission-time mechanism has applied a default request?

A. Kubernetes copies the specified limit and uses it as the requested value for the resource.  
B. Kubernetes will create the container but it will fail with CrashLoopBackOff.  
C. Kubernetes chooses a random value and uses it as the requested value for the resource.  
D. Kubernetes does not allow containers to be created without request values, causing eviction.

**✅ Answer: A**

---

**Q235.** What is the primary purpose of a Horizontal Pod Autoscaler (HPA) in Kubernetes?

A. To allocate and manage persistent volumes required by stateful applications.  
B. To track performance metrics and report health status for nodes and Pods.  
C. To automatically scale the number of Pod replicas based on resource utilization.  
D. To coordinate rolling updates of Pods when deploying new application versions.

**✅ Answer: C**

---

**Q241.** In a Kubernetes cluster, what is the primary role of the Kubernetes scheduler?

A. To manage the lifecycle of the Pods by restarting them when they fail.  
B. To monitor the health of the nodes and pods in the cluster.  
C. To handle network traffic between services within the cluster.  
D. To distribute Pods across nodes based on resource availability and constraints.

**✅ Answer: D**

---

**Q242.** A Kubernetes administrator wants to limit the total memory usage of all Pods in a specific namespace to 4Gi. What should they create to enforce this limit?

A. Namespace  
B. LimitRange  
C. ClusterRole  
D. ResourceQuota

**✅ Answer: D**

---

**Q249.** Which field in a Pod or Deployment manifest ensures that Pods are scheduled only on nodes with specific labels?

A. `annotations: disktype: ssd`  
B. `labels: disktype: ssd`  
C. `nodeSelector: disktype: ssd`  
D. `resources: disktype: ssd`

**✅ Answer: C**

---

**Q259.** A Kubernetes deployment must run only on nodes with specific hardware capabilities. What configuration should be used?

A. Use Pod affinity rules in the deployment configuration.  
B. Set a PodDisruptionBudget for the deployment.  
C. Use a nodeSelector in the deployment specification.  
D. Configure resource limits and requests for the Pods.

**✅ Answer: C**

---

## 9. kubectl Commands & Troubleshooting

> Common kubectl commands, flags, debugging, log retrieval, rollouts, port-forwarding.

---

**Q20.** Which is the correct kubectl command to display logs in real time?

A. `kubectl logs -p test-container-1`  
B. `kubectl logs -c test-container-1`  
C. `kubectl logs -l test-container-1`  
D. `kubectl logs -f test-container-1`

**✅ Answer: D** *(`-f` = follow/stream)*

---

**Q45.** Which command will list the resource types that exist within a cluster?

A. `kubectl api-resources`  
B. `kubectl get namespaces`  
C. `kubectl api-versions`  
D. `curl https://kubectrl/namespaces`

**✅ Answer: A**

---

**Q49.** How can you monitor the progress for an updated Deployment/DaemonSets/StatefulSets?

A. `kubectl rollout watch`  
B. `kubectl rollout progress`  
C. `kubectl rollout state`  
D. `kubectl rollout status`

**✅ Answer: D**

---

**Q63.** Which of these commands is used to retrieve the documentation and field definitions for a Kubernetes resource?

A. `kubectl explain`  
B. `kubectl api-resources`  
C. `kubectl get --help`  
D. `kubectl show`

**✅ Answer: A**

---

**Q71.** Which command provides information about the field `replicas` within the `spec` resource of a deployment object?

A. `kubectl get deployment.spec.replicas`  
B. `kubectl explain deployment.spec.replicas`  
C. `kubectl describe deployment.spec.replicas`  
D. `kubectl explain deployment --spec.replicas`

**✅ Answer: B**

---

**Q75.** How do you deploy a workload to Kubernetes without additional tools?

A. Create a Bash script and run it on a worker node.  
B. Create a Helm Chart and install it with helm.  
C. Create a manifest and apply it with kubectl.  
D. Create a Python script and run it with kubectl.

**✅ Answer: C**

---

**Q76.** How do you perform a command in a running container of a Pod?

A. `kubectl exec --`  
B. `docker exec`  
C. `kubectl run --`  
D. `kubectl attach -i`

**✅ Answer: A**

---

**Q84.** What kubectl command is used to retrieve the resource consumption (CPU and memory) for nodes or Pods?

A. `kubectl cluster-info`  
B. `kubectl version`  
C. `kubectl top`  
D. `kubectl api-resources`

**✅ Answer: C**

---

**Q87.** What methods can you use to scale a deployment?

A. With `kubectl edit deployment` exclusively.  
B. With `kubectl scale-up deployment` exclusively.  
C. With `kubectl scale deployment` and `kubectl edit deployment`.  
D. With `kubectl scale deployment` exclusively.

**✅ Answer: C**

---

**Q97.** Which of the following will view the snapshot of previously terminated ruby container logs from Pod web-1?

A. `kubectl logs -p -c ruby web-1`  
B. `kubectl logs -c ruby web-1`  
C. `kubectl logs -p ruby web-1`  
D. `kubectl logs -p -c web-1 ruby`

**✅ Answer: A** *(`-p` = previous, `-c` = container)*

---

**Q124.** Which kubectl command is useful for collecting information about any type of resource that is active in a Kubernetes cluster?

A. describe  
B. list  
C. expose  
D. explain

**✅ Answer: A**

---

**Q126.** Which is the correct kubectl command to run a nginx deployment with 2 replicas?

A. `kubectl run deploy nginx --image=nginx --replicas=2`  
B. `kubectl create deploy nginx --image=nginx --replicas=2`  
C. `kubectl create nginx deployment --image=nginx –replicas=2`  
D. `kubectl create deploy nginx --image=nginx --count=2`

**✅ Answer: B**

---

**Q159.** Which command lists the running containers in the current Kubernetes namespace?

A. `kubectl get pods`  
B. `kubectl ls`  
C. `kubectl ps`  
D. `kubectl show pods`

**✅ Answer: A**

---

**Q168.** If kubectl is failing to retrieve information from the cluster, where can you find pod logs to troubleshoot?

A. `/var/log/pods/`  
B. `~/.kube/config`  
C. `/var/log/k8s/`  
D. `/etc/kubernetes/`

**✅ Answer: A**

---

**Q201.** Which command updates the image of a running deployment without recreating the resource?

A. `kubectl edit deployment my-app my-container=my-app:v2`  
B. `kubectl replace deployment/my-app my-container=my-app:v2`  
C. `kubectl set image deployment/my-app my-container=my-app:v2`  
D. `kubectl set image deployment/my-app rollout restart`

**✅ Answer: C**

---

**Q206.** Which command retrieves container logs from a specific container in a multi-container Pod?

A. `kubectl logs <pod> -c <container-name>`  
B. `kubectl logs <pod> -c <container-name>` *(duplicate in source — both A & B are correct)*  
C. `kubectl exec -- tail /var/log/app.log`  
D. `kubectl describe pod -c`

**✅ Answer: A** *(use `kubectl logs <pod> -c <container>`)*

---

**Q211.** In a Kubernetes environment, which kubectl patch command correctly updates the image of a container in a Deployment without recreating the Deployment resource?

A. `kubectl patch deployment my-app -p '{"containers":[{"name":"app-container","image":"nginx:1.25"}]}'`  
B. `kubectl patch deployment my-app --image=nginx:1.25`  
C. `kubectl patch deployment my-app -p '{"spec":{"containers":[{"name":"app-container","image":"nginx:1.25"}]}}'`  
D. `kubectl patch deployment my-app -p '{"spec":{"template":{"spec":{"containers":[{"name":"app-container","image":"nginx:1.25"}]}}}}'`

**✅ Answer: D**

---

**Q229.** A Pod named my-app must be created to run a simple nginx container. Which kubectl command should be used?

A. `kubectl run my-app --image=nginx`  
B. `kubectl create my-app --image-nginx`  
C. `kubectl create nginx --name=my-app`  
D. `kubectl run nginx --name=my-app`

**✅ Answer: A**

---

**Q230.** What is the correct method to access an internal service by forwarding a local port to a specific port inside a Pod running in a Kubernetes cluster?

A. Load Balancer  
B. `kubectl port-forward`  
C. Gateway API  
D. NodePort

**✅ Answer: B**

---

**Q231.** A Kubernetes Pod is returning a CrashLoopBackOff status. What is the most likely reason for this behavior?

A. The container's image is missing or cannot be pulled.  
B. There are insufficient resources allocated for the Pod.  
C. The Pod is unable to communicate with the Kubernetes API server.  
D. The application inside the container crashed after starting.

**✅ Answer: D**

---

**Q232.** A site reliability engineer needs to temporarily prevent new Pods from being scheduled on node-2, while keeping the existing workloads running without disruption. Which kubectl command should be used?

A. `kubectl pause deployment`  
B. `kubectl cordon node-2`  
C. `kubectl drain node-2`  
D. `kubectl delete node-2`

**✅ Answer: B**

---

**Q237.** What command is often used to troubleshoot a failing Pod?

A. `kubectl inspect`  
B. `kubectl analyze`  
C. `kubectl info`  
D. `kubectl describe`

**✅ Answer: D**

---

**Q240.** Which command is the most efficient way to check the progress of a Deployment rollout and confirm if it has completed successfully?

A. `kubectl logs deployment/my-deployment --all-containers=true`  
B. `kubectl describe deployment my-deployment --namespace=default`  
C. `kubectl rollout status deployment/my-deployment`  
D. `kubectl get deployments --show-labels -o wide`

**✅ Answer: C**

---

**Q245.** A Pod is stuck in the CrashLoopBackoff state. Which is the correct way to troubleshoot this issue?

A. Use `kubectl top pod` to check CPU usage and then scale the Deployment.  
B. Use `kubectl describe pod` to review recent events and then `kubectl logs` to inspect container output.  
C. Use `kubectl get nodes` to verify node capacity and then `kubectl apply -f` to restart the Pod.  
D. Use `kubectl exec -- bash` to connect inside the container and then check `/var/log/kubelet.log`.

**✅ Answer: B**

---

**Q253.** Which of the following commands could a cluster administrator use to prevent Pod scheduling in a node temporarily, for maintenance purposes?

A. `kubectl cordon`  
B. `kubectl annotate`  
C. `kubectl label`  
D. `kubectl taint`

**✅ Answer: A**

---

**Q257.** What command would be used to check the revisions of Deployment web-app?

A. `kubectl rollout history deployment/web-app`  
B. `kubectl rollout status deployment/web-app`  
C. `kubectl get history deployment/web-app`  
D. `kubectl describe rollout deployment/web-app`

**✅ Answer: A**

---

**Q261.** A Pod was created that shows the ErrImagePull status. What is most the cause of the error?

A. Kubelet is not running properly on the node.  
B. Kube-scheduler cannot find a node to execute the Pod.  
C. The container image cannot be downloaded.  
D. The container image needs to run using hostNetwork.

**✅ Answer: C**

---

**Q267.** DNS issues are suspected inside a Pod. Which command is most useful to verify the problem?

A. `kubectl delete pod` to recreate the Pod and test if the issue resolves.  
B. `kubectl get endpoints` to confirm whether Services have the correct backend addresses.  
C. `kubectl logs` to review application output for errors related to DNS resolution.  
D. `kubectl exec -- nslookup` to check name resolution directly inside the Pod.

**✅ Answer: D**

---

## 10. Helm

> Helm charts, values files, install/upgrade/uninstall commands.

---

**Q19.** What is Helm?

A. An open source dashboard for Kubernetes.  
B. A package manager for Kubernetes applications.  
C. A custom scheduler for Kubernetes.  
D. An end to end testing project for Kubernetes applications.

**✅ Answer: B**

---

**Q117.** Which of the following is a correct definition of a Helm chart?

A. A Helm chart is a collection of YAML files bundled in a tar.gz file and can be applied without decompressing it.  
B. A Helm chart is a collection of JSON files and contains all the resource definitions to run an application on Kubernetes.  
C. A Helm chart is a collection of YAML files that can be applied on Kubernetes by using the kubectl tool.  
D. A Helm chart is similar to a package and contains all the resource definitions to run an application on Kubernetes.

**✅ Answer: D**

---

**Q145.** Which tool is used to streamline installing and managing Kubernetes applications?

A. apt  
B. helm  
C. service  
D. brew

**✅ Answer: B**

---

**Q223.** Which Helm command deletes the release mysql-1234 from the Kubernetes cluster?

A. `helm reject mysql-1234`  
B. `helm erase mysql-1234`  
C. `helm uninstall mysql-1234`  
D. `helm remove mysql-1234`

**✅ Answer: C**

---

**Q239.** Which file in a Helm chart is responsible for defining the default configuration values that can be overridden during deployment?

A. `templates/deployment.yaml`  
B. `Chart.yaml`  
C. `values.yaml`  
D. `requirements.yaml`

**✅ Answer: C**

---

**Q248.** When modifying an existing Helm release to apply new configuration values, which approach is the best practice?

A. Edit the Helm chart source files directly and reapply them.  
B. Use kubectl edit to modify the live release configuration.  
C. Delete the release and reinstall it with the desired configuration.  
D. Use `helm upgrade` with the `--set` flag to apply new values while preserving the release history.

**✅ Answer: D**

---

## 11. GitOps & CI/CD

> GitOps principles, ArgoCD, Flux, CI/CD pipelines, continuous integration, continuous deployment.

---

**Q8.** CI/CD stands for:

A. Continuous Information / Continuous Development  
B. Continuous Integration / Continuous Development  
C. Cloud Integration / Cloud Development  
D. Continuous Integration / Continuous Deployment

**✅ Answer: D**

---

**Q22.** What is the core functionality of GitOps tools like Argo CD and Flux?

A. They track production changes made by a human in a Git repository and generate a human-readable audit trail.  
B. They replace human operations with an agent that tracks Git commands.  
C. They automatically create pull requests when dependencies are outdated.  
D. They continuously compare the desired state in Git with the actual production state and notify or act upon differences.

**✅ Answer: D**

---

**Q101.** What helps an organization to deliver software more securely at a higher velocity?

A. Kubernetes  
B. apt-get  
C. Docker Images  
D. CI/CD Pipeline

**✅ Answer: D**

---

**Q127.** What does "Continuous Integration" mean?

A. The continuous integration and testing of code changes from multiple sources manually.  
B. The continuous integration and testing of code changes from multiple sources via automation.  
C. The continuous integration of changes from one environment to another.  
D. The continuous integration of new tools to support developers in a project.

**✅ Answer: B**

---

**Q131.** What does "continuous" mean in the context of CI/CD?

A. Frequent releases, Manual processes, Repeatable, Fast processing  
B. Periodic releases, Manual processes, Repeatable, Automated Processing  
C. Frequent releases, Automated processes, Repeatable, Fast processing  
D. Periodic releases, Automated processes, Repeatable, Automated processing

**✅ Answer: C**

---

**Q144.** Which cloud native tool keeps Kubernetes clusters in sync with sources of configuration (like Git repositories), and automates updates to configuration when there is new code to deploy?

A. Flux and ArgoCD  
B. GitOps Toolkit  
C. Linkerd and Istio  
D. Helm and Kustomize

**✅ Answer: A**

---

**Q158.** What are the advantages of adopting a GitOps approach for your deployments?

A. Reduce failed deployments, operational costs, and fragile release processes.  
B. Reduce failed deployments, configuration drift, and fragile release processes.  
C. Reduce failed deployments, operational costs, and learn git.  
D. Reduce failed deployments, configuration drift and improve your reputation.

**✅ Answer: B**

---

**Q171.** Which GitOps engine can be used to orchestrate parallel jobs on Kubernetes?

A. Jenkins X  
B. Flagger  
C. Flux  
D. Argo Workflows

**✅ Answer: D**

---

**Q198.** What is Flux constructed with?

A. GitLab Environment Toolkit  
B. GitOps Toolkit  
C. Helm Toolkit  
D. GitHub Actions Toolkit

**✅ Answer: B**

---

**Q254.** A platform engineer wants to ensure that a new microservice is automatically deployed to every cluster registered in Argo CD. Which configuration best achieves this goal?

A. Create an Argo CD ApplicationSet that uses a Git repository containing the microservice manifests.  
B. Set up a Kubernetes CronJob that redeploys the microservice to all registered clusters on a schedule.  
C. Use a Helm chart to package the microservice and manage it with a single Application defined in ArgoCD.  
D. Manually configure every registered cluster with the deployment YAML for installing the microservice.

**✅ Answer: A**

---

**Q256.** Which of the following common practices will be an antipattern when the GitOps workflow goes live?

A. Deploying Kubernetes objects via imperative commands.  
B. Leaving manual comments in the PR comments forum.  
C. Deploying Kubernetes objects via declarative manifests.  
D. Pushing builds with the deploy label.

**✅ Answer: A**

---

**Q260.** Which cloud native tool(s) keeps Kubernetes clusters in sync with sources of configuration (like Git repositories), and automates updates to configuration when there is new code to deploy?

A. Flux and Argo CD  
B. Helm and Kustomize  
C. Linkerd and Istio  
D. Tekton and Jenkins X

**✅ Answer: A**

---

**Q266.** In a Kubernetes environment, how is an Argo CD ApplicationSet configured to ensure continuous deployment of an application?

A. By manually deploying the application using kubectl commands every time changes are made.  
B. By defining the ApplicationSet in a YAML file and linking it to a Git repository containing application manifests.  
C. By defining the ApplicationSet containing deployment parameters and applying it with kubectl.  
D. By using a Helm chart to package the application and deploying it directly without GitOps.

**✅ Answer: B**

---

## 12. Service Mesh

> Istio, Linkerd, Envoy, Service Mesh Interface (SMI), data plane, control plane, mTLS.

---

**Q28.** What is the common standard for Service Meshes?

A. Service Mesh Specification (SMS)  
B. Service Mesh Technology (SMT)  
C. Service Mesh Interface (SMI)  
D. Service Mesh Function (SMF)

**✅ Answer: C**

---

**Q31.** What components are common in a service mesh?

A. tracing and log storage  
B. circuit breaking and Pod scheduling  
C. data plane and runtime plane  
D. service proxy and control plane

**✅ Answer: D**

---

**Q64.** Which of the following is a lightweight tool that manages traffic flows between services, enforces access policies, and aggregates telemetry data, all without requiring changes to application code?

A. NetworkPolicy  
B. Linkerd  
C. kube-proxy  
D. Nginx

**✅ Answer: B**

---

**Q151.** Which of the following scenarios would benefit the most from a service mesh architecture?

A. A few applications with hundreds of pod replicas running in multiple clusters.  
B. Thousands of distributed applications running in a single cluster.  
C. Tens of distributed applications running in multiple clusters.  
D. Thousands of distributed applications running in multiple clusters, each one providing multiple services.

**✅ Answer: D**

---

**Q173.** Which are the core features provided by a service mesh?

A. Authentication and authorization  
B. Distributing and replicating data  
C. Security vulnerability scanning  
D. Configuration management

**✅ Answer: A**

---

**Q197.** What edge and service proxy tool is designed to be integrated with cloud native applications?

A. CoreDNS  
B. CNI  
C. gRPC  
D. Envoy

**✅ Answer: D**

---

**Q205.** Which of the following cloud native proxies is used for ingress/egress in a service mesh and can also serve as an application gateway?

A. Frontend proxy  
B. Kube-proxy  
C. Reverse proxy  
D. Envoy proxy

**✅ Answer: D**

---

**Q207.** Which of the following is a primary use case of Istio in a Kubernetes cluster?

A. Provide service mesh capabilities such as traffic management, observability, and security between services.  
B. To provide secure built-in database management features for application workloads.  
C. To manage and control the versions of container runtimes used on nodes between services.  
D. To provision and manage persistent storage volumes for stateful applications.

**✅ Answer: A**

---

**Q255.** Lightweight network proxies such as Envoy are part of which plane in a service mesh?

A. Management plane  
B. Control plane  
C. Data plane  
D. Gateway plane

**✅ Answer: C**

---

## 13. Container Runtime & Images

> OCI, CRI, containerd, CRI-O, runc, kata, gVisor, Dockerfile, multi-stage builds, image best practices.

---

**Q1.** What native runtime is Open Container Initiative (OCI) compliant?

A. runC  
B. runV  
C. kata-containers  
D. gvisor

**✅ Answer: A**

---

**Q27.** Which group of container runtimes provides additional sandboxed isolation and elevated security?

A. rune, cgroups  
B. docker, containerd  
C. runsc, kata  
D. crun, cri-o

**✅ Answer: C** *(runsc = gVisor, kata = Kata Containers)*

---

**Q47.** Which of the following systems is NOT compatible with the CRI runtime interface standard?

A. CRI-O  
B. dockershim  
C. systemd  
D. containerd

**✅ Answer: C** *(systemd is not a container runtime)*

---

**Q89.** Which is an industry-standard container runtime with an "emphasis" on simplicity, robustness, and portability?

A. cri-o  
B. lxd  
C. containerd  
D. kata-runtime

**✅ Answer: C**

---

**Q93.** What is the purpose of the CRI?

A. To provide runtime integration control when multiple runtimes are used.  
B. Support container replication and scaling on nodes.  
C. Provide an interface allowing Kubernetes to support pluggable container runtimes.  
D. Allow the definition of dynamic resource criteria across containers.

**✅ Answer: C**

---

**Q105.** What is a Dockerfile?

A. A bash script that is used to automatically build a docker image.  
B. A config file that defines which image registry a container should be pushed to.  
C. A text file that contains all the commands a user could call on the command line to assemble an image.  
D. An image layer created by a running container stored on the host.

**✅ Answer: C**

---

**Q109.** What is a best practice to minimize the container image size?

A. Use a DockerFile.  
B. Use multistage builds.  
C. Build images with different tags.  
D. Add a build.sh script.

**✅ Answer: B**

---

**Q115.** The Container Runtime Interface (CRI) defines the protocol for the communication between:

A. The kubelet and the container runtime.  
B. The container runtime and etcd.  
C. The kube-apiserver and the kubelet.  
D. The container runtime and the image registry.

**✅ Answer: A**

---

**Q133.** What is the reference implementation of the OCI runtime specification?

A. lxc  
B. cri-o  
C. runc  
D. docker

**✅ Answer: C**

---

**Q139.** Which of the following sentences is true about container runtimes in Kubernetes?

A. If you let iptables see bridged traffic, you don't need a container runtime.  
B. If you enable IPv4 forwarding, you don't need a container runtime.  
C. Container runtimes are deprecated, you must install CRI on each node.  
D. You must install a container runtime on each node to run pods on it.

**✅ Answer: D**

---

**Q218.** Which option represents best practices when building container images?

A. Use multi-stage builds, pin the base image version to a specific digest, and only install necessary packages.  
B. Use multi stage builds, pin the base image version to a specific digest, and install extra packages just in case.  
C. Avoid multi-stage builds, use the latest tag for image version, and install extra packages just in case.  
D. Use multi stage builds, use the latest tag for image version, and only install necessary packages.

**✅ Answer: A**

---

**Q222.** What does SBOM stand for?

A. Software Bill of Materials  
B. Software Bill Operations Management  
C. System Bill of Materials  
D. Security Baseline for Open Source Management

**✅ Answer: A**

---

**Q247.** What is an important consideration when choosing a base image for a container in a Kubernetes deployment?

A. It should be the largest available image to ensure all dependencies are included.  
B. It should always be the latest version to ensure access to the newest features.  
C. It should be minimal and purpose-built for the application to reduce attack surface and improve performance.  
D. It can be any existing image from the public repository without consideration of its contents.

**✅ Answer: C**

---

**Q252.** In a cloud native environment, how do containerization and virtualization differ in terms of resource management?

A. Containerization consumes more memory than virtualization by default.  
B. Containerization shares the host OS, while virtualization runs a full OS for each instance.  
C. Containerization uses hypervisors to manage resources, while virtualization does not.  
D. Containerization allocates resources per container, virtualization does not isolate them.

**✅ Answer: B**

---

## 14. Cloud Native, CNCF & Open Source

> Cloud native concepts, CNCF, open source governance, serverless, microservices, hybrid cloud, FinOps, SRE, IaC.

---

**Q12.** What is the name of the lightweight Kubernetes distribution built for IoT and edge computing?

A. OpenShift  
B. k3s  
C. RKE  
D. k1s

**✅ Answer: B**

---

**Q15.** In a cloud native world, what does the IaC abbreviation stand for?

A. Infrastructure and Code  
B. Infrastructure as Code  
C. Infrastructure above Code  
D. Infrastructure across Code

**✅ Answer: B**

---

**Q16.** In which framework do the developers no longer have to deal with capacity, deployments, scaling and fault tolerance, and OS?

A. Docker Swarm  
B. Kubernetes  
C. Mesos  
D. Serverless

**✅ Answer: D**

---

**Q17.** Which of the following characteristics is associated with container orchestration?

A. Application message distribution  
B. Dynamic scheduling  
C. Deploying application JAR files  
D. Virtual Machine distribution

**✅ Answer: B**

---

**Q25.** The Kubernetes project work is carried primarily by SIGs. What does SIG stand for?

A. Special Interest Group  
B. Software Installation Guide  
C. Support and Information Group  
D. Strategy Implementation Group

**✅ Answer: A**

---

**Q30.** What best describes cloud native service discovery?

A. It's a mechanism for applications and microservices to locate each other on a network.  
B. It's a procedure for discovering a MAC address, associated with a given IP address.  
C. It's used for automatically assigning IP addresses to devices connected to the network.  
D. It's a protocol that turns human-readable domain names into IP addresses on the Internet.

**✅ Answer: A**

---

**Q34.** Which of the following would fall under the responsibilities of an SRE?

A. Developing a new application feature.  
B. Creating a monitoring baseline for an application.  
C. Submitting a budget for running an application in a cloud.  
D. Writing policy on how to submit a code change.

**✅ Answer: B**

---

**Q41.** What are the characteristics for building every cloud-native application?

A. Resiliency, Operability, Observability, Availability  
B. Resiliency, Containerd, Observability, Agility  
C. Kubernetes, Operability, Observability, Availability  
D. Resiliency, Agility, Operability, Observability

**✅ Answer: D**

---

**Q42.** What does CNCF stand for?

A. Cloud Native Community Foundation  
B. Cloud Native Computing Foundation  
C. Cloud Neutral Computing Foundation  
D. Cloud Neutral Community Foundation

**✅ Answer: B**

---

**Q69.** Which persona is normally responsible for defining, testing, and running an incident management process?

A. Site Reliability Engineers  
B. Project Managers  
C. Application Developers  
D. Quality Engineers

**✅ Answer: A**

---

**Q72.** Which of the following is a responsibility of the governance board of an open source project?

A. Decide about the marketing strategy of the project.  
B. Review the pull requests in the main branch.  
C. Outline the project's "terms of engagement".  
D. Define the license to be used in the project.

**✅ Answer: C**

---

**Q79.** Which of the following are tasks performed by a container orchestration tool?

A. Schedule, scale, and manage the health of containers.  
B. Create images, scale, and manage the health of containers.  
C. Debug applications, and manage the health of containers.  
D. Store images, scale, and manage the health of containers.

**✅ Answer: A**

---

**Q80.** Which of the following is a definition of Hybrid Cloud?

A. An architecture that uses a combination of services running in public and private data centers, only including data centers from the same cloud provider.  
B. A cloud native architecture that uses a combination of services running in public clouds, excluding data centers in different availability zones.  
C. A cloud native architecture that uses a combination of services running in different public and private clouds, including on-premises data centers.  
D. An architecture that uses a combination of services running in public and private data centers, excluding serverless functions.

**✅ Answer: C**

---

**Q82.** Why is Cloud-Native Architecture important?

A. Cloud Native Architecture revolves around containers, microservices and pipelines.  
B. Cloud Native Architecture removes constraints to rapid innovation.  
C. Cloud Native Architecture is modern for application deployment and pipelines.  
D. Cloud Native Architecture is a bleeding edge technology and service.

**✅ Answer: B**

---

**Q92.** What is Serverless computing?

A. A computing method of providing backend services on an as-used basis.  
B. A computing method of providing services for AI and ML operating systems.  
C. A computing method of providing services for quantum computing operating systems.  
D. A computing method of providing services for cloud computing operating systems.

**✅ Answer: A**

---

**Q95.** In CNCF, who develops specifications for industry standards around container formats and runtimes?

A. Open Container Initiative (OCI)  
B. Linux Foundation Certification Group (LFCG)  
C. Container Network Interface (CNI)  
D. Container Runtime Interface (CRI)

**✅ Answer: A**

---

**Q108.** What is the practice of bringing financial accountability to the variable spend model of cloud resources?

A. FaaS  
B. DevOps  
C. CloudCost  
D. FinOps

**✅ Answer: D**

---

**Q123.** In the DevOps framework and culture, who builds, automates, and offers continuous delivery tools for developer teams?

A. Application Users  
B. Application Developers  
C. Platform Engineers  
D. Cluster Operators

**✅ Answer: C**

---

**Q125.** The cloud native architecture centered around microservices provides a strong system that ensures ______________.

A. fallback  
B. resiliency  
C. failover  
D. high reachability

**✅ Answer: B**

---

**Q141.** What is CloudEvents?

A. A specification for describing event data in common formats for Kubernetes network traffic management and cloud providers.  
B. A specification for describing event data in common formats in all cloud providers including major cloud providers.  
C. A specification for describing event data in common formats to provide interoperability across services, platforms and systems.  
D. A Kubernetes specification for describing events data in common formats for iCloud services, iOS platforms and iMac.

**✅ Answer: C**

---

**Q143.** Imagine you're releasing open-source software for the first time. Which of the following is a valid semantic version?

A. 1.0  
B. 2021-10-11  
C. 0.1.0-rc  
D. v1beta1

**✅ Answer: C** *(valid semver: MAJOR.MINOR.PATCH[-pre-release])*

---

**Q149.** In a serverless computing architecture:

A. Users of the cloud provider are charged based on the number of requests to a function.  
B. Serverless functions are incompatible with containerized functions.  
C. Users should make a reservation to the cloud provider based on an estimation of usage.  
D. Containers serving requests are running in the background in idle status.

**✅ Answer: A**

---

**Q160.** Which of the following is a good habit for cloud native cost efficiency?

A. Follow an automated approach to cost optimization, including visibility and forecasting.  
B. Follow manual processes for cost analysis, including visibility and forecasting.  
C. Use only one cloud provider to simplify the cost analysis.  
D. Keep your legacy workloads unchanged, to avoid cloud costs.

**✅ Answer: A**

---

**Q162.** Which of the following is a challenge derived from running cloud native applications?

A. The operational costs of maintaining the data center of the company.  
B. The cost optimization is complex to maintain across different public cloud environments.  
C. The lack of different container images available in public image repositories.  
D. The lack of services provided by the most common public clouds.

**✅ Answer: B**

---

**Q172.** What is the main purpose of the Open Container Initiative (OCI)?

A. Accelerating the adoption of containers and Kubernetes in the industry.  
B. Creating open industry standards around container formats and runtimes.  
C. Creating industry standards around container formats and runtimes for private purposes.  
D. Improving the security of standards around container formats and runtimes.

**✅ Answer: B**

---

**Q175.** Which of the following is the name of a container orchestration software?

A. OpenStack  
B. Docker  
C. Apache Mesos  
D. CRI-O

**✅ Answer: C**

---

**Q181.** In a cloud native environment, who is usually responsible for maintaining the workloads running across the different platforms?

A. The cloud provider.  
B. The Site Reliability Engineering (SRE) team.  
C. The team of developers.  
D. The Support Engineering team (SE).

**✅ Answer: B**

---

**Q184.** What is a cloud native application?

A. It is a monolithic application that has been containerized and is running now on the cloud.  
B. It is an application designed to be scalable and take advantage of services running on the cloud.  
C. It is an application designed to run all its functions in separate containers.  
D. It is any application that runs in a cloud provider and uses its services.

**✅ Answer: B**

---

**Q185.** What's the most adopted way of conflict resolution and decision-making for the open-source projects under the CNCF umbrella?

A. Financial Analysis  
B. Discussion and Voting  
C. Flipism Technique  
D. Project Founder Say

**✅ Answer: B**

---

**Q189.** Why do administrators need a container orchestration tool?

A. To manage the lifecycle of an elevated number of containers.  
B. To assess the security risks of the container images used in production.  
C. To learn how to transform monolithic applications into microservices.  
D. Container orchestration tools such as Kubernetes are the future.

**✅ Answer: A**

---

**Q224.** During a team meeting, a developer mentions the significance of open collaboration in the cloud native ecosystem. Which statement accurately reflects principles of collaborative development and community stewardship?

A. Community events and working groups foster collaboration by bringing people together to share knowledge and build connections.  
B. Maintainers of open source projects act independently to make technical decisions without requiring input from contributors.  
C. Community stewardship emphasizes guiding project growth but does not necessarily include sustainability considerations.  
D. Open source projects succeed when contributors focus on code quality without the overhead of community engagement.

**✅ Answer: A**

---

## 15. Kubernetes API, Extensions & Versions

> API versioning (alpha/beta/stable), CRDs, Aggregation Layer, required YAML fields, REST API objects.

---

**Q11.** How long should a stable API element in Kubernetes be supported (at minimum) after deprecation?

A. 9 months  
B. 24 months  
C. 12 months  
D. 6 months

**✅ Answer: C**

---

**Q24.** We can extend the Kubernetes API with Kubernetes API Aggregation Layer and CRDs. What is CRD?

A. Custom Resource Definition  
B. Custom Restricted Definition  
C. Customized RUST Definition  
D. Custom RUST Definition

**✅ Answer: A**

---

**Q33.** What fields must exist in any Kubernetes object (e.g. YAML) file?

A. apiVersion, kind, metadata  
B. kind, namespace, data  
C. apiVersion, metadata, namespace  
D. kind, metadata, data

**✅ Answer: A**

---

**Q59.** Services and Pods in Kubernetes are ______ objects.

A. JSON  
B. YAML  
C. Java  
D. REST

**✅ Answer: D**

---

**Q96.** Which of the following options includes valid API versions?

A. alpha1v1, beta3v3, v2  
B. alpha1, beta3, v2  
C. v1alpha1, v2beta3, v2  
D. v1alpha1, v2beta3, 2.0

**✅ Answer: C**

---

**Q174.** Which of the following options include only mandatory fields to create a Kubernetes object using a YAML file?

A. apiVersion, template, kind, status  
B. apiVersion, metadata, status, spec  
C. apiVersion, template, kind, spec  
D. apiVersion, metadata, kind, spec

**✅ Answer: D**

---

**Q179.** How can you extend the Kubernetes API?

A. Adding a CustomResourceDefinition or implementing an aggregation layer.  
B. Adding a new version of a resource, for instance v4beta3.  
C. With the command `kubectl extend api`, logged in as an administrator.  
D. Adding the desired API object as a kubelet parameter.

**✅ Answer: A**

---

**Q191.** In Kubernetes, if the API version of a feature is v2beta3, it means that:

A. The version will remain available for all future releases within a Kubernetes major version.  
B. The API may change in incompatible ways in a later software release without notice.  
C. The software is well tested. Enabling a feature is considered safe.  
D. The software may contain bugs. Enabling a feature may expose bugs.

**✅ Answer: C** *(Beta = well-tested; Alpha = may have bugs)*

---

## 16. Cluster Management & Namespaces

> Namespaces, HA etcd topology, cluster sizing, garbage collection, ResourceQuota.

---

**Q35.** What are the initial namespaces that Kubernetes starts with?

A. default, kube-system, kube-public, kube-node-lease  
B. default, system, kube-public  
C. kube-default, kube-system, kube-main, kube-node-lease  
D. kube-default, system, kube-main, kube-primary

**✅ Answer: A**

---

**Q43.** Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called:

A. namespaces  
B. containers  
C. hypervisors  
D. cgroups

**✅ Answer: A**

---

**Q99.** How many hosts are required to set up a highly available Kubernetes cluster when using an external etcd topology?

A. Four hosts. Two for control plane nodes and two for etcd nodes.  
B. Four hosts. One for a control plane node and three for etcd nodes.  
C. Three hosts. The control plane nodes and etcd nodes share the same host.  
D. Six hosts. Three for control plane nodes and three for etcd nodes.

**✅ Answer: D**

---

**Q118.** Which of the following sentences is true about namespaces in Kubernetes?

A. You can create a namespace within another namespace in Kubernetes.  
B. You can create two resources of the same kind and name in a namespace.  
C. The default namespace exists when a new cluster is created.  
D. All the objects in the cluster are namespaced by default.

**✅ Answer: C**

---

**Q177.** What is the minimum of etcd members that are required for a highly available Kubernetes cluster?

A. Two etcd members.  
B. Five etcd members.  
C. Six etcd members.  
D. Three etcd members.

**✅ Answer: D**

---

**Q186.** Which of the following options include resources cleaned by the Kubernetes garbage collection mechanism?

A. Stale or expired CertificateSigningRequests (CSRs) and old deployments.  
B. Nodes deleted by a cloud controller manager and obsolete logs from the kubelet.  
C. Unused container and container images, and obsolete logs from the kubelet.  
D. Terminated pods, completed jobs, and objects without owner references.

**✅ Answer: D**

---

*End of Study Guide — Q271–Q300 were not available in the source material.*
