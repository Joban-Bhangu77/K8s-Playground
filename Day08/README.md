☸️ Day 08 – ReplicationController, ReplicaSet & Deployment

Kubernetes 40-Days Series — K8s-Playground

📘 Overview

Day 08 covers three foundational Kubernetes workload controllers:

ReplicationController (RC)

ReplicaSet (RS)

Deployment

These help maintain pod availability, ensure scalability, and support rollout/rollback strategies.

🧩 1. ReplicationController (RC)
🔍 What Is a ReplicationController?

A ReplicationController ensures that a specific number of Pod replicas are always running.
Although considered legacy today, it is important for understanding Kubernetes evolution.

⭐ Key Responsibilities

Maintains the desired number of Pods

Automatically replaces failed Pods

Uses labels and selectors to manage Pods

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

ReplicaSet is the advanced form of ReplicationController.
It supports set-based selectors and is primarily managed via Deployments.

⭐ Key Responsibilities

Ensures the correct number of replicas

More powerful label selectors

Provides better orchestration for Pods

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

Deployment is the most widely used Kubernetes controller.
It manages ReplicaSets, supports declarative updates, and handles rollouts and rollbacks automatically.

⭐ Key Responsibilities

Declarative Pod management

Automated rollouts

Rollbacks to previous revisions

Rolling updates & controlled surge/unavailability

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

🛠️ Commands Practiced Today
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

RC = Old controller, good for learning

RS = Modern controller, replaces RC

Deployment = Full lifecycle management + rollout/rollback

Scaling and rolling updates are critical in production

Label selectors are the foundation of workload controllers

🏁 Conclusion

Today’s session reinforced how Kubernetes ensures high availability and smooth application lifecycle through controllers.
Understanding RC, RS, and Deployment prepares you for advanced concepts like StatefulSets, DaemonSets, and Autoscaling.

🔗 References

Kubernetes Controllers: https://kubernetes.io/docs/concepts/workloads/controllers/

ReplicaSet Docs: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

Deployment Docs: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/