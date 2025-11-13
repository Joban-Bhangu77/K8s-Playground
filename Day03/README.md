🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how to build a production-ready Docker image using Multi-Stage Builds — a technique that separates the build environment (Node.js) from the runtime environment (Nginx).
This keeps your final image lighter, faster, and more secure, following real-world DevOps best practices.

🧠 What This Project Covers

🔹 Building a simple static site using Node.js

🔹 Compiling production-ready files using npm run build

🔹 Serving the optimized output via Nginx

🔹 Implementing Docker Multi-Stage Builds

🔹 Running & inspecting containers

🔹 Viewing logs and debugging container operations

🔹 Documenting the workflow with screenshots

🌈 Why Use Multi-Stage Builds?

⚡ Smaller final image size

🔐 Better security (no build tools in production)

🚀 Faster CI/CD pipelines

🧹 Cleaner separation of build vs runtime

☁️ Cloud & Kubernetes ready images

🧩 Reduced attack surface

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

Open in your browser:

http://localhost:8080


📸 Screenshot: Images/Day03_Output.jpg

📜 View Container Logs
docker logs day03-container


📸 Screenshot: Images/Day03_Docker_Logs.jpg

🔧 Useful Docker Commands

🔄 Restart container

docker restart day03-container


⛔ Stop container

docker stop day03-container


▶️ Start container

docker start day03-container


🗑️ Remove container

docker rm -f day03-container


🧼 Remove image

docker rmi day03-app


🔍 Inspect container

docker inspect day03-container


📡 Follow live logs

docker logs -f day03-container


🐚 Enter shell inside container

docker exec -it day03-container sh

💡 Key Takeaways

🟩 Multi-Stage Builds dramatically reduce image size

🟩 Node.js build tools never reach production

🟩 Nginx serves a clean, optimized static build

🟩 Image builds, pushes, and deployments become much faster

🟩 Greatly improves security & maintainability

🟩 Perfect for Kubernetes, DevOps, and cloud-native workflows

🧭 Conclusion

This project showcases how Docker Multi-Stage Builds transform container development by producing highly efficient, secure, and production-ready images.
By isolating build-time dependencies from runtime execution, we gain improved performance, tighter security, and smoother deployment pipelines.

This approach is a must-have skill for modern DevOps, Cloud, and Kubernetes engineers.