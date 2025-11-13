# 🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how to build and run a production-ready container image using **Docker Multi-Stage Builds**. Multi-stage builds keep images lightweight, secure, and optimized by separating the **build environment** from the **runtime environment**.

---

## 📁 Project Structure

Day03/
├── Dockerfile
├── package.json
├── src/
│ └── index.html
└── Images/
├── Day03_Code1.jpg
├── Day03_Code2.jpg
├── Day03_Container_Running.jpg
├── Day03_Container_Build.jpg
├── Day03_Docker_Logs.jpg
└── Day03_Output.jpg

yaml
Copy code

---

## 🧠 Overview

The purpose of this project was to:

- Build a static website using **Node.js**
- Optimize the container using **Multi-Stage Builds**
- Serve the final build output via **Nginx**
- Keep the production image lightweight and secure
- Test, debug, and document the running container

Multi-stage builds offer:

✔ Smaller image size  
✔ Stronger security  
✔ Clear separation of build vs runtime  
✔ Faster CI/CD pipelines  
✔ Production-grade containerization  

---

## 🛠️ Multi-Stage Dockerfile

![Dockerfile](Images/Day03_Code1.jpg)

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM nginx:latest AS runtime
COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
📦 Building the Docker Image

bash
Copy code
docker build -t day03-app .
▶️ Running the Container

bash
Copy code
docker run -d -p 8080:80 --name day03-container day03-app
Check containers:

bash
Copy code
docker ps
🌐 Accessing the Application
Open in browser:

arduino
Copy code
http://localhost:8080

📜 Viewing Container Logs

bash
Copy code
docker logs day03-container
🔧 Troubleshooting & Useful Docker Commands
✔ Restart container
nginx
Copy code
docker restart day03-container
✔ Stop container
arduino
Copy code
docker stop day03-container
✔ Start container
sql
Copy code
docker start day03-container
✔ Remove container
bash
Copy code
docker rm -f day03-container
✔ Remove image
nginx
Copy code
docker rmi day03-app
✔ Inspect container
nginx
Copy code
docker inspect day03-container
✔ Follow live logs
nginx
Copy code
docker logs -f day03-container
✔ Shell inside container
bash
Copy code
docker exec -it day03-container sh
🚑 Troubleshooting Errors
❗ Error: “failed to read dockerfile: no such file or directory”
Fix:
Make sure you're inside the correct folder:

bash
Copy code
cd Day03
❗ Error: “COPY failed: file not found”
Common causes:

Wrong file name (Package.json instead of package.json)

Wrong file path

Incorrect Docker context

Check:

bash
Copy code
ls -l
❗ Error: “bind: address already in use”
Port 8080 is busy.

Fix:

arduino
Copy code
docker run -d -p 9090:80 day03-app
💡 Key Takeaways
Multi-stage builds drastically reduce image size

Node.js build environment stays entirely out of production

Final image uses clean Nginx runtime

Faster CI/CD pipelines

Higher security by removing build tools

Ideal pattern for DevOps and Kubernetes environments

🧭 Conclusion
This project shows how to build efficient, production-ready Docker images using multi-stage builds. By isolating build dependencies from runtime execution, containers become smaller, faster, and more secure. This workflow aligns perfectly with modern DevOps, CI/CD, and cloud-native best practice