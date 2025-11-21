📘 Day 10 – Kubernetes Namespaces, Deployments & Cross-Namespace Communication
Kubernetes 40-Day Learning Series – K8s-Playground
📎 Table of Contents

Overview

Objectives

Lab Architecture

Prerequisites

Hands-On Implementation

1. Create Namespaces

2. Create Deployments

3. Retrieve Pod IPs

4. Pod-to-Pod Communication

5. Scale Deployments

6. Create Services

7. Pod-to-Service Communication

8. Service Name DNS Test

9. FQDN-Based Communication

10. Cleanup

Repository Structure

Key Learnings

Medium Blog Post

Conclusion

References

📝 Overview

This lab dives deep into Kubernetes networking fundamentals using namespaces, deployments, ClusterIP services, and DNS resolution.
You will validate how pods and services communicate within and across namespaces, and understand how Kubernetes DNS behaves in multi-namespace environments.

This hands-on session provides real-world experience for both administration and CKA/CKAD exam preparation.

🎯 Objectives

By the end of Day 10, you will:

Understand namespace-based resource isolation

Deploy isolated workloads inside separate namespaces

Test cross-namespace pod-to-pod communication

Scale deployments and observe replica distribution

Create ClusterIP services and validate their behavior

Understand why service names do not resolve across namespaces

Use FQDN for cross-namespace service-to-service communication

Cleanly delete namespaces and their resources

🧩 Lab Architecture
Namespaces
 ├── ns1
 └── ns2

Deployments
 ├── deploy-ns1 (nginx, scaled to 3 replicas)
 └── deploy-ns2 (nginx, scaled to 3 replicas)

Services
 ├── svc-ns1 (ClusterIP)
 └── svc-ns2 (ClusterIP)

Communication Paths Tested
 ├── Pod → Pod (IP-based)
 ├── Pod → Service (ClusterIP)
 └── Service → Service (FQDN-based DNS)

🧰 Prerequisites

Before starting, ensure you have:

A functional Kubernetes cluster

kubectl configured and authenticated

Basic understanding of pods, deployments, and namespaces

🛠️ Hands-On Implementation
1️⃣ Create Namespaces
kubectl create namespace ns1
kubectl create namespace ns2
kubectl get ns

2️⃣ Create Deployments in Each Namespace
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

kubectl get pods -n ns1
kubectl get pods -n ns2


Each namespace now has one NGINX pod.

3️⃣ Get Pod IP Addresses
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2


Record the Pod IPs for testing communication.

4️⃣ Test Pod-to-Pod Communication

Exec into a pod in ns1:

kubectl exec -it <pod-name> -n ns1 -- /bin/bash


Curl pod in ns2:

curl http://<pod-ip-ns2>


✔️ Expected: NGINX default HTML response
Confirms Kubernetes flat networking model.

5️⃣ Scale Deployments to 3 Replicas
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

kubectl get pods -n ns1
kubectl get pods -n ns2

6️⃣ Create ClusterIP Services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

kubectl get svc -n ns1
kubectl get svc -n ns2

7️⃣ Pod → Service Communication (Cross-Namespace)

Inside a pod in ns1:

curl http://<svc-ns2-cluster-ip>


✔️ Expected: NGINX HTML output
ClusterIP is reachable cluster-wide.

8️⃣ Test Service Name Resolution (Expected Failure)
curl svc-ns2


❌ Fails — service names are namespace-scoped.

9️⃣ Use FQDN to Reach Services Across Namespaces

Kubernetes service DNS format:

<service>.<namespace>.svc.cluster.local


Test:

curl svc-ns2.ns2.svc.cluster.local
curl svc-ns1.ns1.svc.cluster.local


✔️ Works across namespaces
Confirms how Kubernetes DNS hierarchy functions.

🔟 Cleanup (Delete Namespaces)
kubectl delete ns ns1
kubectl delete ns ns2


Namespace deletion removes all deployments, pods, and services.

🔗 Medium Blog Post

A detailed hands-on walkthrough with screenshots is available here:

👉 Medium Blog: [https://medium.com/@jobanjitsinghamritsar/day-10-kubernetes-networking-namespaces-cross-namespace-communication-hands-on-lab-f9b5694cd5ab]

🧠 Key Learnings

Kubernetes networking is flat—pods can communicate across namespaces

ClusterIP services provide stable entrypoints and load-balancing

DNS service names resolve only within the same namespace

Cross-namespace communication requires FQDN

Scaling deployments distributes load and increases availability

Namespace deletion automatically clears all resources

🏁 Conclusion

This lab offered a practical, in-depth understanding of how Kubernetes handles networking, DNS, and namespace isolation.
By testing communication at multiple levels—pods, services, and FQDN—you gain real operational insight into how real-world microservices interact inside a Kubernetes cluster.

Mastering these concepts prepares you for advanced networking topics such as:

NetworkPolicies

Ingress & load balancing

Service Mesh (Istio, Linkerd)

Multi-tenant cluster architecture

Day 10 strengthens your foundation for the rest of the Kubernetes journey.

📚 References

Kubernetes Docs – https://kubernetes.io/docs

Services & Networking – https://kubernetes.io/docs/concepts/services-networking/

DNS for Services – https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/