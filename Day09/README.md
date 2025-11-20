# 🚀 Kubernetes Services: ClusterIP, NodePort, and LoadBalancer

## Day 09: Kubernetes 40-Day Series

This document serves as the full reference and hands-on guide for the exploration of **Kubernetes Services**. Services are a critical component of Kubernetes networking, providing **stable access** and **load balancing** for dynamic, ephemeral Pods.

---

## 🎯 Core Concepts: Why Services?

Kubernetes **Pods** are designed to be short-lived, with dynamic IP addresses that can change upon restarting, scaling, or rescheduling. A **Service** abstracts this instability, offering a consistent access point:

* **Virtual IP:** A stable IP address inside the cluster.
* **DNS Name:** A recognizable name (e.g., `http://myapp`) for internal resolution.
* **Load Balancing:** Traffic distribution across the target Pod replicas.



---

## 🔧 Understanding Kubernetes Service Types

The choice of Service type determines the accessibility and deployment context of your application.

| Service Type | Primary Access | Use Case | Accessibility |
| :--- | :--- | :--- | :--- |
| **ClusterIP** (Default) | Internal Cluster | Backend APIs, internal microservices, database connections. | Only inside the cluster. |
| **NodePort** | Node IP and Static Port | Local development, testing, non-production environments. | $\text{http://<NodeIP>:\text{<NodePort>}}$ |
| **LoadBalancer** | Cloud-managed Public IP | Production applications requiring public internet exposure. | Public internet access. **Requires a cloud provider.** |
| **ExternalName** | External DNS Name | Connecting to external services (e.g., cloud databases, external APIs). | DNS redirection only (no proxying). |

### Detailed Breakdown

### **1. ClusterIP (Default)** 🏠

* **Function:** Exposes the Service on an internal cluster-wide IP address.
* **Access:** Only reachable from **within** the cluster.
* **Mechanism:** Routes traffic to target Pods on $\text{<ClusterIP>}:\text{<Port>}$.

### **2. NodePort** 🚪

* **Function:** Builds on ClusterIP by opening a static port (default range: **30000-32767**) on **every worker node** in the cluster.
* **Access:** Accessible externally using $\text{http://<NodeIP>:\text{<NodePort>}}$.
* **Caveat:** Generally **not recommended for production** due to port constraints and reliance on specific node IPs.

### **3. LoadBalancer** ☁️

* **Function:** Provisioned a cloud-native **Load Balancer** (e.g., AWS ELB, Azure Load Balancer).
* **Access:** Provides a publicly routable, stable **External IP**.
* **Requirement:** Requires the Kubernetes cluster to be running on a supporting cloud platform.

### **4. ExternalName** (Brief) 🗺️

* **Function:** Does not proxy traffic. Instead, it maps the Service to a content of the "externalName" field (a DNS name).
* **Mechanism:** Returns a $\text{CNAME}$ record of the external DNS name.

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
