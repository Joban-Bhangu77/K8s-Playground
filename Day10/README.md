🚀 Day 10 – Kubernetes Namespaces & Cross-Namespace Communication
Kubernetes 40-Day Learning Series – K8s-Playground
📘 Overview

This lab explores how Kubernetes handles namespaces, pod networking, ClusterIP services, and DNS resolution across namespaces.
Through a hands-on practical session, you validate:

Pod-to-pod communication

Pod-to-service communication

DNS behavior across namespaces

FQDN usage in Kubernetes

Scaling workloads

Namespace-level cleanup

This exercise is essential for Kubernetes real-world operations and certifications.

🎯 What You Accomplished Today

Created isolated namespaces (ns1 & ns2)

Deployed NGINX workloads in each namespace

Retrieved pod IPs and tested pod-level communication

Scaled deployments from 1 → 3 replicas

Created ClusterIP services and tested service connectivity

Validated namespace-scoped DNS

Used FQDN for cross-namespace resolution

Deleted namespaces to clean up all resources

🧩 Lab Architecture
ns1 ── deploy-ns1 ── svc-ns1 (ClusterIP)
ns2 ── deploy-ns2 ── svc-ns2 (ClusterIP)

Tests Performed:
✓ Pod → Pod (cross-namespace)
✓ Pod → Service (ClusterIP)
✓ Service → Service using FQDN
✗ Service name resolution across namespaces (expected failure)

🛠️ Hands-On Implementation
1️⃣ Create Namespaces
kubectl create namespace ns1
kubectl create namespace ns2

2️⃣ Deploy NGINX in Each Namespace
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

3️⃣ Retrieve Pod IPs
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2

4️⃣ Test Pod → Pod Communication

Exec into an ns1 pod:

kubectl exec -it <pod-ns1> -n ns1 -- /bin/bash
curl http://<pod-ip-ns2>

5️⃣ Scale Deployments
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

6️⃣ Create ClusterIP Services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

7️⃣ Pod → Service Communication

Inside an ns1 pod:

curl http://<svc-ns2-cluster-ip>

8️⃣ Test Service Name Resolution (Expected Failure)
curl svc-ns2


Service names only resolve within their namespace.

9️⃣ Use FQDN for Cross-Namespace Access
curl svc-ns2.ns2.svc.cluster.local
curl svc-ns1.ns1.svc.cluster.local


✔️ Works using Kubernetes DNS hierarchy.

🔟 Cleanup
kubectl delete ns ns1
kubectl delete ns ns2

🔗 Medium Blog Post

Full detailed walkthrough with screenshots:
👉 Add your Medium link here

🧠 Key Learnings

Kubernetes networking is flat — every pod can reach every other pod

ClusterIP services provide stable endpoints and load balancing

DNS service names are namespace-scoped

FQDN is required for cross-namespace service communication

Scaling deployments increases reliability and load distribution

Namespace deletion automatically removes all internal resources

🏁 Conclusion

This lab built a solid understanding of how Kubernetes manages multi-namespace environments, networking, DNS, and service discovery.
You now understand how applications communicate inside a cluster, how DNS resolution works, and how services behave when interacting across namespaces.

This foundation prepares you for advanced Kubernetes topics:

NetworkPolicies

Ingress controllers

Service Mesh (Istio / Linkerd)

Cluster security and multi-tenant design

Day 10 was a major step forward in mastering Kubernetes networking concepts.