# 🐳 Day 02: Deploy Nginx Web Server Using Docker (PowerShell)

> 🚀 This project is part of my **“40 Days of Kubernetes” Learning Series.** In this lab, you’ll learn how to deploy a simple yet powerful **Nginx web server** inside a Docker container using **PowerShell on Windows** an essential step toward mastering containerization.

---

## 🎯 Objective / Overview

Containers are the backbone of modern application deployment.  
Before diving deep into Kubernetes, it's crucial to understand **how applications are packaged and run using Docker**.

This lab focuses on deploying **Nginx**, a lightweight and high-performance web server, inside a **Docker container**.  
You’ll:
- Learn the complete workflow of creating a Docker project from scratch.
- Understand how to write a basic **Dockerfile**.
- Build and run custom Docker images.
- Access your Nginx-hosted web page via your local browser.
- Gain foundational experience for scaling, composing, and orchestrating containers in Kubernetes.

By the end of this lab, you’ll have hands-on experience with Docker fundamentals that directly translate to **real-world DevOps and Kubernetes scenarios**.

---

## 🧰 Step 1: Verify Docker Installation

Check if Docker Desktop is installed and running.

```powershell
docker --version

![Docker Version Check](https://github.com/user-attachments/assets/bb8b1e49-369f-4bf9-bbe5-7cc449570c83)

✅ Expected Output:

Docker version 27.1.1, build bcfed03

📁 Step 2: Create a Project Folder

Create a clean directory structure for your project.

cd ~\Desktop
mkdir K8s-Playground
cd K8s-Playground
mkdir day02_nginx_docker
cd day02_nginx_docker


This ensures a well-organized environment for Docker files, HTML files, and any related configurations.

🧾 Step 3: Create an HTML File

Create and edit a simple webpage that Nginx will serve.

New-Item index.html
notepad index.html


Paste the following code:

<!DOCTYPE html>
<html>
  <head>
    <title>Welcome to Docker Nginx!</title>
    <style>
      body { 
        font-family: Arial, sans-serif; 
        text-align: center; 
        margin-top: 100px; 
        background-color: #fafafa; 
      }
      h1 { color: #0db7ed; }
    </style>
  </head>
  <body>
    <h1>🚀 Hello from Nginx running inside Docker!</h1>
    <p>This page is served by an Nginx container.</p>
  </body>
</html>

🧱 Step 4: Create a Dockerfile

The Dockerfile defines how the container image will be built.
It includes the base image, files to copy, ports to expose, and commands to run.

New-Item Dockerfile
notepad Dockerfile


Paste this content:

# Use the official Nginx image from Docker Hub
FROM nginx:latest

# Copy our custom HTML into Nginx's web directory
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80 for web access
EXPOSE 80

⚙️ Step 5: Build the Docker Image

Build the image using the Dockerfile.

docker build -t my-nginx-site .


✅ Expected Output:

[+] Building 3.4s (6/6) FINISHED
 => [internal] load build definition from Dockerfile
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to my-nginx-site


You’ve now created your own custom Docker image based on Nginx that serves your HTML page.

🚀 Step 6: Run the Nginx Container

Start the container using your new image and map it to a host port for access.

docker run -d -p 8080:80 --name nginx-container my-nginx-site


✅ Expected Output:

fce45cbe89011f9e4e439a8b9b9984729f3f3d95f36e51fba3fa6b8924cda5f9


Here, port 8080 on your local machine is mapped to port 80 in the container, allowing you to access the web page at http://localhost:8080.

🌐 Step 7: Access the Website

Open your browser and navigate to:

👉 http://localhost:8080

You should see your HTML message served by Nginx running inside Docker:

“🚀 Hello from Nginx running inside Docker!”

🧩 Step 8: Verify Containers and Images

List all running containers:

docker ps


List all available images:

docker images


Use these commands to confirm that your Nginx container is up and running and that the custom image exists locally.

📜 Step 9: Check Nginx Logs

View real-time Nginx access logs inside the container:

docker logs nginx-container


This helps verify that your requests are hitting the Nginx web server successfully.

🛑 Step 10: Stop and Remove the Container

When you’re done testing, stop and clean up your environment.

docker stop nginx-container
docker rm nginx-container

📂 Final Project Structure
K8s-Playground/
│
└── day02/
    ├── Dockerfile          # Blueprint for the Nginx image
    ├── index.html          # Custom web content served by Nginx
    ├── Day01_Docker_Lab/   # Directory for all lab screenshots/assets
    │   ├── Day02_Docker_Version.jpg
    │   ├── Day02_Project_Folder_Structure.jpg
    │   ├── Day02_HTML_File_Creation.jpg
    │   ├── Day01_Dockerfile_Content.jpg
    │   ├── Day02_Docker_Build_Success.jpg
    │   ├── Day02_Docker_Run_Success.jpg
    │   ├── Day02_Nginx_WebOutput.jpg
    │   ├── Day02_Docker_PS_Images_List.jpg
    │   ├── Day02_Docker_Logs.jpg
    │   └── Day02_Docker_Stop_Remove.jpg
    └── README.md

💎 Key Takeaways

💠 Gained hands-on experience deploying a web server inside Docker.
💠 Understood essential Dockerfile instructions — FROM, COPY, and EXPOSE.
💠 Learned how to map host ports to container ports.
💠 Practiced container lifecycle management (create → run → inspect → stop → remove).
💠 Set the foundation for future labs involving Docker Compose and Kubernetes Deployments.

🧠 Conclusion

This exercise demonstrated how quickly you can containerize a simple application using Docker.
By isolating Nginx inside a container, you created a reproducible, portable web server environment that can run anywhere — from your laptop to the cloud.

You’ve successfully completed your first step in the DevOps containerization journey.
Next, we’ll move toward managing multi-container setups and preparing them for orchestration in Kubernetes.

🔧 Key Highlights

✅ Built a Docker image using a Dockerfile based on nginx:latest
✅ Copied a custom index.html into the container
✅ Exposed port 80 and mapped it to 8080 on the host
✅ Accessed the running web server via http://localhost:8080
✅ Verified container status, logs, and cleanup using basic Docker commands



