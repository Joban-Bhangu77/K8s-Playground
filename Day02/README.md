# 🐳 Day 02 — Deploy Nginx Web Server Using Docker (PowerShell)

📚 Part of the **40 Days Kubernetes & DevOps Series — K8s-Playground** 

## 🧠 Overview

This project demonstrates how to **containerize a static Nginx web application** using **Docker**.  
It represents a foundational concept in modern DevOps workflows — **“Build once, run anywhere.”**

By the end of this lab, you’ll have:
- Built a **custom Docker image** for Nginx serving your HTML file
- Deployed the container locally using **PowerShell**
- Verified web access via `http://localhost:8080`
- Understood how Docker helps standardize **application deployment** across environments  

This is one of the most important exercises before moving toward Kubernetes deployments.

---

## 🧩 Technical Objectives

1. Create and structure a simple web project  
2. Write a **Dockerfile** to containerize the Nginx web server  
3. Build and tag the Docker image  
4. Run and test the containerized Nginx server locally  
5. Verify container networking, ports, and lifecycle commands  

---

## 🧰 Tech Stack

| Tool | Description |
|------|--------------|
| 🐳 **Docker** | Container engine used for image build and deployment |
| 🧾 **PowerShell / WSL2** | CLI environment for managing containers |
| 💻 **Nginx** | Lightweight HTTP web server |
| 🧠 **HTML** | Frontend static page served by Nginx |


## ⚙️ Step-by-Step Implementation

### 🧩 Step 1: Verify Docker Installation
```bash
docker --version
docker info


If Docker is running properly, you’ll see the installed version and system info.

🧩 Step 2: Create Project Folder and Files
mkdir day02_nginx_docker
cd day02_nginx_docker


Project Folder Structure:
## 📁 Project Structure
K8s-Playground/
│
├── day01_nginx_docker/
│   ├── Dockerfile
│   ├── index.html
│   ├── Screenshots/
│   │   ├── Day01_Docker_Version.jpg
│   │   ├── Day01_Project_Folder_Structure.jpg
│   │   ├── Day01_HTML_File_Creation.jpg
│   │   ├── Day01_Dockerfile_Content.jpg
│   │   ├── Day01_Docker_Build_Success.jpg
│   │   ├── Day01_Docker_Run_Success.jpg
│   │   ├── Day01_Nginx_WebOutput.jpg
│   │   ├── Day01_Docker_PS_Images_List.jpg
│   │   ├── Day01_Docker_Logs.jpg
│   │   └── Day01_Docker_Stop_Remove.jpg
│   └── README.md


🧩 Step 3: Create HTML File
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Welcome to Joban’s Nginx Container</title>
    <style>
        body { font-family: Arial; text-align: center; background-color: #f7f9fb; }
        h1 { color: #0078D7; }
        p { color: #333; font-size: 18px; }
    </style>
</head>
<body>
    <h1>🚀 Hello from Nginx Docker Container!</h1>
    <p>This web page is served from a Dockerized Nginx web server.</p>
</body>
</html>


🧩 Step 4: Write the Dockerfile
# Use the official Nginx base image from Docker Hub
FROM nginx:latest

# Copy custom HTML into container’s web root
COPY ./index.html /usr/share/nginx/html/index.html

# Expose the container port
EXPOSE 80

# Default command (already defined in base image)
CMD ["nginx", "-g", "daemon off;"]

Explanation:

FROM defines the base image

COPY places our HTML inside Nginx’s default web directory

EXPOSE tells Docker which port to publish

CMD ensures Nginx stays in the foreground

🧩 Step 5: Build Docker Image
docker build -t my-nginx-site .

Verify: docker images

REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
my-nginx-site    latest    e5b3df12d9aa   10 seconds ago   142MB


🧩 Step 6: Run the Container
docker run -d -p 8080:80 --name nginx-container my-nginx-site
Explanation:

-d → detached mode

-p → maps port 8080 (host) to port 80 (container)

--name → gives the container a readable name


🧩 Step 7: Test in Browser

Visit: 👉 http://localhost:8080

You should see your custom HTML served from inside the container.


🧩 Step 8: Verify docker images
    docker images

🧩 Step 9: Veriy Docker logs for troubleshooting purposes
    docker logs nginx-container

🧩 Step 10: Stop & Remove Container (Cleanup)
docker stop nginx-container
docker rm nginx-container

🔍 Docker Lifecycle & Commands Summary
Command	Description
docker build -t <image_name> .	Build Docker image from Dockerfile
docker images	List local images
docker run -d -p <host>:<container> <image>	Run container in detached mode
docker ps	List running containers
docker stop <container_id>	Stop a running container
docker rm <container_id>	Remove a container
docker rmi <image_id>	Remove image

🧠 Conceptual Understanding
Concept	Explanation
Containerization	Package software and dependencies into isolated units for consistency across systems
Docker Image	Immutable snapshot containing OS, binaries, and app code
Container	Lightweight runtime instance of an image
Port Mapping	Allows external access to internal container services
Stateless Design	Containers are ephemeral and easily replaceable

🧩 Debugging Tips

If port 8080 is already in use, run:

docker run -d -p 9090:80 my-nginx-site

To inspect logs:

docker logs nginx-container

To open a shell inside container:

docker exec -it nginx-container bash

🧠 Key Takeaways

💎 Docker enables lightweight and portable environments
💎 Nginx acts as a fast, secure, and stable web server inside containers
💎 Containerization is the first step before deploying to Kubernetes
💎 You can easily scale containers, automate builds, and integrate into CI/CD pipelines

🏁 Conclusion

In this lab, we successfully built, deployed, and accessed an Nginx web server running in a Docker container.
This workflow forms the base for deploying microservices, reverse proxies, or web applications in containerized environments.