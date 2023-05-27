### Book
- [Certified Kubernetes Administrator CKA Study Guide](https://learning.oreilly.com/library/view/certified-kubernetes-administrator/9781098107215/)
- [Certified Kubernetes Security Specialist (CKS) Study Guide](https://learning.oreilly.com/library/view/certified-kubernetes-security/9781098132965/)

### Course 
[Certified Kubernetes Administrator CKA](https://learning.oreilly.com/videos/certified-kubernetes-administrator/9780138103804/)

### important points
- API resource types include Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob, Pod
- configmap, service, ingress, secrets

### Links can be opened in the exam
- [https://kubernetes.io/docs](https://kubernetes.io/docs)
- [https://kubernetes.io/blog/](https://kubernetes.io/blog/)

### Free exam simulator
[https://killercoda.com/kimwuestkamp/scenario/cks-cka-ckad-remote-desktop](https://killercoda.com/kimwuestkamp/scenario/cks-cka-ckad-remote-desktop)

## Useful commands
```alias k=kubectl```
Use kubectl - create, run

Resource Short names
```kubectl api-resources```

Get info
```k explain pod.spec.volumes```

Finding Object Information
```kubectl describe pods```
```kubectl get pods -o yaml ```

Generate deployment files
```k create deployment mypod1 --image=nginx --dry-run -o yaml > first.yml```

Run a pod
```kubectl run nginx-pod --image=nginx --restart=Never --port=80```

Check if build-robot has privs to list pods
```kubectl auth can-i list pods --as build-robot```

