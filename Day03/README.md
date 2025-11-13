🐳 Day 03 – Docker Multi-Stage Builds (Node + Nginx)

This project demonstrates how to build and run a production-ready container image using Docker Multi-Stage Builds. Multi-stage builds help keep container images lightweight, secure, and optimized by separating the build environment from the runtime environment.

This approach is widely used in real-world DevOps and cloud deployments.

📁 Project Structure

K8s-Playground/Day03/
├── Dockerfile
├── package.json
├── src/
│   └── index.html
└── Images/
    ├── Day03_Code1.jpg
    ├── Day03_Code2.jpg
    ├── Day03_Container_Running.jpg
    ├── Day03_Container_Build.jpg
    ├── Day03_Docker_Logs.jpg
    └── Day03_Output.jpg

🧠 Overview

The goal of this lab was to:

Build a simple static website using Node.js

Use multi-stage builds to optimize the final Docker image

Serve the compiled frontend using Nginx

Reduce image size by excluding development tools from production

Run and test the final container locally

Capture logs, inspect container status, and troubleshoot common issues

Multi-stage builds ensure:

✔ Smaller image size
✔ Better security
✔ Cleaner separation of concerns
✔ Faster CI/CD pipelines
✔ Cloud-ready containerization

🛠️ Multi-Stage Dockerfile

This Dockerfile uses:

Stage 1 (Builder) → Node.js Alpine to install dependencies & run build

Stage 2 (Runtime) → Nginx to serve the static site

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
```

📸 SEE IMAGE: Day03_Code1.jpg

📦 Build the Docker Image

Run from inside the Day03 folder:

docker build -t day03-app .


📸 SEE IMAGE: Day03_Container_Build.jpg

▶️ Run the Container
docker run -d -p 8080:80 --name day03-container day03-app


📸 SEE IMAGE: Day03_Container_Running.jpg

Check the running container:

docker ps

🌐 Access the Application

Open in your browser:

👉 http://localhost:8080

📸 SEE IMAGE: Day03_Output.jpg

📜 View Container Logs
docker logs day03-container


📸 SEE IMAGE: Day03_Docker_Logs.jpg

🔧 Useful Docker Commands (Troubleshooting & Operations)
✔ Restart container
docker restart day03-container

✔ Stop container
docker stop day03-container

✔ Start container again
docker start day03-container

✔ Remove container
docker rm -f day03-container

✔ Delete image
docker rmi day03-app

✔ Inspect container details
docker inspect day03-container

✔ Check container network settings
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' day03-container

✔ Live logs
docker logs -f day03-container

✔ Shell access inside a container (if needed)
docker exec -it day03-container sh

🚑 Troubleshooting
❗Error: “failed to read dockerfile: no such file or directory”

Fix:
Make sure your terminal is inside the correct folder:

cd Day03

❗Error: “COPY failed: file not found”

Common causes:

Wrong file name (Package.json instead of package.json)

File in the wrong directory

Incorrect path in Dockerfile

Fix:
Check spelling and file structure:

ls -l

❗Error: “bind: address already in use”

Means port 8080 is already being used.

Fix:
Stop the container using that port or run on a different port:

docker run -d -p 9090:80 day03-app

🔍 Key Takeaways

Multi-stage builds drastically reduce final image size

Only essential files make it to production

Application runs in a secure, clean Nginx environment

Builds are faster and easier to debug

Proper folder structure simplifies workflows

Essential Docker commands help manage, inspect, and troubleshoot containers

This method is industry standard for modern DevOps and Kubernetes environments

🧭 Conclusion

This project demonstrates how to create efficient, production-ready Docker images using multi-stage builds. By separating build-time dependencies from runtime execution, containers become smaller, faster, and significantly more secure.

This is a core skill for anyone working with containerized applications, cloud deployments, and Kubernetes.