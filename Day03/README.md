🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how how to build a production-ready Docker image using Multi-Stage Builds, a technique that greatly improves container efficiency by separating the build environment (Node.js) from the runtime environment (Nginx).

By doing so, the final image becomes:

Lightweight

More secure

Faster to deploy

Easier to maintain

Perfect for cloud & Kubernetes environments

🧠 Overview

This project focuses on implementing Multi-Stage Docker Builds to:

Build a simple static site using Node.js

Generate an optimized build output

Serve the application using Nginx

Reduce image size by removing build-time dependencies

Run and debug the application using Docker commands

Capture and showcase results with screenshots

Why Multi-Stage Builds?

✔ Smaller final images
✔ No build tools in production
✔ Faster CI/CD pipelines
✔ Clear separation of build and runtime
✔ Best practice for cloud-native applications

🛠️ Multi-Stage Dockerfile

📸 Screenshot: Images/Day03_Code1.jpg

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


This Dockerfile ensures that:

Node.js stays in the builder stage only

The final Nginx image contains only the static build output

No unnecessary dependencies reach the runtime image

📦 Build the Docker Image
docker build -t day03-app .


📸 Screenshot: Images/Day03_Container_Build.jpg

▶️ Run the Container
docker run -d -p 8080:80 --name day03-container day03-app


Verify the running container:

docker ps


📸 Screenshot: Images/Day03_Container_Running.jpg

🌐 Access the Application

Open your browser and visit:

http://localhost:8080


📸 Screenshot: Images/Day03_Output.jpg

📜 View Container Logs
docker logs day03-container


📸 Screenshot: Images/Day03_Docker_Logs.jpg

🔧 Useful Docker Commands

Restart the container:

docker restart day03-container


Stop the container:

docker stop day03-container


Start the container:

docker start day03-container


Remove the container:

docker rm -f day03-container


Remove the image:

docker rmi day03-app


Inspect container details:

docker inspect day03-container


Follow logs live:

docker logs -f day03-container


Open shell inside the container:

docker exec -it day03-container sh

💡 Key Takeaways

Multi-Stage Builds dramatically reduce final image size

Node.js build stage stays completely out of production

Nginx provides a clean, efficient static file server

Faster builds, pushes, pulls, and deployments

Stronger runtime security

A best-practice approach for DevOps and Kubernetes workflows

🧭 Conclusion

This project highlights how Docker Multi-Stage Builds enable efficient, secure, and production-ready container images.
By splitting the build and runtime environments, we eliminate unnecessary dependencies, reduce attack surface, and create highly optimized images suitable for scalable, modern cloud deployments.

This technique is essential for anyone working with containers, DevOps pipelines, or Kubernetes-based architectures