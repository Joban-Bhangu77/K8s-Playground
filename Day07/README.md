# 🟦 Day 07 — Kubernetes YAML & Pod Management
### 40 Days Kubernetes Series — K8s-Playground

This session focused on mastering YAML in Kubernetes, converting imperative commands into declarative configurations, and practically working with Pods using both approaches. You also troubleshooted real-world container image issues using Kubernetes-native debugging commands.

---

# 📘 What is YAML?

YAML (YAML Ain’t Markup Language) is a lightweight, human-readable configuration language widely used across DevOps, cloud-native workflows, automation, and especially Kubernetes.

It provides a clean, structured way to express configuration in a declarative format, allowing Kubernetes to maintain the system’s desired state automatically.

### Key Characteristics of YAML
- Simple and human-friendly syntax  
- Uses indentation-based structure (2 spaces, no tabs)  
- Supports lists and nested hierarchies  
- Highly readable compared to JSON and XML  
- Universally used for Kubernetes resource definitions  

---

# 🧩 Why YAML is Essential in Kubernetes

Kubernetes operates on the principle of a desired state, meaning you describe what you want, and Kubernetes ensures it becomes reality. YAML is the language used to express that desired state.

### YAML enables:
- Version-controlled infrastructure (GitOps)  
- Consistent deployments across environments  
- Repeatable and automated workflows  
- Declarative configuration for Pods, Deployments, Services, etc.  
- Easy collaboration and sharing of manifests  

---

# 🧱 Basic YAML Structure for Kubernetes

Below is a typical Pod definition in YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: demo
spec:
  containers:
  - name: nginx-container
    image: nginx:latest

🔹Important YAML Rules in Kubernetes

🔹Indentation = 2 spaces

🔹No tabs allowed

🔹Lists begin with "-"

🔹Keys must be aligned correctly

🔹Order of main sections matters (apiVersion, kind, metadata, spec)

🏗️ TASK 1— Create a Pod Using Imperative Commands

First, we used the kubectl run command to create a Pod quickly using an image.

Command
kubectl run nginx --image=nginx

Verify
kubectl get pods
kubectl describe pod nginx

🏗️ TASK 2— Generate YAML From an Imperative Pod & Recreate

Step 2— Edit and Clean the YAML

Remove automatically generated fields:

🔹status

🔹uid

🔹resourceVersion

🔹creationTimestamp

🔹managedFields

🔹selfLink


Update the Pod name:
metadata:
  name: nginx-new

Step 3— Apply the New YAML
kubectl apply -f nginx.yaml

Step 4 — Verify Creation
kubectl get pods

Expected output includes:
nginx
nginx-new

🧪 TASK 3 — Apply Faulty YAML, Identify Issues, and Fix Them
Provided YAML (with errors)
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: test
  name: redis
spec:
  containers:
  - image: rediss
    name: redis

The issue: image name "rediss" is incorrect → the Pod will fail with ImagePullBackOff.

Step-by-Step Troubleshooting
Apply the YAML
kubectl apply -f redis.yaml

Check Pod status
kubectl get pods


You will see:

ErrImagePull

Describe the Pod for exact error
kubectl describe pod redis


Expected message:

Failed to pull image "rediss": image not found
Back-off pulling image "rediss"


This confirms the incorrect image name.

Fix the YAML File

Correct the image name:

image: redis

Reapply:
kubectl apply -f redis.yaml

Final Verification:
kubectl get pods


Expected:

redis   Running

🟦 Key Takeaways

YAML is the backbone of Kubernetes configuration

Declarative manifests make deployments scalable and automated

Converting imperative resources into YAML is a key DevOps skill

kubectl describe is essential for troubleshooting

ImagePullBackOff errors often relate to incorrect image names

Clean YAML files before recreating Pods from exported manifests

🏁 Conclusion

Today’s session provided deep hands-on experience with Kubernetes YAML, a foundational skill for anyone working with Kubernetes, DevOps, Cloud Engineering, or GitOps. You learned how to:

Create Pods imperatively

Convert running Pods into reusable YAML manifests

Rebuild Pods declaratively

Debug and fix real-world Kubernetes errors

These concepts prepare you for upcoming topics like Deployments, ReplicaSets, Services, Namespaces, and ConfigMaps.

📚 References
1. Official Kubernetes Documentation — YAML & Object Management

https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/

2. Kubernetes — Managing Resources with kubectl

https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/

3. YAML Official Specification

https://yaml.org/spec/

4. Kubernetes Pods Documentation

https://kubernetes.io/docs/concepts/workloads/pods/

5. Kubernetes Troubleshooting Guide (ImagePullBackOff)

https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#imagepullbackoff

6. kubectl Command Reference

https://kubernetes.io/docs/reference/kubectl/