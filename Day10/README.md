# Day 10 – Kubernetes Namespaces and Cross-Namespace Communication

Welcome to Day 10 of the Kubernetes 40-Day Series. In this session, we worked with namespaces, deployments, services, and DNS to understand how workloads communicate inside and across namespaces in a Kubernetes cluster.

## Why Namespaces Matter

Namespaces allow you to group, isolate, and manage Kubernetes resources in a clean and organized way. As clusters grow, namespaces prevent resource collisions, simplify access control, and support multi-environment setups such as dev, staging, and production.

Namespaces enable you to:

- Organize resources by project, team, or environment
- Prevent naming conflicts within the same cluster
- Apply RBAC for access control
- Use ResourceQuotas to limit usage
- Separate test and production workloads logically

Some resources in Kubernetes are namespaced, while others exist at the cluster level.

## How Namespace Scoping Works

A namespaced object only exists inside the namespace it was created in. Two namespaces can contain objects with the same name without conflict.

If you don’t specify a namespace when creating resources, they go into the default namespace.

Kubernetes includes several built-in namespaces:

- default
- kube-system
- kube-public
- kube-node-lease

## Namespaces and DNS

Service names resolve only inside their own namespace. To access a service in a different namespace, you must use its Fully Qualified Domain Name (FQDN):

<service-name>.<namespace>.svc.cluster.local


Short names like `service-name` only work within the same namespace.

## Hands-On Work (Day 10 Lab)

### 1. Create two namespaces

```bash
kubectl create namespace ns1
kubectl create namespace ns2

2. Create a Deployment in each namespace
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

3. Retrieve Pod IP addresses
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2

4. Test Pod-to-Pod communication

Execute into a Pod in ns1:

kubectl exec -it <pod-from-ns1> -n ns1 -- /bin/bash


From inside the pod:

curl http://<pod-ip-in-ns2>

5. Scale the deployments
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

6. Create ClusterIP Services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

7. Test Pod-to-Service communication (using service IP)
curl http://<svc-ns2-clusterip>

8. Test service name resolution across namespaces (expected failure)
curl svc-ns2

9. Use FQDN for cross-namespace access
curl svc-ns2.ns2.svc.cluster.local

10. Cleanup
kubectl delete ns ns1
kubectl delete ns ns2

Key Takeaways

Namespaces provide logical separation but do not restrict network traffic by default.

Pod IPs are reachable across the entire cluster.

Service names resolve only inside their own namespace.

FQDN is required for cross-namespace communication.

ClusterIP services load-balance traffic across pod replicas.

Namespace deletion removes all resources within it.

Medium Blog Post

Full documentation with screenshots is available on Medium:
(https://medium.com/@jobanjitsinghamritsar/day-10-kubernetes-networking-namespaces-cross-namespace-communication-hands-on-lab-f9b5694cd5ab)

Conclusion

Day 10 provided clear insight into how namespaces, networking, and DNS work inside Kubernetes. Understanding pod communication, service resolution, and FQDN is essential before advancing to NetworkPolicies, Ingress, and service mesh topics. This hands-on exercise builds the networking foundation required for real-world Kubernetes operations.