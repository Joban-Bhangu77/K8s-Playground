# 🟦 Day 07: Mastering Kubernetes Pods and YAML ⚙️

## 🎯 Session Overview: Declarative Configuration, YAML, and Debugging

This session, part of the **40 Days Kubernetes Series**, focused on mastering **YAML** as the language of the desired state. We covered the crucial transition from quick **imperative** commands to robust, version-controlled **declarative** manifests and gained practical experience in diagnosing the common `ImagePullBackOff` error.

***

## 📘 Foundations: YAML and the Declarative Model

The **Declarative Approach**—defining the desired state via a YAML manifest—is the standard for production Kubernetes environments, ensuring configurations are **version-controllable, repeatable, and scalable**.

### 1. YAML Structure and Syntax

* **YAML (YAML Ain't Markup Language):** Uses an indentation-based structure.
* **Dictionaries (Maps):** Key-value pairs (`employee: name: Gaurav`).
* **Lists (Arrays):** Denoted by a dash (`-`).
* **Indentation Rule:** Strictly **2 spaces** (no tabs).

### 2. Mandatory Manifest Fields

Every Kubernetes object requires the following four top-level fields:

1.  **`apiVersion`**: Specifies the Kubernetes API version.
2.  **`kind`**: The type of object (e.g., `Pod`, `Deployment`).
3.  **`metadata`**: Information like `name` and **`labels`**.
4.  **`spec`**: Defines the object's **desired state** (e.g., container details).

#### Example Pod Manifest (`pod-definition.yaml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    env: test
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
🚀 Hands-On Tasks & Core Workflow
🏗️ TASK 1: Imperative Pod Creation
Create Pod: kubectl run nginx-pod --image=nginx

Verify Status: kubectl get pods (Output shows STATUS: Running)

Inspect Details: kubectl describe pod nginx-pod

🔄 TASK 2: Converting Imperative to Declarative
Generate YAML Template: Use flags to print the YAML output without deploying:

Bash

kubectl run nginx --image=nginx --dry-run=client -o yaml > new-pod.yaml
Manifest Hygiene (Critical): If exporting from a running Pod, remove auto-generated operational fields (status:, uid:, etc.).

Apply Manifest: kubectl apply -f pod-definition.yaml

🛠️ Troubleshooting and Inspection 🚨
Diagnosing ImagePullBackOff
This common error occurs when the Kubelet fails to pull the container image (e.g., typo in the image name).

Status Check: kubectl get pods shows ErrImagePull or ImagePullBackOff.

Root Cause Analysis: Run kubectl describe pod <pod_name>. The Events Section will provide the explicit failure message (e.g., Failed to pull image "nginx12345").

Resolution: Correct the image name in the YAML file and reapply.

Advanced Inspection Commands
kubectl get pods -o wide: Shows extended details, including the Pod IP address and the Node it is scheduled on.

kubectl get pods --show-labels: Displays all key-value labels attached to the Pods.

kubectl get nodes -o wide: Shows extended information for cluster Nodes.

kubectl explain <object_type>: Provides inline documentation for any Kubernetes resource field.

🏁 Conclusion
You've mastered the foundational skills of Kubernetes YAML and Pod management, covering the full cycle from imperative creation to declarative deployment and troubleshooting. This expertise directly prepares you for working with higher-level controllers like Deployments and Services.

📚 Detailed References
Kubernetes Objects & YAML: Official guide on declarative configuration and the structure of resources.

URL: https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/

kubectl Command Reference: Comprehensive documentation for all kubectl commands.

URL: https://kubernetes.io/docs/reference/kubectl/

Pod Lifecycle and States: Explains why states like ImagePullBackOff occur.

URL: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/

YAML Specification: Clarifies all YAML syntax rules.

URL: https://yaml.org/spec/