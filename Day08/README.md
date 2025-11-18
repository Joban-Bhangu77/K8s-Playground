📘 Day 08 – Kubernetes ReplicationController, ReplicaSet & Deployment

Kubernetes 40-Days Series — K8s-Playground

🚀 Overview

Day 08 focuses on three fundamental Kubernetes workload controllers:

ReplicationController (RC)

ReplicaSet (RS)

Deployment

These controllers ensure scalability, high availability, and reliable updates across applications running inside Kubernetes clusters.
Today’s work included creating and validating these controllers, scaling workloads, updating images, managing rollouts, and performing rollbacks.

🧩 1. ReplicationController (RC)
What is a ReplicationController?

A ReplicationController ensures that a specified number of Pod replicas are always running.
Although older and mostly replaced by ReplicaSets, it remains valuable for understanding Kubernetes' evolution.

Key Features

Ensures a fixed number of replicas are running

Replaces failed Pods automatically

Uses label-based selectors

Legacy controller (superseded by ReplicaSet)

Sample RC YAML
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
        image: nginx:1.7.9
        ports:
        - containerPort: 80

🧩 2. ReplicaSet (RS)
What is a ReplicaSet?

A ReplicaSet is the modern and improved version of a ReplicationController.
It maintains a stable set of running Pods and is commonly used underneath Deployments.

Key Features

Ensures desired number of Pods

Uses set-based selectors

Automatically replaces Pods

Forms the backend for Deployments

Sample ReplicaSet YAML
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    matchLabels:
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
        image: nginx:1.7.9
        ports:
        - containerPort: 80

🧩 3. Deployment
What is a Deployment?

A Deployment is the most advanced and widely used Kubernetes controller. It provides a declarative way to manage application updates while ensuring zero downtime.

Key Features

Declarative updates

Rollback support

Rollout history tracking

Automatic strategy management (rolling updates)

Scales applications seamlessly

Manages underlying ReplicaSets

🛠️ Task 8/40 – Practical Hands-On Exercises
✅ ReplicaSet Practice
1. Create a ReplicaSet with 3 replicas
kubectl apply -f nginx-rs.yaml
kubectl get rs
kubectl get po

2. Update replicas to 4 (via YAML)

Edit YAML:

replicas: 4


Apply:

kubectl apply -f nginx-rs.yaml

3. Update replicas to 6 (via command line)
kubectl scale rs nginx-rs --replicas=6
kubectl get rs

✅ Deployment Practice
1. Create a Deployment using nginx:1.23.0 (3 replicas)
kubectl create deployment nginx \
  --image=nginx:1.23.0 \
  --replicas=3 \
  --dry-run=client -o yaml > nginx-deploy.yaml


Add labels:

metadata:
  labels:
    tier: backend
template:
  metadata:
    labels:
      app: v1


Apply:

kubectl apply -f nginx-deploy.yaml
kubectl get deploy
kubectl get po

2. Update image to nginx:1.23.4
kubectl set image deploy/nginx nginx=nginx:1.23.4
kubectl rollout status deploy/nginx

3. Add change cause annotation
kubectl annotate deploy/nginx kubernetes.io/change-cause="Pick up patch version"

4. Scale Deployment to 5 replicas
kubectl scale deploy/nginx --replicas=5
kubectl get deploy

5. View rollout history
kubectl rollout history deploy/nginx

6. Roll back to Revision 1
kubectl rollout undo deploy/nginx --to-revision=1


Verify:

kubectl get po -o wide


Expected image:

nginx:1.23.0

🔍 Troubleshooting Commands (Quick Reference)
🔹 Deployment
kubectl get deploy
kubectl describe deploy <name>
kubectl rollout status deploy/<name>
kubectl rollout history deploy/<name>
kubectl rollout undo deploy/<name>
kubectl set image deploy/<name> container=image:tag
kubectl scale deploy/<name> --replicas=X

🔹 ReplicaSet
kubectl get rs
kubectl describe rs <name>
kubectl scale rs <name> --replicas=X

🔹 ReplicationController
kubectl get rc
kubectl describe rc <name>
kubectl delete rc <name>

🔹 Pods
kubectl get po -o wide
kubectl describe po <name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/bash

🏁 Conclusion

Day 08 strengthened understanding of Kubernetes workload controllers:

ReplicationController — legacy controller ensuring Pod replication

ReplicaSet — improved version with set-based selectors

Deployment — most advanced, supports rollouts, rollbacks, scaling, and strategy management

By completing these exercises, you learned how to:

✔ Create and scale workloads
✔ Update container images
✔ Monitor rollouts
✔ View revision history
✔ Perform rollbacks
✔ Fix YAML and selector issues
✔ Troubleshoot using kubectl

These concepts are critical for real-world production Kubernetes deployments, ensuring reliability, availability, and zero-downtime updates.

📚 References

Kubernetes Documentation — https://kubernetes.io/docs/home/

Deployments — https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

ReplicaSets — https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

Kubectl Cheat Sheet — https://kubernetes.io/docs/reference/kubectl/cheatsheet/