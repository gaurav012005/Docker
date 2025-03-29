# 🚀 Introduction to Docker
Docker is a platform for developing, shipping, and running applications in containers. It allows you to package applications and their dependencies into a single container image.

---
## 🏗 Docker Architecture
Docker follows a client-server architecture:
- **Docker Client** 🖥️
- **Docker Daemon** 🐳
- **Docker Registry** 📦
- **Docker Objects** (Images, Containers, Networks, Volumes)

🔹 Commands:
```
docker version
docker info
docker system df
```

---
## 📥 Installing Docker
Follow these steps to install Docker on your system:

🔹 Commands:
```
# Linux
curl -fsSL https://get.docker.com | sh
sudo systemctl enable docker
sudo systemctl start docker

# Windows/Mac
Download and install Docker Desktop from https://www.docker.com/products/docker-desktop
```

---
## 📦 Docker Images
Docker images are templates used to create containers.

🔹 Commands:
```
docker images
docker pull <image-name>
docker rmi <image-id>
```

---
## 📝 Dockerfile 
A `Dockerfile` is a script containing instructions to build a Docker image.

🔹 Example `Dockerfile`:
```Dockerfile
# 1️⃣ Base Image  
FROM ubuntu:latest  

# 2️⃣ Set Working Directory  
WORKDIR /app  

# 3️⃣ Copy Files  
COPY . /app  

# 4️⃣ Install Dependencies  
RUN apt-get update && apt-get install -y curl  

# 5️⃣ Set Environment Variables  
ENV APP_ENV=production  

# 6️⃣ Expose Ports  
EXPOSE 8080  

# 7️⃣ Define Command to Run the App  
CMD ["bash"]

```

🔹 Commands:
```
docker build -t my-java-app .
docker run my-java-app
```

---
## 🌐 Docker Networking
Docker provides several networking options:
- **Bridge** (default)
- **Host**
- **None**
- **Overlay**
- **Macvlan**

🔹 Commands:
```
docker network ls
docker network create --driver bridge my_network
docker network inspect my_network
docker network rm my_network
```

---
## 🗄 Docker Volumes and Storage
Docker provides volumes to persist data.

🔹 Commands:
```
docker volume create my_volume
docker volume ls
docker run -v my_volume:/data busybox
```

---
## ⚙ Docker Compose
Docker Compose is a tool for defining and running multi-container applications.

🔹 Example `docker-compose.yml`:
```yaml
version: ""  # Define the Docker Compose version

services:
  app:  # Define a service
    image: ""  # Specify the Docker image
    build: ""  # Path to the Dockerfile
    container_name: ""  # Name the container
    environment:  # Define environment variables
      - KEY=VALUE
    ports:  # Map ports (host:container)
      - ""
    volumes:  # Mount volumes
      - ""
    networks:  # Connect to a network
      - ""

  db:  # Another service (e.g., database)
    image: ""
    container_name: ""
    environment:
      - ""
    volumes:
      - ""
    ports:
      - ""
    networks:
      - ""

volumes:  # Define named volumes
  volume_name: {}

networks:  # Define networks
  network_name: {}

```

🔹 Commands:
```
docker-compose up -d
docker-compose ps
docker-compose down
```

---
## 📤 Docker Registry
Docker registries store and distribute images.

🔹 Commands:
```
docker login
docker tag my-image myrepo/my-image:v1
docker push myrepo/my-image:v1
docker pull myrepo/my-image:v1
```

---
## 🏗 Multi-stage Docker Builds
Multi-stage builds help reduce image size.

🔹 Example `Dockerfile`:
```Dockerfile
# 🚀 Stage 1: Build Stage  
FROM maven:3.8.4-jdk-11 AS builder  
WORKDIR /app  
COPY . .  
RUN mvn clean package  

# 🎯 Stage 2: Runtime Stage  
FROM openjdk:11-jdk-slim  
WORKDIR /app  
COPY --from=builder /app/target/app.jar app.jar  
CMD ["java", "-jar", "app.jar"]

```
📌 Best Practices for Multi-Stage Dockerfile


🔹 1. Use a Lightweight Base Image
Use distroless or Alpine-based images to reduce the attack surface and improve performance.

🔹 2. Minimize Layers
Combine multiple RUN commands into a single layer to reduce image size.

🔹 3. Use .dockerignore
Exclude unnecessary files (logs, node_modules, .git, etc.) to keep images clean.

