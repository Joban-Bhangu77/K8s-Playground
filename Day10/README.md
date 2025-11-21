# Day 10 – Kubernetes Namespaces and Cross-Namespace Communication
Kubernetes 40-Day Learning Series – K8s-Playground

## Overview
This lab focuses on Kubernetes namespaces, deployments, services, and DNS-based communication across namespaces.  
You validate:

- Pod-to-pod communication across namespaces
- Pod-to-service communication using ClusterIP
- DNS behavior and service name resolution
- Use of FQDN (Fully Qualified Domain Name) for cross-namespace access
- Namespace cleanup and automatic resource deletion

This exercise strengthens your understanding of Kubernetes networking and prepares you for real-world multi-namespace environments.

## Objectives
By the end of this lab, you will be able to:

- Create and use Kubernetes namespaces
- Deploy workloads into specific namespaces
- Retrieve pod IPs and test connectivity
- Scale deployments and verify replicas
- Expose deployments using ClusterIP services
- Understand why service names are namespace-scoped
- Use FQDN to access services across namespaces
- Clean up namespaces and their resources

## Lab Architecture

Namespaces:
- ns1
- ns2

Deployments:
- deploy-ns1 (nginx, scaled to 3 replicas)
- deploy-ns2 (nginx, scaled to 3 replicas)

Services:
- svc-ns1 (ClusterIP)
- svc-ns2 (ClusterIP)

Scenarios tested:
- Pod to pod (IP-based, cross-namespace)
- Pod to service (ClusterIP)
- Service access using FQDN across namespaces
- Service name access across namespaces (expected failure)

## Steps Performed

### 1. Create namespaces

```bash
kubectl create namespace ns1
kubectl create namespace ns2

kubectl get ns

2. Create deployments in each namespace
kubectl create deployment deploy-ns1 --image=nginx -n ns1
kubectl create deployment deploy-ns2 --image=nginx -n ns2

kubectl get pods -n ns1
kubectl get pods -n ns2

3. Get pod IP addresses
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2


Use the IP addresses shown here for curl tests.

4. Test pod-to-pod communication

Exec into a pod in ns1:

kubectl exec -it <pod-name-in-ns1> -n ns1 -- /bin/bash


From inside the pod, curl a pod IP in ns2:

curl http://<pod-ip-in-ns2>


You should receive the default NGINX HTML response.

5. Scale deployments to three replicas
kubectl scale deployment deploy-ns1 --replicas=3 -n ns1
kubectl scale deployment deploy-ns2 --replicas=3 -n ns2

kubectl get pods -n ns1
kubectl get pods -n ns2


Each namespace should now have three pods.

6. Create ClusterIP services
kubectl expose deployment deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deployment deploy-ns2 --name=svc-ns2 --port=80 -n ns2

kubectl get svc -n ns1
kubectl get svc -n ns2


Note the ClusterIP of each service.

7. Test pod-to-service communication (using service IP)

Exec into a pod in ns1:

kubectl exec -it <pod-name-in-ns1> -n ns1 -- /bin/bash


From inside that pod, curl the ClusterIP of svc-ns2:

curl http://<svc-ns2-cluster-ip>


You should again see the NGINX default page.
This confirms that ClusterIP services are reachable cluster-wide.

8. Test service name resolution across namespaces (expected failure)

Still inside the ns1 pod, run:

curl svc-ns2


This should fail with a message similar to:

"Could not resolve host"
or

"Could not resolve dns name"

This happens because service names are only resolvable inside their own namespace.

9. Use FQDN for cross-namespace service access

Kubernetes service FQDN format is:

<service-name>.<namespace>.svc.cluster.local


From the ns1 pod:

curl svc-ns2.ns2.svc.cluster.local


From a pod in ns2 (similar test the other way):

curl svc-ns1.ns1.svc.cluster.local


Both commands should succeed and return the NGINX HTML page.

10. Cleanup

When the lab is complete, delete both namespaces:

kubectl delete ns ns1
kubectl delete ns ns2


This will remove all deployments, pods, and services created inside them.

Medium Blog Post

I have documented this lab with a full hands-on explanation and screenshots on Medium:

Medium article: [Add your Medium link here]

Key Learnings

Kubernetes uses a flat network model; pods can reach each other across namespaces.

Namespaces isolate resources logically but do not block network traffic by default.

ClusterIP services provide stable virtual IPs and load balancing across replicas.

DNS service names are namespace-scoped and do not resolve across namespaces.

FQDN is required for cross-namespace service discovery.

Deleting a namespace automatically deletes all underlying resources.

Conclusion

This Day 10 lab improved understanding of how Kubernetes handles networking, services, and DNS in multi-namespace setups.
By testing pod-to-pod, pod-to-service, and FQDN-based communication, you now have a clear, practical picture of how applications talk to each other inside a Kubernetes cluster.

These fundamentals are essential before moving on to more advanced topics such as NetworkPolicies, Ingress controllers, service mesh, and secure multi-tenant cluster design.