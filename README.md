# K8s-Playground 🌟

Welcome to **K8s-Playground**, a comprehensive hands-on **Kubernetes lab repository**.  
This repository is based on **Piyush SachDeva’s 40 Days Kubernetes Series** and is designed for learners who want to **master Kubernetes from scratch** through practical, step-by-step labs.

---

## 📌 Overview

Kubernetes is the leading container orchestration platform in the cloud-native ecosystem.  
The **K8s-Playground** repository is designed to help learners:

- Understand Kubernetes concepts deeply
- Practice deploying and managing resources
- Explore real-world scenarios in a safe, local environment

This repository contains **40 structured labs**, one for each day of the series, covering topics from Docker basics to advanced Kubernetes networking, storage, and troubleshooting.

---

## 🎯 Scope

This repository is ideal for:

- Beginners looking to start their Kubernetes journey  
- DevOps engineers who want hands-on practice  
- Cloud engineers preparing for CKA / CKAD / real-world deployments  

Each day includes:

1. **Objective / Learning Outcome**  
2. **Step-by-Step Instructions**  
3. **YAML Manifests** (`yaml/` folder)  
4. **Screenshots / Outputs** (`screenshots/` folder)  
5. **Optional diagrams** (`diagrams/` folder)

Topics covered include:

- Docker Fundamentals & Containerization  
- Kubernetes Architecture & Cluster Setup  
- Pods, Deployments, ReplicaSets  
- Services, Ingress, Networking & DNS  
- ConfigMaps, Secrets, Volumes, Persistent Storage  
- RBAC, Authentication & Authorization  
- Monitoring, Logging, Autoscaling & Troubleshooting  

---

## ⚙️ Prerequisites

- **Local Kubernetes cluster**: Minikube, Kind, or kubeadm  
- **kubectl** installed and configured  
- Basic **Docker** knowledge  
- Optional: Helm, Kustomize, and YAML familiarity  

---

## 🗂 Repository Structure

K8s-Playground/
│
├── Day01_Docker_Fundamentals/
├── Day02_Dockerize_Application/
├── Day03_Docker_MultiStage_Builds/
├── Day04_Why_Kubernetes/
├── Day05_Kubernetes_Architecture/
├── Day06_Install_Kubernetes_Cluster/
├── Day07_Pods/
├── Day08_ReplicaSets_Deployments/
├── Day09_Services/
├── Day10_Namespaces/
├── Day11_MultiContainer_Pods/
├── Day12_DaemonSets_Jobs_CronJobs/
├── Day13_Static_Pods_Node_Scheduling/
├── Day14_Taints_Tolerations/
├── Day15_Node_Affinity_AntiAffinity/
├── Day16_Resource_Requests_Limits/
├── Day17_Autoscaling_HPA_VPA/
├── Day18_Probes_Liveness_Readiness/
├── Day19_ConfigMaps_Secrets/
├── Day20_SSL_TLS_Fundamentals/
├── Day21_TLS_Kubernetes_Certificates/
├── Day22_Authentication_Authorization/
├── Day23_RBAC/
├── Day24_ClusterRoles_ClusterRoleBindings/
├── Day25_Service_Accounts/
├── Day26_Network_Policies_CNI/
├── Day27_Kubeadm_MultiNode_Cluster/
├── Day28_Docker_Storage_Fundamentals/
├── Day29_Kubernetes_Storage_PV_PVC_StorageClass/
├── Day30_DNS_Fundamentals/
├── Day31_DNS_Kubernetes_CoreDNS_ServiceDiscovery/
├── Day32_Kubernetes_Networking_Overview/
├── Day33_Ingress_Controllers_Resources/
├── Day34_Cluster_Upgrade_kubeadm/
├── Day35_etcd_Backup_Restore/
├── Day36_Monitoring_Logging_Alerting/
├── Day37_Troubleshoot_Application_Failures/
├── Day38_Troubleshoot_Cluster_Component_Failures/
├── Day39_Network_Troubleshooting_WorkerNode_Drain_Cordon/
├── Day40_JSONPath_Advanced_kubectl/
│
├── scripts/                  # Optional automation scripts
├── diagrams/                 # Architecture/networking diagrams
├── README.md                 # Main landing README
├── SUMMARY.md                # Table of contents linking all 40 days
└── .gitignore


Each **day folder** contains:  

- `yaml/` → Kubernetes manifests for that day  
- `screenshots/` → Outputs, visual references  
- `README.md` → Step-by-step instructions  

---

## 🚀 How to Use This Repository

1. **Clone the repository**
```bash
git clone https://github.com/<YourUsername>/K8s-Playground.git
cd K8s-Playground
Navigate to any day

bash
Copy code
cd day01_Docker_Fundamentals
cat README.md       # Follow instructions
kubectl apply -f yaml/<manifest>.yaml
Check results

bash
Copy code
kubectl get pods
kubectl describe pod <pod-name>
Optional: Refer to screenshots/ for expected outputs.

Cleanup resources after each lab

bash
Copy code
kubectl delete -f yaml/<manifest>.yaml
💡 Takeaways
By completing these 40 labs, you will be able to:

Understand the Kubernetes architecture and components

Deploy, scale, and manage containerized applications

Configure network policies, ingress, and service discovery

Manage storage with Persistent Volumes & Persistent Volume Claims

Implement RBAC and secure your cluster

Monitor and troubleshoot applications and clusters

Prepare for Kubernetes certification (CKA/CKAD) with hands-on practice

📚 Additional Resources
Official Kubernetes Documentation

Piyush SachDeva’s YouTube Channel

CKA / CKAD Certification Guides

🔖 Notes
All labs are designed for learning and practice purposes

Modify YAML manifests to experiment with different scenarios

Use scripts/ folder for quick deployment/cleanup if provided

Screenshots and diagrams help verify your outputs visually

🏁 Conclusion
K8s-Playground provides a complete, hands-on Kubernetes learning journey from beginner to advanced.
Follow the labs day by day, practice, experiment, and you’ll gain confidence to manage real-world Kubernetes clusters.

“Kubernetes mastery comes from doing, not just watching. Explore, create, and learn!”

🌟 Connect

GitHub: https://github.com/Joban-Bhangu77/Joban-Bhangu77.git

Inspired by: Piyush SachDeva Kubernetes 40 Days Series