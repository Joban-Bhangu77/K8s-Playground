# Day 10 – Kubernetes Namespaces and Cross-Namespace Communication

Welcome to Day 10 of the Kubernetes 40-Day Series. In this session, we focused on understanding how namespaces work in Kubernetes, how workloads operate inside them, and how pods and services communicate across namespace boundaries. Namespaces are one of the most essential concepts in Kubernetes for organizing, isolating, and managing resources in multi-tenant or multi-environment clusters.

## Why Namespaces Matter

As clusters grow, managing all resources in a single default namespace becomes difficult. Namespaces provide logical separation for teams, applications, and environments such as dev, staging, and production. They also help avoid naming conflicts, apply RBAC, and enforce resource quotas.

Namespaces allow you to:

- Organize objects by project, team, or environment
- Prevent naming collisions (resources only need unique names *within* a namespace)
- Apply access control through Role-Based Access Control (RBAC)
- Limit resource usage via ResourceQuotas
- Create isolated virtual environments inside a single cluster

Not all Kubernetes objects are namespaced. Resources like Nodes, PersistentVolumes, ClusterRoles, and StorageClasses exist at the cluster level.

## How Namespace Scoping Works

A namespaced object belongs to one namespace only. For example, you can have two Deployments named "nginx-app" as long as each one is in a different namespace (such as dev and prod). If you create a resource without specifying a namespace, Kubernetes automatically places it in the default namespace.

Kubernetes clusters come with these built-in namespaces:

- **default**: used when no namespace is specified
- **kube-system**: contains system components (kube-dns, kube-proxy, scheduler, etc.)
- **kube-public**: readable by all users; rarely used directly
- **kube-node-lease**: used for node heartbeat tracking

## Namespaces and DNS

Kubernetes provides DNS resolution for services and pods. Service names resolve only inside their own namespace. To communicate across namespaces, you must use the Fully Qualified Domain Name (FQDN):

<service-name>.<namespace>.svc.cluster.local


Example:


curl svc-ns2.ns2.svc.cluster.local


Short names like `svc-ns2` only work inside the ns2 namespace.

## Practical Hands-On Work (Day 10 Lab)

### 1. Create two namespaces



kubectl create namespace ns1
kubectl create namespace ns2


### 2. Create a Deployment in each namespace



kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2


### 3. Retrieve Pod IPs



kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2


Pods in different namespaces can still communicate because Kubernetes uses a flat cluster network.

### 4. Test Pod-to-Pod communication

Exec into a pod in ns1:



kubectl exec -it <pod-name-ns1> -n ns1 -- /bin/bash


Inside the pod, curl the Pod IP in ns2:



curl http://<pod-ip-ns2>


This succeeds because pod networking is cluster-wide.

### 5. Scale both deployments to three replicas



kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2


### 6. Create services for each deployment



kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2


### 7. Test cross-namespace service communication using service IP

Inside a pod in ns1:



curl http://<svc-ns2-cluster-ip>


This works because ClusterIP services are reachable across the cluster.

### 8. Test service name resolution (expected to fail)



curl svc-ns2


This fails because service names are only resolvable within their own namespace.

### 9. Use the FQDN to access services across namespaces



curl svc-ns2.ns2.svc.cluster.local


This works, confirming how Kubernetes DNS resolves cross-namespace service traffic.

### 10. Cleanup



kubectl delete ns ns1
kubectl delete ns ns2


Deleting namespaces automatically removes the deployments, pods, and services inside them.

## Key Takeaways

- Namespaces isolate Kubernetes objects logically but do not block network traffic by default.
- Pod IPs are accessible across the entire cluster.
- Service names work only within the same namespace.
- FQDN is required to access services across namespaces.
- Scaling deployments increases redundancy and load distribution.
- Namespace deletion is an effective cleanup mechanism.
- Namespaces, when combined with RBAC and ResourceQuotas, provide strong multi-tenant isolation.

## Medium Blog Post

Full hands-on documentation with screenshots is available here:  
(Add your Medium link here)

## Conclusion

Day 10 provided a clear understanding of how Kubernetes handles resource organization, pod networking, and DNS resolution across namespaces. This knowledge is essential for building multi-environment, multi-team Kubernetes architectures. With these fundamentals in place, the next steps will involve NetworkPolicies, Ingress controllers, and service mesh concepts to secure and manage traffic flow more effectively.
