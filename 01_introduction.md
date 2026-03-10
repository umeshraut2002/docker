

---

# 01 - INTRODUCTION TO DOCKER

### Definition

**Docker** is a **containerization platform** used to package an application along with its dependencies, libraries, and runtime into a **portable container** that can run consistently across different environments.

It solves the classic problem:

> “It works on my machine but not in production.”

### Key Idea

Docker packages everything needed to run software into **containers** so the application behaves the same in:

* developer laptop
* testing server
* staging
* production cloud infrastructure

### Example Scenario (Real DevOps Use Case)

A team develops a **Node.js web application**.

Problem without Docker:

| Environment      | Issue    |
| ---------------- | -------- |
| Developer laptop | Node v16 |
| QA server        | Node v18 |
| Production       | Node v14 |

Application crashes due to **dependency mismatch**.

Solution:

Docker container contains:

* Node runtime
* dependencies
* OS libraries
* application code

Now the same container runs **everywhere**.

---

# 2. Why Docker Was Created

Before Docker, teams used **Virtual Machines (VMs)**.

Example using **VMs**

```
Server
 ├── VM1
 │   └── Ubuntu + App1
 ├── VM2
 │   └── Ubuntu + App2
 └── VM3
     └── Ubuntu + App3
```

Problems:

1. Heavy resource usage
2. Slow startup time
3. Large storage consumption
4. Difficult scaling

VM includes:

* full OS
* kernel
* libraries
* application

---

# 3. Containers vs Virtual Machines

| Feature     | Containers    | Virtual Machines |
| ----------- | ------------- | ---------------- |
| Boot Time   | Seconds       | Minutes          |
| Size        | MBs           | GBs              |
| OS          | Shared Kernel | Full OS          |
| Performance | High          | Lower            |
| Isolation   | Process level | Hardware level   |

Example:

```
Host OS
 ├── Docker Engine
 │   ├── Container 1
 │   ├── Container 2
 │   └── Container 3
```

Containers **share the host OS kernel**.

---

# 4. What is Containerization

### Definition

Containerization is the process of **packaging an application and its dependencies into isolated environments called containers**.

Each container includes:

* application code
* runtime
* system libraries
* dependencies

But it **shares host kernel**.

---

# 5. Docker Architecture

Docker follows a **client-server architecture**.

```
Docker Client
      |
      v
Docker Daemon (dockerd)
      |
      v
Docker Objects
 ├── Images
 ├── Containers
 ├── Networks
 └── Volumes
```

### Components

#### Docker Client

Command line interface:

```
docker build
docker run
docker pull
docker push
```

It sends commands to Docker daemon.

---

#### Docker Daemon (dockerd)

The **background service** responsible for:

* building images
* running containers
* managing networks
* managing volumes

---

#### Docker Registry

Storage for Docker images.

Examples:

* Docker Hub
* Private registry
* Cloud registries

Example:

```
docker pull nginx
```

Docker downloads image from registry.

---

# 6. Docker Images

### Definition

A **Docker Image** is a **read-only template** used to create containers.

It contains:

* application code
* libraries
* runtime
* environment configuration

### Example

```
nginx image
node image
python image
```

---

### Image Layers (Important Interview Topic)

Docker images use **layered filesystem**.

Example:

```
Layer 1 → Ubuntu base
Layer 2 → Node runtime
Layer 3 → App dependencies
Layer 4 → Application code
```

Advantages:

* Faster builds
* Layer caching
* Reduced storage

---

# 7. Docker Containers

### Definition

A **container is a running instance of an image**.

Example:

```
docker run nginx
```

Creates:

```
nginx image → nginx container
```

---

### Container Lifecycle

```
created → running → paused → stopped → removed
```

Commands:

```
docker run
docker stop
docker start
docker restart
docker rm
```

---

# 8. Dockerfile

### Definition

A **Dockerfile** is a text file containing instructions to build Docker images.

Example Dockerfile:

```
FROM node:18

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["node", "server.js"]
```

---

### Dockerfile Instructions (Interview Important)

| Instruction | Purpose                   |
| ----------- | ------------------------- |
| FROM        | base image                |
| WORKDIR     | working directory         |
| COPY        | copy files                |
| ADD         | copy with extraction      |
| RUN         | execute command           |
| CMD         | default container command |
| ENTRYPOINT  | main executable           |

