☸️ Day 08 – ReplicationController, ReplicaSet & Deployment

Kubernetes 40-Days Series — K8s-Playground

📘 Overview

Day 08 focuses on three essential Kubernetes workload controllers:

🔹 ReplicationController (RC)

🔹 ReplicaSet (RS)

🔹 Deployment

These objects ensure scalability, high availability, and automated rollout/rollback of containerized applications.
Today, I created and configured all three, updated replica counts, performed rollouts, viewed revision history, and tested rollback scenarios.

🧩 1. ReplicationController (RC)

🔍 What Is a ReplicationController?

A ReplicationController ensures that a specified number of identical Pods are always running.
Although considered legacy today, understanding RC helps build a foundation for modern workload controllers.

⭐ Key Responsibilities

🔹 Maintains desired replica count

🔹 Replaces failed Pods automatically

🔹 Uses labels + selectors to manage Pods

📝 Sample RC YAML

apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    app: nginx
    env: demo
  template:
    metadata:
      labels:
        app: nginx
        env: demo
    spec:
      containers:
      - name: nginx
        image: nginx:latest

        🧩 2. ReplicaSet (RS)

🔍 What Is a ReplicaSet?

A ReplicaSet is the successor of the ReplicationController.
It uses label selectors and is typically managed through a Deployment.

⭐ Key Responsibilities

🔹 Ensures the correct number of Pod replicas

🔹 Uses set-based selectors

🔹 More flexible than RC

📝 Sample ReplicaSet YAML

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      tier: frontend
  template:
    metadata:
      labels:
        app: nginx
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx:1.25

🧩 3. Deployment

🔍 What Is a Deployment?

A Deployment is the most commonly used workload controller in Kubernetes.
It manages rollouts, rollbacks, scaling, strategy, and ReplicaSets.

⭐ Key Responsibilities

🔹 Declarative Pod/ReplicaSet updates

🔹 Automated rollouts

🔹 Rollback to previous revisions

🔹 Rolling updates & max surge/max unavailable control

📝 Sample Deployment YAML

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

🛠️ Commands Practiced Today:

🔹 ReplicationController
kubectl create -f rc.yaml
kubectl get rc
kubectl delete rc nginx-rc

🔹 ReplicaSet
kubectl create -f rs.yaml
kubectl get rs
kubectl scale rs nginx-rs --replicas=5
kubectl delete rs nginx-rs

🔹 Deployment
kubectl create -f deploy.yaml
kubectl get deployment
kubectl rollout status deployment nginx-deployment
kubectl rollout history deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment
kubectl delete deployment nginx-deployment

🚀 Key Takeaways

🔹 ReplicationController → Legacy but useful for fundamentals

🔹 ReplicaSet → Modern controller ensuring Pod availability

🔹 Deployment → Most powerful & widely used; handles rollout/rollback

🔹 Scaling, rolling updates, and revision history are crucial for production workloads

🔹 Label selectors are the backbone of Kubernetes object management

🏁 Conclusion

Day 08 strengthened my understanding of Kubernetes workload controllers—how Pods are replicated, updated, and managed at scale. These concepts form the foundation for real-world application deployment in Kubernetes clusters.

🔗 References

🔹 Kubernetes Official Docs – https://kubernetes.io/docs/

🔹 Workload Controllers – https://kubernetes.io/docs/concepts/workloads/controllers/

🔹 Deployments – https://kubernetes.io/docs/concepts/workloads/controllers/deployment/