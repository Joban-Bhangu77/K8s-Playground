# Kubernetes Services: ClusterIP, NodePort and LoadBalancer  
Kubernetes 40-Day Series – Day 08

This document covers Kubernetes Services with a full hands-on implementation. Services provide stable networking for dynamic Pods and ensure reliable internal and external access depending on the Service type.

---

## Overview

Kubernetes Pods frequently restart, reschedule, and change IP addresses. To avoid communication issues, Kubernetes uses Services to provide:

- A stable virtual IP (ClusterIP)
- A DNS name
- Load balancing across multiple Pods
- Consistent networking even when Pods change

This session explored:
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName (brief overview)

---

## Kubernetes Service Types

### ClusterIP
- Default Service type  
- Accessible only inside the cluster  
- Ideal for backend APIs and internal microservices  
- Not reachable externally  

### NodePort
- Exposes a port between 30000–32767 on each node  
- Accessible using http://NodeIP:NodePort  
- Useful for local clusters, testing and development  
- Not recommended for production  

### LoadBalancer
- Creates a cloud-managed load balancer  
- Provides a public IP address  
- Ideal for production and internet-facing workloads  
- Requires AWS, Azure, GCP or another cloud provider  

### ExternalName
- Maps a Service to an external DNS name  
- No ClusterIP created  
- DNS redirection only  
- Used when connecting to external databases or APIs  

---

## Hands-On Work

Below are the exact steps performed in this session.

---

### Step 1: Create the NGINX Deployment

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


---

## 💻 Hands-On Work

### 1. Create the NGINX Deployment

We start by deploying a basic NGINX application.

**File: `myapp-deploy.yaml`**

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

            pply and Verify:

Apply and Verify:
kubectl apply -f myapp-deploy.yaml
kubectl get pods -o wide

2. Expose Internally with ClusterIP (Default)
This Service routes internal traffic on port 80 to the Pods on their container port 80.

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

Apply and Verify:
kubectl apply -f myapp-svc.yaml
kubectl get svc myapp

Note the assigned ClusterIP.

3. Test Internal Access and Load Balancing
Use a temporary Pod to confirm the service is reachable via its DNS name and is load balancing across the two replicas.
# Test internal access via DNS
kubectl run test-client --image=busybox -it --rm --restart=Never -- wget -O- http://myapp

# Test external access (This will FAIL as ClusterIP is not external)
wget http://<ClusterIP>

4. Enable External Access via NodePort

We modify the existing service to switch its type to NodePort. This will automatically assign a high-numbered port (30000-32767) on all cluster nodes.
kubectl edit svc myapp

kubectl edit svc myapp

Change the type field from ClusterIP to NodePort.

Check the Updated Service:
kubectl get svc myapp

Find the NodePort in the PORTS column (e.g., 80:32010/TCP).

Test External Access Using NodePort:Access the service using any worker node's IP address and the assigned NodePort.Syntax: $\text{http://<NodeIP>:\text{<NodePort>}}$

# Example for local clusters (Minikube/Kind, often using 127.0.0.1 or host IP)
wget http://<NodeIP>:<NodePort>

5. LoadBalancer Example (Conceptual)
In a cloud environment, you would use this manifest to automatically provision a cloud-managed load balancer with a public IP address.

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

When deployed on a cloud cluster, kubectl get svc myapp-lb would show an External-IP provisioned by the cloud provider.

🚀 Conclusion
Services are the backbone of Kubernetes networking, ensuring high availability and seamless communication. Mastering ClusterIP (internal), NodePort (node-level external), and LoadBalancer (cloud-level external) prepares you for building scalable, production-ready applications.
