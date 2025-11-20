# Kubernetes Services: ClusterIP, NodePort and LoadBalancer  
Kubernetes 40-Day Series – Day 09

This document explains Kubernetes Services and includes the hands-on work completed in this session. Services provide stable networking for dynamic Pods and allow internal and external access depending on the Service type.

## Overview

Kubernetes Pods have dynamic IPs and can restart, scale or reschedule at any time. Services provide a stable way to access these Pods using:
- A virtual IP inside the cluster
- A DNS name
- Load balancing across multiple Pods

In this session, the following Service types were explored:
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName (brief overview)

## Kubernetes Service Types

### ClusterIP
- Default Service type
- Accessible only inside the cluster
- Used for backend APIs and internal microservices
- Not reachable from outside the cluster

### NodePort
- Opens a port between 30000 and 32767 on each worker node
- Accessible externally using http://NodeIP:NodePort
- Useful for local clusters, testing or development
- Not recommended for production

### LoadBalancer
- Creates a cloud-managed load balancer
- Provides a public IP
- Ideal for production applications
- Requires a cloud provider such as AWS, Azure or GCP

### ExternalName (Brief)
- Maps a Service to an external DNS name
- No ClusterIP created
- Uses DNS redirection only
- Helpful when connecting to external databases or APIs

## Hands-On Work

### 1. Create the NGINX Deployment

File: myapp-deploy.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx:1.23.4-alpine
          ports:
            - containerPort: 80

Apply:
kubectl apply -f myapp-deploy.yaml
kubectl get pods -o wide

2. Create a ClusterIP Service

File: myapp-svc.yaml

apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  type: ClusterIP
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 80

Apply:
kubectl apply -f myapp-svc.yaml
kubectl get svc myapp

3. Scale the Deployment
kubectl scale deployment myapp --replicas=2
kubectl get pods -o wide

4. Test Internal Access
kubectl run testpod --image=busybox -it --restart=Never -- wget -O- http://myapp


If DNS fails:

kubectl run testpod --image=busybox -it --restart=Never -- wget -O- http://ClusterIP

5. Test External Access (Expected Failure)
wget http://ClusterIP


ClusterIP cannot be accessed from outside the cluster.

6. Convert the Service to NodePort

kubectl edit svc myapp


Change the type to:

type: NodePort


Check the updated Service:

kubectl get svc myapp


A NodePort will appear, for example 32010.

7. Test External Access Using NodePort
wget http://NodeIP:NodePort


For Kind:
wget http://127.0.0.1:NodePort

LoadBalancer Example (Conceptual Only)

apiVersion: v1
kind: Service
metadata:
  name: myapp-lb
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 80

When to Use Each Type

ClusterIP

Internal communication only

NodePort

Local testing or development

LoadBalancer

Production and public access

ExternalName

External DNS mapping

Conclusion

This session demonstrated how Kubernetes Services provide stable access to Pods, enable internal and external connectivity, and support load balancing across replicas. You deployed an application, exposed it internally, validated access, scaled Pods, and enabled external access through NodePort. These concepts form the foundation for advanced Kubernetes networking such as Ingress, service mesh and API gateways.

Medium Post Link

(Add your Medium link here)