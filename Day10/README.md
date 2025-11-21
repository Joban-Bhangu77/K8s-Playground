🚀 Day 10: Kubernetes Networking — Namespaces and Cross-Namespace Communication

Kubernetes 40-Day Learning Series – K8s-Playground

This hands-on lab is essential for mastering how Kubernetes manages workload isolation, networking, service discovery, and DNS resolution in multi-namespace environments. This exercise validates core concepts crucial for advanced Kubernetes operations and certification readiness.

🎯 Key Learning Goals

By completing this session, you will gain practical expertise in:

Concept

Description

Namespace Isolation

Creating and using Namespaces (ns1, ns2) for logical resource separation.

Flat Network Model

Proving that all Pods communicate directly via IP, regardless of Namespace.

ClusterIP Services

Implementing services (svc-ns1, svc-ns2) for stable, load-balanced internal traffic.

DNS Scoping

Understanding that short service names are Namespace-scoped and fail across boundaries.

FQDN Requirement

Successfully using the Fully Qualified Domain Name (<svc>.<ns>.svc.cluster.local) for cross-namespace service access.

Scaling & Cleanup

Managing workload scaling (1 → 3 replicas) and automatic resource deletion via Namespace removal.

🏗️ Lab Architecture

The environment consists of two identical, isolated application stacks designed to demonstrate and test the boundaries of Kubernetes networking and service discovery rules.

Component

Namespace ns1

Namespace ns2

Role in Testing

Deployment

deploy-ns1 (3x NGINX replicas)

deploy-ns2 (3x NGINX replicas)

Scalable web server targets.

Service

svc-ns1 (ClusterIP)

svc-ns2 (ClusterIP)

Stable endpoint for testing load balancing and DNS.

Tests Performed:

✓ Pod $\rightarrow$ Pod (cross-namespace flat networking)

✓ Pod $\rightarrow$ Service (ClusterIP load balancing)

✓ Service $\rightarrow$ Service using FQDN

✗ Service name resolution across namespaces (expected failure)

🛠️ Hands-On Walkthrough

Prerequisites

A functional Kubernetes cluster (local or cloud-based).

The kubectl command-line tool configured.

Phase 1: Environment Setup

Create all necessary namespaces, deployments, and services.

# 1. Create the two isolated namespaces
kubectl create namespace ns1
kubectl create namespace ns2

# 2. Deploy and scale identical NGINX workloads (3 replicas for load balancing test)
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

# 3. Expose the Deployments using ClusterIP services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

# Verification steps
kubectl get pods -n ns1
kubectl get svc -n ns2


Phase 2: Connectivity Testing

We use a pod in ns1 to initiate tests against resources in ns2.

Test A: Pod-to-Pod Connectivity (Flat Network Check)

# A. Get the name of a Pod in ns1 for execution
NS1_POD=$(kubectl get pods -n ns1 -o jsonpath='{.items[0].metadata.name}')

# B. Get a target Pod IP address from ns2
kubectl get pods -n ns2 -o wide

# C. Curl the ns2 Pod IP from inside the ns1 Pod
kubectl exec -it $NS1_POD -n ns1 -- curl http://<NS2_POD_IP>

# 🎉 Expected Result: Successful connection, confirming flat networking across namespaces.


Test B: Cross-Namespace DNS Resolution (The FQDN Rule)

# Use the ns1 Pod for testing the ns2 Service

# 1. Attempt short name resolution (Expected Failure)
kubectl exec -it $NS1_POD -n ns1 -- curl http://svc-ns2

# ❌ Expected Result: Connection fails/times out. Short DNS names are local to the namespace.

# 2. Use the FQDN for successful resolution
FQDN_SERVICE_NS2="svc-ns2.ns2.svc.cluster.local"
kubectl exec -it $NS1_POD -n ns1 -- curl http://$FQDN_SERVICE_NS2

# 🎉 Expected Result: Successful connection. FQDN is required for cross-namespace service access.


Phase 3: Cleanup Resources

Perform the automatic cascade cleanup by deleting the namespaces.

# Delete both namespaces
kubectl delete ns ns1
kubectl delete ns2

# Verify status (Namespaces should enter 'Terminating' state)
kubectl get ns


🧠 Key Learnings Summary

DNS Hierarchy: Service names are scoped to the namespace, requiring the service.namespace.svc.cluster.local FQDN structure for external access.

Networking Default: Kubernetes provides a flat network, meaning you can always use Pod IPs directly, even across namespaces (though this is not best practice).

Best Practice: Always use FQDN or the local short name when addressing services to leverage load balancing and stability provided by the Service resource.

Resource Management: Namespace deletion is the simplest, safest way to tear down a logically grouped set of application components.

[Optional: Link to full blog post]
Full detailed walkthrough with screenshots: [Add your Medium link here]