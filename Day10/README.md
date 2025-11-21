📘 Day 10 – Kubernetes Namespaces, Deployments & Cross-Namespace Communication

Kubernetes 40-Days Series – K8s-Playground

📝 Overview

Day 10 focuses on hands-on Kubernetes networking using namespaces, deployments, ClusterIP services, and DNS resolution across namespaces.
This practical lab demonstrates how workloads communicate inside Kubernetes and how DNS naming works across different scopes.

You will learn:

How to create & isolate workloads using namespaces

How Kubernetes networking is flat, enabling pod-to-pod communication

How services load-balance traffic across replicas

Why service names don’t resolve across namespaces

How to use FQDN for cross-namespace communication

How Kubernetes cleans up resources when namespaces are deleted

🎯 Objectives

By the end of this lab, you will be able to:

Create and manage Kubernetes namespaces

Deploy workloads inside isolated namespaces

Retrieve pod IPs and test pod-level connectivity

Scale deployments and observe replica behavior

Create ClusterIP services and test service communication

Use DNS FQDN for cross-namespace service access

Cleanly delete namespaces with all underlying resources

🔗 Medium Blog Post

I have documented this entire Day 10 practical lab with detailed explanations and screenshots in my Medium blog.

👉 Medium Blog (Full Hands-On Walkthrough): [https://medium.com/@jobanjitsinghamritsar/day-10-kubernetes-networking-namespaces-cross-namespace-communication-hands-on-lab-f9b5694cd5ab]

🏗️ Lab Architecture

This lab creates the following structure:

Namespaces:
  ├── ns1
  └── ns2

Deployments:
  ├── deploy-ns1 (nginx, 3 replicas)
  └── deploy-ns2 (nginx, 3 replicas)

Services:
  ├── svc-ns1 (ClusterIP)
  └── svc-ns2 (ClusterIP)

Communication Tested:
  • Pod → Pod (cross-namespace)
  • Pod → Service (ClusterIP IP)
  • Service → FQDN (cross-namespace DNS)

🛠️ Step-by-Step Implementation
1️⃣ Create Namespaces
kubectl create namespace ns1
kubectl create namespace ns2
kubectl get ns

2️⃣ Create Deployments in Each Namespace
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2
kubectl get pods -n ns1
kubectl get pods -n ns2

3️⃣ Get Pod IP Addresses
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2


Use these IPs for testing pod communication.

4️⃣ Test Pod-to-Pod Communication

Exec into a pod in ns1:

kubectl exec -it <pod-name> -n ns1 -- /bin/bash


Curl a pod in ns2:

curl http://<ns2-pod-ip>


✔️ Expected: NGINX default page output.
This confirms Kubernetes provides flat networking.

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

Exec into a pod in ns1:

kubectl exec -it <pod-name> -n ns1 -- /bin/bash


Curl ns2 service:

curl http://<svc-ns2-cluster-ip>


✔️ Expected: NGINX HTML output.

8️⃣ Test Service Name Resolution (Expected Failure)
curl svc-ns2


❌ Fails because service names are namespace-scoped.

9️⃣ Use FQDN to Reach Services Across Namespaces
curl svc-ns2.ns2.svc.cluster.local
curl svc-ns1.ns1.svc.cluster.local


✔️ Works successfully because FQDN includes namespace and cluster domain.

🔟 Cleanup (Delete Namespaces)
kubectl delete ns ns1
kubectl delete ns ns2
kubectl get ns


All deployments, pods, and services inside these namespaces are deleted automatically.

🧠 Key Learnings

Kubernetes networking is flat, enabling pod-to-pod communication across namespaces.

Deployments ensure replica management and scalability.

ClusterIP services load-balance internal traffic.

DNS short names resolve only inside the same namespace.

FQDN enables cross-namespace service communication.

Namespace deletion removes all underlying resources automatically.

🏁 Conclusion

This lab provided a complete hands-on understanding of Kubernetes namespace isolation, networking behavior, and DNS rules.
Testing pod-to-pod, pod-to-service, and service-to-service communication clarified how Kubernetes networking works behind the scenes.
These concepts form the foundation for advanced topics such as:

NetworkPolicies

Ingress controllers

Service Mesh (Istio/Linkerd)

Multi-tenant cluster security

Day 10 significantly strengthened my confidence in designing, testing, and debugging Kubernetes networking in real environments.

📚 References

Kubernetes Documentation: https://kubernetes.io/docs

Services and Networking: https://kubernetes.io/docs/concepts/services-networking/

DNS for Services: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/