# Docker Overview

Docker is a platform for developing, shipping, and running applications inside containers. It enables developers to package applications and their dependencies into a standardized unit called a **container**, which can run consistently across different environments.

---

## **Docker Components**

### 1. Docker Engine
- The core component that runs and manages containers.
- Consists of the Docker daemon, REST API, and CLI.

### 2. Docker Daemon (`dockerd`)
- Runs in the background and manages containers.
- Handles container creation, networking, and storage.

### 3. Docker CLI (Command Line Interface)
- Used to interact with Docker via commands.
- Example: `docker run`, `docker build`, `docker ps`.

### 4. Docker Images
- Read-only templates that define how a container should run.
- Created using `Dockerfile` and stored in repositories.

### 5. Docker Containers
![docker container](Day1/docker1.png)
- Running instances of Docker images.
- Can be started, stopped, and deleted.

### 6. Docker Hub & Registry
- Public and private repositories to store and distribute images.
- Example: Docker Hub (public), AWS ECR, GCR, ACR (private).

### 7. Docker Compose
- Tool for defining and running multi-container Docker applications.
- Uses `docker-compose.yml` file.

### 8. Docker Networking
- Enables communication between containers and the external world.
- Network types: Bridge, Host, Overlay, Macvlan, and None.

### 9. Docker Volumes
- Persistent storage for containers.
- Helps retain data even after container restarts.

---

## **Docker Architecture**

![docker architecture](Day1/docker2.png)

Docker follows a **client-server architecture**, consisting of:

### 1. **Docker Client**
- Sends commands to the Docker daemon via REST API or CLI.
- Example: `docker run ubuntu`.

### 2. **Docker Daemon (`dockerd`)**
- Runs on the host machine.
- Listens to API requests and manages containers.

### 3. **Docker Images & Containers**
- Images are templates for creating containers.
- Containers are isolated environments that run applications.

### 4. **Docker Registries**
- Stores Docker images.
- Docker Hub is the default registry.

---

## **High-Level Workflow**
1. **Pull** an image from Docker Hub:
   ```sh
   docker pull nginx
   ```
2. **Run** a container:
   ```sh
   docker run -d -p 80:80 nginx
   ```
3. **Manage** containers:
   ```sh
   docker ps
   docker stop <container_id>
   docker rm <container_id>
   ```
4. **Build** a custom image:
   ```sh
   docker build -t myapp .
   ```
5. **Push** an image to a registry:
   ```sh
   docker push myapp