Day 08 – Kubernetes Services: ClusterIP, NodePort & LoadBalancer (Hands-On Guide)

Part of the Kubernetes 40-Day Series — K8s-Playground

This README documents the full hands-on work completed for Day 08, focusing on how Kubernetes Services provide stable and reliable networking for dynamic Pods.
In this session, you deployed an NGINX application, exposed it using a Service, validated internal and external connectivity, and explored when to use ClusterIP, NodePort, LoadBalancer, and ExternalName Services.

📘 Overview

Kubernetes Pods are ephemeral — they restart, scale, reschedule, and change IPs frequently. Relying on Pod IPs for communication leads to instability.
Kubernetes solves this through Services, which provide:

A stable virtual IP (ClusterIP)

A consistent DNS name

Load balancing across multiple Pods

Decoupled and resilient networking independent of Pod lifecycle

This day covers:

ClusterIP

NodePort

LoadBalancer

ExternalName

Hands-on internal and external traffic testing

📌 Kubernetes Service Types (Summary)
1. ClusterIP (Default)

Accessible inside the cluster only

For internal microservices, backend APIs

Most secure & commonly used

2. NodePort

Accessible externally at:

http://<NodeIP>:<NodePort>


Great for local clusters (Kind, Minikube), dev/test

Not ideal for production

3. LoadBalancer

Cloud providers (AWS/GCP/Azure) create a public load balancer

Ideal for production and public-access workloads

4. ExternalName

DNS-only mapping to an external hostname

No ClusterIP or proxying

Used to connect Kubernetes workloads to external systems (DBs, APIs)

🛠️ Hands-On Implementation — Day 08 Work

This section outlines every step performed today.

1️⃣ Deploy the NGINX Application

Create myapp-deploy.yaml:

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


✔ One NGINX Pod is running.

2️⃣ Create a ClusterIP Service

Create myapp-svc.yaml:

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


✔ This service is reachable only from inside the cluster.

3️⃣ Scale the Deployment
kubectl scale deployment myapp --replicas=2
kubectl get pods -o wide


✔ Service now load balances between 2 Pods.

4️⃣ Test Internal Access Using BusyBox
kubectl run testpod --image=busybox -it --restart=Never -- wget -O- http://myapp


If DNS fails:

kubectl run testpod --image=busybox -it --restart=Never -- wget -O- http://<CLUSTER-IP>


✔ You receive the NGINX default HTML page.

5️⃣ Attempt External Access (Expected Failure)

From host:

wget http://<CLUSTER-IP>


❌ Expected: Connection fails
Reason: ClusterIP is internal-only.

6️⃣ Convert ClusterIP → NodePort

Edit Service:

kubectl edit svc myapp


Change:

type: ClusterIP


to:

type: NodePort


Save → exit → check:

kubectl get svc myapp


✔ NodePort assigned (example: 32xxx).

7️⃣ Access the Application Externally

From host:

wget http://<NodeIP>:<NodePort>


For Kind:

wget http://127.0.0.1:<NodePort>


✔ Application is now accessible from outside the cluster.

📦 LoadBalancer Service (Conceptual Example)

Used in cloud environments for public access.

Example:

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


✔ Cloud provider assigns a public IP
✔ Ideal for production workloads

📘 When to Use Each Service Type
ClusterIP

Internal access

Backend APIs, microservices

Default & secure

NodePort

Simple external access

Local development & testing

Not production-grade

LoadBalancer

Production internet-facing services

Requires cloud provider integration

Scalable, reliable

ExternalName

DNS mapping to external resources

No proxying or ClusterIP

🟦 Conclusion

Day 08 introduced one of the most important concepts in Kubernetes: Services. You learned how Services abstract Pod IP changes, provide stable DNS names, balance traffic across replicas, and enable both internal and external connectivity.
Through hands-on practice, you deployed an application, exposed it internally, scaled it, tested routing with BusyBox, and enabled external access using NodePort.

With this foundation, you are now ready to explore advanced networking topics such as Ingress controllers, API gateways, TLS termination, service meshes, and network policies.

🔗 Medium Hands-On Article Link (Add After Publishing)

Paste your Medium link here:

🔗 Medium Article:
<ADD YOUR MEDIUM LINK HERE>
