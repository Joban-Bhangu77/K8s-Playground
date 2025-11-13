🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how to build a production-ready Docker container using Multi-Stage Builds.
By separating the build environment from the runtime environment, we create a final image that is lightweight, secure, and optimized for cloud and DevOps workloads.

The application is built using Node.js and served using Nginx, following industry-grade containerization best practices.

🧠 What This Project Covers

Building a simple static site using Node.js

Implementing Docker Multi-Stage Builds

Serving optimized static files using Nginx

Running and inspecting Docker containers

Viewing logs and performing container operations

Adding screenshots for documentation

Multi-Stage Builds provide:

✔ Smaller final image size
✔ Better security (no build tools in production)
✔ Faster CI/CD pipelines
✔ Clear separation of build vs runtime
✔ Cloud & Kubernetes-ready images

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

📦 Build the Docker Image
docker build -t day03-app .


📸 Screenshot: Images/Day03_Container_Build.jpg

▶️ Run the Container
docker run -d -p 8080:80 --name day03-container day03-app


Check running containers:

docker ps


📸 Screenshot: Images/Day03_Container_Running.jpg

🌐 Access the Application

Open in browser:

http://localhost:8080


📸 Screenshot: Images/Day03_Output.jpg

📜 View Container Logs
docker logs day03-container


📸 Screenshot: Images/Day03_Docker_Logs.jpg

🔧 Useful Docker Commands

Restart container:

docker restart day03-container


Stop container:

docker stop day03-container


Start container:

docker start day03-container


Remove container:

docker rm -f day03-container


Remove image:

docker rmi day03-app


Inspect container:

docker inspect day03-container


Follow live logs:

docker logs -f day03-container


Enter container shell:

docker exec -it day03-container sh

💡 Key Takeaways

Multi-stage builds drastically reduce image size

Node.js build environment stays out of production

Clean Nginx runtime ensures optimal performance

Faster builds, pushes, and deployments

Greatly enhances security and maintainability

Ideal method for DevOps, cloud, and Kubernetes workflows

🧭 Conclusion

This project highlights how Docker Multi-Stage Builds create secure, lightweight, and production-ready container images.
By isolating build dependencies from runtime execution, we achieve better performance, tighter security, and greater deployment efficiency.

This workflow aligns with modern DevOps and cloud-native best practices and is essential for scalable application deploymen