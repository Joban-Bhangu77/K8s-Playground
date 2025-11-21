📘 Day 10: Kubernetes Namespaces, Deployments, and Cross-Namespace Communication

K8s-Playground | Kubernetes 40-Days Series

This hands-on lab, part of the Kubernetes 40-Days Series, provides a practical deep dive into fundamental Kubernetes networking concepts, including isolation via Namespaces, traffic load balancing with ClusterIP Services, and the crucial role of Fully Qualified Domain Names (FQDN) in cross-namespace service resolution.

🎯 Key Objectives

By completing this lab, you will gain hands-on experience and understanding of:

Workload Isolation: Creating and managing isolated environments using Kubernetes Namespaces.

Flat Networking: Verifying flat network behavior (Pod-to-Pod communication) across different namespaces.

Service Load Balancing: Deploying ClusterIP Services and observing their load-balancing capability.

DNS Resolution: Demonstrating the necessity of FQDN (<service-name>.<namespace>.svc.cluster.local) for cross-namespace service access.

Resource Lifecycle: Observing automatic resource cleanup upon Namespace deletion.

🏗️ Lab Architecture

This lab establishes two identical application stacks across two separate namespaces to rigorously test networking and DNS rules.

Component

Namespace ns1

Namespace ns2

Role

Deployment

deploy-ns1 (3x NGINX replicas)

deploy-ns2 (3x NGINX replicas)

Manages scalable, identical workloads.

Service

svc-ns1 (ClusterIP)

svc-ns2 (ClusterIP)

Provides internal, stable IP and load balancing.

Tested Communication

Pod ns1 $\rightarrow$ Pod ns2

Pod ns1 $\rightarrow$ Service ns2 (via FQDN)

Confirms flat networking and DNS scope.

🛠️ Implementation Guide

Prerequisites

A running Kubernetes cluster (e.g., local setup with KinD, Minikube, or a managed service).

kubectl installed and configured.

Basic Linux shell knowledge for curl and exec commands.

Step-by-Step Walkthrough

Execute the following commands sequentially to build and test the environment.

1. Setup Namespaces and Deployments

Create the two isolated environments and the initial workloads.

# 1. Create Namespaces
kubectl create namespace ns1
kubectl create namespace ns2

# Verify creation
kubectl get ns

# 2. Create Deployments (initial 1 replica)
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

# 3. Scale Deployments to 3 Replicas
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

# Verify pods in both namespaces
kubectl get pods -n ns1
kubectl get pods -n ns2


2. Test Flat Networking (Pod-to-Pod)

Kubernetes implements a flat network, meaning every Pod can reach every other Pod directly, regardless of the Namespace boundary.

# A. Get the IP of a pod in ns2 (e.g., pod-ns2-ip)
kubectl get pods -n ns2 -o wide

# B. Select a running pod name from ns1 (e.g., ns1-pod-name)
NS1_POD=$(kubectl get pods -n ns1 -o jsonpath='{.items[0].metadata.name}')

# C. Execute a curl command from the ns1 pod to the ns2 pod IP
kubectl exec -it $NS1_POD -n ns1 -- curl http://<NS2_POD_IP>

# ✔️ Expected Outcome: The NGINX default page is returned, confirming flat pod networking.


3. Setup and Test ClusterIP Services

Services provide a stable entry point and internal load balancing for the replicas within their namespace.

# 1. Create ClusterIP Services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

# 2. Get the Service ClusterIPs
kubectl get svc -n ns1
kubectl get svc -n ns2


4. Test Cross-Namespace DNS Resolution (The FQDN Rule)

Test the difference between short-name DNS (namespace-scoped) and FQDN (cluster-wide).

# Use the same ns1 pod for testing
NS1_POD=$(kubectl get pods -n ns1 -o jsonpath='{.items[0].metadata.name}')

# A. Test using the short service name from a different namespace (ns1 trying to reach ns2 service)
kubectl exec -it $NS1_POD -n ns1 -- curl http://svc-ns2

# ❌ Expected Failure: The service name is not resolved because short DNS names are namespace-scoped.

# B. Test using the Fully Qualified Domain Name (FQDN)
# FQDN structure: <service-name>.<namespace>.svc.cluster.local

kubectl exec -it $NS1_POD -n ns1 -- curl [http://svc-ns2.ns2.svc.cluster.local](http://svc-ns2.ns2.svc.cluster.local)

# ✔️ Expected Success: The NGINX default page is returned. This is the correct way to reach a service across namespaces.

# C. (Optional) Test the local FQDN
kubectl exec -it $NS1_POD -n ns1 -- curl [http://svc-ns1.ns1.svc.cluster.local](http://svc-ns1.ns1.svc.cluster.local)


5. Cleanup

Namespaces are powerful tools for resource lifecycle management; deleting a namespace automatically cascades and removes all contained resources (Deployments, Pods, Services, etc.).

# Delete both namespaces
kubectl delete ns ns1
kubectl delete ns ns2

# Verify they are gone (Status: Terminating or not found)
kubectl get ns


🧠 Key Learnings Summary

Networking: Kubernetes uses a flat network structure that allows Pods to communicate directly across namespace boundaries using their Pod IP addresses.

Isolation: Namespaces provide logical isolation and scope for resource names, not network isolation (which requires NetworkPolicies).

DNS Scope: Service short names (svc-ns2) are only resolvable within their own namespace.

Cross-Namespace Access: To reliably access a service from a different namespace, the Fully Qualified Domain Name (FQDN) must be used, including the service name, namespace, and cluster domain (svc-ns2.ns2.svc.cluster.local).

Cleanup: Deleting a namespace is an efficient, atomic way to clean up all associated resources within a cluster environment.