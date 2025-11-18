# ☸️ Day 08 – ReplicationController, ReplicaSet & Deployment

A part of the **Kubernetes 40-Days Series – K8s-Playground**

---

## 📘 Overview

In Kubernetes, workload controllers ensure **availability**, **scalability**, and **lifecycle management** of applications.  
Today’s focus is on the three core controllers that manage Pod replicas:

| **Controller** | **Status** | **Function** | **Key Feature** |
| -------------- | ---------- | ------------ | ---------------- |
| **ReplicationController (RC)** | Legacy | Keeps a desired number of Pods running | Equality-based selectors |
| **ReplicaSet (RS)** | Modern | Ensures correct Pod replica count | Set-based selectors |
| **Deployment** | Standard | Declarative updates & rollbacks | Manages ReplicaSets |

---

## 🧩 1. ReplicationController (RC)

### 🔍 What It Is  
The **ReplicationController** is one of the earliest Kubernetes controllers (now legacy). It introduced the basic idea of maintaining a stable set of replicated Pods.

### ⭐ Key Responsibilities  
- Ensures a fixed number of Pods are always running  
- Recreates Pods when they fail  
- Uses **equality-based selectors**  

### 📝 Sample RC Manifest

```yaml
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
```

---

## 🧩 2. ReplicaSet (RS)

### 🔍 What It Is  
The **ReplicaSet** is the improved and modern replacement for RC. It supports **set-based selectors** and is commonly managed indirectly via Deployments.

### ⭐ Key Responsibilities  
- Maintains the desired number of Pod replicas  
- Supports **in**, **notIn**, **exists** selectors  
- Serves as the backbone for Deployments  

### 📝 Sample ReplicaSet Manifest

```yaml
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
```

---

## 🧩 3. Deployment

### 🔍 What It Is  
The **Deployment** is the most widely used Kubernetes controller. It manages ReplicaSets and provides safe, declarative rollouts and rollbacks.

### ⭐ Key Responsibilities  
- Declarative Pod updates  
- Creates/Maintains ReplicaSets  
- Supports automated rollouts  
- Supports rollbacks to previous revisions  
- Controls availability during updates  

### 📝 Sample Deployment Manifest

```yaml
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
```

---

## 🛠️ Commands Practiced

| **Controller / Component** | **Command Example** | **Purpose** |
| -------------------------- | ------------------- | ----------- |
| RC | `kubectl create -f rc.yaml` | Create ReplicationController |
| RS | `kubectl scale rs nginx-rs --replicas=5` | Scale replicas imperatively |
| Deployment | `kubectl get deployment` | List Deployments |
| Deployment | `kubectl rollout status deployment nginx-deployment` | Check rollout progress |
| Deployment | `kubectl rollout undo deployment nginx-deployment` | Roll back to previous revision |

---

## 🚀 Key Takeaways

- **Hierarchy:** Deployment ➝ ReplicaSet ➝ Pod  
- **Modern Standard:** Use **Deployments** for stateless applications  
- **Selectors Matter:** Controllers rely on label matching  
- **High Availability:** Managed rolling updates and automated recovery  
- **Consistency:** ReplicaSets ensure stable replica counts  

---

## 🏁 Conclusion

Day 08 strengthened the understanding of Kubernetes workload controllers.  
By learning how RC, RS, and Deployments work—and how they evolve from each other—you now understand how Kubernetes ensures **high availability**, **scalable infrastructure**, and **zero-downtime updates** for modern applications.

These concepts set the foundation for more advanced controllers such as:  
- **DaemonSets**  
- **StatefulSets**  
- **Jobs & CronJobs**

---

## 🔗 References

- Kubernetes Controllers  
  https://kubernetes.io/docs/concepts/workloads/controllers/

- ReplicaSet Documentation  
  https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

- Deployment Documentation  
  https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