🔹 4. Use Specific Versions
Pin dependencies (e.g., FROM golang:1.21) to ensure build consistency.

🔹 5. Remove Unnecessary Files in Final Stage
Only copy essential files from the build stage to the final image.



🔹 Commands:
```
docker build -t my-multi-stage-app .
docker run my-multi-stage-app
```

---
# 📌 **DockerHub: Pull, Push & Essential Commands**  

Docker Hub is a **cloud-based container registry** that allows you to store, manage, and distribute Docker images. Below are the key **use cases** and **commands** for working with DockerHub.  

---

## 🏗 **1. Why Use DockerHub?**  
✅ **Store and Share Images** → Host your custom images online  
✅ **Pull Pre-built Images** → Download official images for quick use  
✅ **Automated Builds** → Integrate with CI/CD for DevOps workflows  
✅ **Deploy Anywhere** → Pull your images from any server  

---

## 🔹 **2. DockerHub Login**  
Before pushing images, authenticate to DockerHub:  
```sh
docker login
```
🔹 **Using a token (More Secure 🔒)**  
```sh
docker login --username <your-username>
```
---

## 🔹 **3. Pull an Image from DockerHub**  
To download a public image from DockerHub:  
```sh
docker pull <image-name>:<tag>
```
💡 **Example:** Pull the latest Ubuntu image  
```sh
docker pull ubuntu:latest
```
💡 **Example:** Pull a specific version of Nginx  
```sh
docker pull nginx:1.21
```
💡 **Example:** Pull a private image (after login)  
```sh
docker pull myusername/private-repo:latest
```
---

## 🔹 **4. List Downloaded Images**  
```sh
docker images
```
---

## 🔹 **5. Tag an Image for DockerHub**  
Before pushing an image, tag it with your **DockerHub username**:  
```sh
docker tag <local-image> <dockerhub-username>/<repo-name>:<tag>
```
💡 **Example:** Tag a local image for pushing  
```sh
docker tag my-app myusername/my-app:latest
```
---

## 🔹 **6. Push an Image to DockerHub**  
After tagging the image, push it to DockerHub:  
```sh
docker push <dockerhub-username>/<repo-name>:<tag>
```
💡 **Example:**  
```sh
docker push myusername/my-app:latest
```
🔹 **Push a private image** (only accessible with login)  
```sh
docker push myusername/private-app:1.0
```
---

## 🔹 **7. Check Your Images on DockerHub**  
Visit **[https://hub.docker.com/](https://hub.docker.com/)** and log in to see your pushed images.

---

## 🔹 **8. Remove Local Images (Cleanup)**  
To free up space, remove an image:  
```sh
docker rmi <image-id>
```
💡 **Remove all unused images**  
```sh
docker image prune -a
```
---

## 🔹 **9. Pull and Run a Container in One Step**  
```sh
docker run -d -p 8080:80 nginx
```
This **pulls the image** (if not available locally) and **runs a container**.

---

## 🔹 **10. Pull, Modify & Push a Custom Image**  

### 🔥 **Full Workflow Example**
```sh
# Login to DockerHub
docker login

# Pull an image from DockerHub
docker pull ubuntu:latest

# List available images
docker images

# Tag the image with your DockerHub username
docker tag ubuntu:latest myusername/ubuntu-custom:1.0

# Push the image to DockerHub
docker push myusername/ubuntu-custom:1.0

# Verify it on DockerHub
```

---

## 🔥 **Bonus: Pull Image from Private Repository**  
If an image is **private**, you must **log in** before pulling:  
```sh
docker login  
docker pull myusername/private-image:latest
```

---

### ✅ **Key Takeaways**  
🔹 `docker pull` → Download images from DockerHub  
🔹 `docker push` → Upload images to DockerHub  
🔹 `docker login` → Authenticate to DockerHub  
🔹 `docker tag` → Rename an image before pushing  
🔹 `docker images` → List local images  
🔹 `docker rmi` → Remove unwanted images  

🚀 **Now you can easily manage Docker images with DockerHub!** Let me know if you need more details. 😊
## 🎯 Orchestrating Docker with Kubernetes (Introduction)
Kubernetes is a container orchestration system for automating deployment, scaling, and operations.

🔹 Commands:
```
kubectl get nodes
kubectl apply -f deployment.yaml
kubectl get pods
kubectl logs <pod-name>
```

---
credit to =Trainwithshubham