---

# 9. Docker Build Process

Example:

```
docker build -t myapp .
```

Process:

```
Dockerfile
   ↓
Build context
   ↓
Layer creation
   ↓
Image created
```

Docker uses **layer caching** to speed builds.

---

# 10. Docker Networking

Containers communicate using networks.

Types:

| Network | Description                   |
| ------- | ----------------------------- |
| bridge  | default network               |
| host    | container shares host network |
| none    | no networking                 |
| overlay | multi-host networking         |
| macvlan | container gets MAC address    |

Example:

```
docker network create mynetwork
```

---

# 11. Docker Volumes

### Problem

Containers are **ephemeral**.

If container stops:

```
data lost
```

### Solution

Volumes store persistent data.

Example:

```
docker volume create dbdata
```

Use:

```
docker run -v dbdata:/var/lib/mysql mysql
```

---

# 12. Docker Compose

### Definition

Docker Compose is used to run **multi-container applications**.

Example:

Web App Architecture:

```
Frontend
Backend
Database
Redis
```

Instead of running containers individually, use **compose file**.

Example:

```
docker-compose.yml
```

```
version: '3'

services:

  web:
    image: nginx
    ports:
      - "80:80"

  db:
    image: mysql
```

Run:

```
docker compose up
```

---

# 13. Docker in CI/CD Pipelines

In DevOps pipelines:

```
Developer
   ↓
Git push
   ↓
CI pipeline
   ↓
Docker image build
   ↓
Push to registry
   ↓
Deployment
```

Tools used:

* Jenkins
* GitHub Actions
* GitLab CI
* ArgoCD

Example pipeline step:

```
docker build -t myapp:v1 .
docker push registry/myapp:v1
```

---

# 14. Docker in Microservices Architecture

Example architecture:

```
User Service → Container
Payment Service → Container
Auth Service → Container
Database → Container
```

Benefits:

* independent deployment
* horizontal scaling
* fault isolation

---

# 15. Docker Security

Best practices:

* Use **minimal base images**
* Avoid running as root
* Use **image scanning**
* Use **Docker secrets**

Example:

```
USER appuser
```

---

# 16. Docker Behind the Scenes (Very Important)

Docker uses **Linux kernel features**:

### Namespaces

Provide isolation for:

* process
* network
* user
* filesystem

Each container sees its **own environment**.

---

### cgroups (Control Groups)

Limit resources:

* CPU
* memory
* disk
* network

Example:

```
docker run --memory="512m"
```

---

### Union File System

Used for **image layering**.

Example drivers:

* OverlayFS
* AUFS
* Btrfs

---

### containerd

Low-level runtime used by Docker to manage containers.

---

### runc

Responsible for **creating and running containers**.

---

# 17. Docker vs Kubernetes

| Docker            | Kubernetes              |
| ----------------- | ----------------------- |
| Container runtime | Container orchestration |
| Runs containers   | Manages clusters        |
| Single host       | Multi-node              |

Example:

```
Docker → runs containers
Kubernetes → manages thousands of containers
```

---

# 18. Real Production Deployment Scenario

Example: **E-commerce application**

Architecture:

```
Load Balancer
     |
Frontend Container (React)
     |
Backend Container (Node.js)
     |
Database Container (PostgreSQL)
     |
Redis Cache Container
```

Deployment flow:

```
Developer → GitHub
       ↓
CI/CD pipeline
       ↓
Docker build
       ↓
Push to registry
       ↓
Deploy on Kubernetes / Docker Swarm
```

---

# 19. Common Docker Interview Questions

### Q1 What is Docker?

Platform for containerizing applications.

---

### Q2 Difference between Image and Container?

| Image     | Container        |
| --------- | ---------------- |
| blueprint | running instance |

---

### Q3 What is Dockerfile?

File with instructions to build images.

---

### Q4 What happens when you run docker run?

Steps internally:

1. Check image locally
2. Pull from registry if missing
3. Create container
4. Setup filesystem
5. Setup network
6. Start container process

---

### Q5 What is Docker volume?

Persistent storage for containers.

---

# 20. Best Practices in Production

1. Use **multi-stage builds**
2. Keep images small
3. Use **.dockerignore**
4. Avoid root user
5. Use **health checks**

Example:

```
HEALTHCHECK CMD curl --fail http://localhost || exit 1
```

---
---
