Day 10 – Kubernetes Namespaces and Cross-Namespace Communication

Kubernetes 40-Day Learning Series – K8s-Playground

Overview

This lab focuses on Kubernetes namespaces, deployments, services, and DNS-based communication across namespaces. You will validate pod-to-pod and pod-to-service communication and learn how FQDN works in Kubernetes.

This hands-on practice builds foundational networking knowledge required for real-world Kubernetes operations and certifications.

Objectives

Create and use namespaces

Deploy workloads in isolated environments

Test cross-namespace pod connectivity

Scale deployments

Create ClusterIP services

Validate DNS rules across namespaces

Use FQDN for cross-namespace communication

Perform namespace cleanup

Lab Architecture
Namespaces:
  ns1
  ns2

Deployments:
  deploy-ns1 (nginx, 3 replicas)
  deploy-ns2 (nginx, 3 replicas)

Services:
  svc-ns1 (ClusterIP)
  svc-ns2 (ClusterIP)

Tests:
  Pod → Pod
  Pod → Service
  Service → FQDN
  Service name (expected failure across namespaces)

Hands-On Implementation
1. Create Namespaces
kubectl create namespace ns1
kubectl create namespace ns2

2. Create Deployments
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

3. Retrieve Pod IP Addresses
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2

4. Test Pod-to-Pod Communication

Exec into a pod in ns1:

kubectl exec -it <pod-ns1> -n ns1 -- /bin/bash


Curl a pod in ns2:

curl http://<pod-ip-ns2>

5. Scale Deployments to 3 Replicas
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

6. Create ClusterIP Services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

7. Pod to Service Communication

Inside a pod in ns1:

curl http://<svc-ns2-cluster-ip>

8. Service Name Test (Expected Failure)
curl svc-ns2


The request fails because service names only resolve inside their own namespace.

9. Cross-Namespace Communication Using FQDN
curl svc-ns2.ns2.svc.cluster.local
curl svc-ns1.ns1.svc.cluster.local


This works because Kubernetes DNS requires FQDN for cross-namespace lookups.

10. Cleanup
kubectl delete ns ns1
kubectl delete ns ns2

Medium Blog Post

Full hands-on article with screenshots:
(Add your Medium link here)

Key Learnings

Kubernetes uses a flat cluster-wide network model

Pods can communicate across namespaces without additional configs

Service names are namespace-scoped

FQDN is required for cross-namespace service communication

Deployment scaling increases replica availability

Namespaces automatically clean up their resources when deleted

Conclusion

This lab demonstrated how Kubernetes handles networking across namespaces, how services behave, and how DNS resolution works at different scopes. Understanding these fundamentals is crucial before moving into NetworkPolicies, Ingress, and service mesh technologies.