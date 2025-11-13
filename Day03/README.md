🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how to build a production-ready Docker container using Multi-Stage Builds — a technique that reduces image size, improves security, and keeps the runtime environment clean and optimized.

The application is first built using Node.js (builder stage), and the final optimized output is served using Nginx (runtime stage). This separation reflects real-world DevOps and cloud-native best practices.

🧠 What This Project Covers

Creating a static web application with Node.js

Using Multi-Stage Docker Builds for optimized images

Serving the final build using Nginx

Running, testing, and logging Docker containers

Using core Docker commands for debugging and operations

Capturing key screenshots for GitHub documentation

Multi-Stage Builds provide:

✔ Smaller image size
✔ Stronger security
✔ Faster deployments
✔ Cleaner runtime environment
✔ Cloud & Kubernetes readiness

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

Node.js and build tools never reach the production image

Only the essential static files are included

Nginx serves a clean, optimized build

📦 Build the Docker Image
docker build -t day03-app .


📸 Screenshot: Images/Day03_Container_Build.jpg

▶️ Run the Container
docker run -d -p 8080:80 --name day03-container day03-app


Check running containers:

docker ps


📸 Screenshot: Images/Day03_Container_Running.jpg

🌐 Access the Application

Open in your browser:

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


Follow logs live:

docker logs -f day03-container


Enter container shell:

docker exec -it day03-container sh

💡 Key Takeaways

Multi-stage builds dramatically reduce the final image size

The Node.js environment stays completely out of production

Nginx serves a clean, optimized, static build

Faster image builds, pushing, pulling, and deployments

Better security by eliminating unnecessary tooling

Perfect workflow for Kubernetes and DevOps pipelines

🧭 Conclusion

This project shows how Docker Multi-Stage Builds improve performance, security, and maintainability.
By isolating build-time dependencies from the runtime environment, we create smaller, faster, and more secure containers that are ready for real-world cloud deployments.

This approach is considered the industry standard for modern containerization and aligns perfectly with DevOps and Kubernetes best practices.