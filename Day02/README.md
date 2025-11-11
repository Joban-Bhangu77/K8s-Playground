# 🐳 Day 02: Deploy Nginx Web Server Using Docker (PowerShell), This Project is Part of My “40 Days of Kubernetes” learning Series.


---

## 🎯 Objective
In this lab, I  deployed a fully functional **Nginx web server** inside a Docker container using **PowerShell on Windows**.  
You’ll create a simple HTML page, build a Docker image, run the container, and access the page from your browser.

---

## 🧰 Step 1: Verify Docker Installation

Check if Docker Desktop is installed and running.

```powershell
docker --version

✅ Expected Output:

Docker version 27.1.1, build bcfed03

📁 Step 2: Create a Project Folder

Create a clean directory structure for your project.
cd ~\Desktop
mkdir K8s-Playground
cd K8s-Playground
mkdir day01_nginx_docker
cd day01_nginx_docker

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

Create a Dockerfile that defines your Nginx container.

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

 🚀 Step 6: Run the Nginx Container

Run the newly built image as a container.

docker run -d -p 8080:80 --name nginx-container my-nginx-site


✅ Expected Output:

fce45cbe89011f9e4e439a8b9b9984729f3f3d95f36e51fba3fa6b8924cda5f9

🌐 Step 7: Access the Website

Open your browser and navigate to:

👉 http://localhost:8080

✅ You should see:

🚀 Hello from Nginx running inside Docker!

🧩 Step 8: Verify Containers and Images

List all running containers:

docker ps


List all Docker images:

docker images

📜 Step 9: Check Nginx Logs

View Nginx access logs:

docker logs nginx-container

🛑 Step 10: Stop and Remove the Container

Stop and clean up your environment.

docker stop nginx-container
docker rm nginx-container

K8s-Playground/
│
├── day02/
│   ├── Dockerfile
│   ├── index.html
│   ├── Day01_Docker_Lab/
│   │   ├── Day02_Docker_Version.jpg
│   │   ├── Day02_Project_Folder_Structure.jpg
│   │   ├── Day02_HTML_File_Creation.jpg
│   │   ├── Day01_Dockerfile_Content.jpg
│   │   ├── Day02_Docker_Build_Success.jpg
│   │   ├── Day02_Docker_Run_Success.jpg
│   │   ├── Day02_Nginx_WebOutput.jpg
│   │   ├── Day02_Docker_PS_Images_List.jpg
│   │   ├── Day02_Docker_Logs.jpg
│   │   └── Day02_Docker_Stop_Remove.jpg
│   └── README.md

💎 Key Takeaways

💠 Learned how to build and run a Dockerized Nginx web server.
💠 Understood essential Dockerfile instructions — FROM, COPY, and EXPOSE.
💠 Built and deployed a custom Nginx image using PowerShell commands.
💠 Mapped host ports (8080:80) to access the containerized web app.
💠 Verified images, containers, and logs for complete visibility.

🧠 Conclusion

I ve successfully hosted a custom web page using Nginx inside Docker.
This marks your first step toward mastering containerized web applications.
Next, you can extend this by:

Hosting multiple pages using Docker volumes

Running multiple containers with Docker Compose

Deploying your container to a Kubernetes cluster

🔧 Key Highlights:

Built a Docker image using a Dockerfile based on Nginx:latest

Copied a custom index.html into the container

Exposed port 80 and mapped it to 8080 on the host

Accessed the running web server via http://localhost:8080

Verified container status, logs, and cleanup using basic Docker commands

This exercise lays the foundation for future days where I will extend the setup using Docker Compose and later deploy it in Kubernetes.