
---

# 1. Docker Installation on Local Machine (Using Docker Desktop)

Docker Desktop is the easiest way to run Docker locally on **Windows or macOS**.

Docker Desktop includes:

* Docker Engine
* Docker CLI
* containerd
* Kubernetes (optional)
* GUI dashboard

Provided by **Docker, Inc.**.

---

# Step 1 — Check System Requirements

Before installing Docker Desktop ensure:

### Windows

Requirements:

* Windows 10 / 11 (64-bit)
* WSL2 enabled
* Virtualization enabled in BIOS

Check virtualization:

Open **Task Manager → Performance → CPU**

Look for:

```text
Virtualization: Enabled
```

---

# Step 2 — Enable WSL2 (Windows Subsystem for Linux)

Docker Desktop uses **Windows Subsystem for Linux**.

Open PowerShell as Administrator:

```powershell
wsl --install
```

Restart the system.

Verify installation:

```powershell
wsl --status
```

---

# Step 3 — Download Docker Desktop

Download Docker Desktop from:

Official website:

**Docker Desktop**

Steps:

1. Open browser
2. Go to docker.com
3. Download Docker Desktop for Windows

---

# Step 4 — Install Docker Desktop

Run installer.

Installation steps:

```text
1 Double-click installer
2 Enable WSL2 integration
3 Click Install
4 Restart computer
```

After installation Docker service starts automatically.

---

# Step 5 — Verify Docker Installation

Open terminal:

```bash
docker --version
```

Example output:

```text
Docker version 26.x.x
```

Check Docker engine:

```bash
docker info
```

---

# Step 6 — Run First Container

Run a test container:

```bash
docker run hello-world
```

What happens internally:

```text
1 Docker client sends request
2 Docker daemon checks image
3 Image downloaded from registry
4 Container created
5 Test message printed
```

Example output:

```text
Hello from Docker!
This message shows Docker is working.
```

---

# Step 7 — Test Docker Container

Run nginx container:

```bash
docker run -d -p 8080:80 nginx
```

Open browser:

```text
http://localhost:8080
```

You should see the **nginx welcome page**.

---

# Step 8 — Check Running Containers

Command:

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE   STATUS   PORTS
abc123         nginx   Up       0.0.0.0:8080->80/tcp
```

---

# Local Docker Architecture (After Installation)

```text
Developer
   │
Docker CLI
   │
Docker Desktop
   │
Docker Engine
   │
containerd
   │
runc
   │
Linux Kernel (WSL2)
```

---

# 2. Docker Installation on AWS Cloud (EC2)

Now imagine a real DevOps scenario.

You want to deploy containers on cloud infrastructure like **Amazon Web Services**.

We will install Docker on an EC2 server.

---

# Step 1 — Launch EC2 Instance

Login to AWS console.

Navigate to:

**Amazon EC2**

Launch instance.

Recommended configuration:

```text
AMI: Amazon Linux 2023
Instance Type: t2.micro
Storage: 8 GB
Security Group: Allow SSH
```

Launch instance.

Download **key pair (.pem)**.

---

# Step 2 — Connect to EC2 Server

From your terminal:

```bash
ssh -i mykey.pem ec2-user@EC2_PUBLIC_IP
```

Example:

```bash
ssh -i devops.pem ec2-user@13.233.120.45
```

Now you are inside the server.

---

# Step 3 — Update System Packages

First update packages:

```bash
sudo yum update -y
```

This ensures the system is updated.

---

# Step 4 — Install Docker

Install Docker using package manager.

```bash
sudo yum install docker -y
```

This installs:

* Docker engine
* containerd
* CLI tools

---

# Step 5 — Start Docker Service

Start Docker daemon:

```bash
sudo systemctl start docker
```

Enable Docker to start automatically:

```bash
sudo systemctl enable docker
```

Check service status:

```bash
sudo systemctl status docker
```

---

# Step 6 — Allow Non-Root Docker Access

By default Docker requires **sudo**.

Add user to docker group:

```bash
sudo usermod -aG docker ec2-user
```

Apply changes:

```bash
newgrp docker
```

Now you can run Docker without sudo.

---

# Step 7 — Verify Docker Installation

Run:

```bash
docker --version
```

Test container:

```bash
docker run hello-world
```

Expected output:

```text
Hello from Docker!
```

---

# Step 8 — Run Web Server Container

Deploy nginx container:

```bash
docker run -d -p 80:80 nginx
```

Now the web server runs inside container.

Open browser:

```text
http://EC2_PUBLIC_IP
```

You will see nginx page.

---

# Step 9 — Check Running Containers

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE   STATUS   PORTS
abc123         nginx   Up       0.0.0.0:80->80/tcp
```

---

# Cloud Docker Architecture

When running on AWS:

```text
User
 │
SSH
 │
EC2 Server
 │
Docker Engine
 │
containerd
 │
runc
 │
Linux Kernel
 │
Containers
```

---

# Local vs Cloud Docker Installation

| Feature        | Local (Docker Desktop) | AWS EC2      |
| -------------- | ---------------------- | ------------ |
| Environment    | developer machine      | cloud server |
| Virtualization | WSL2                   | native Linux |
| Installation   | GUI installer          | CLI commands |
| Use case       | development            | production   |

---

# DevOps Best Practice

Typical workflow:

```text
Developer Laptop
      │
Docker Build
      │
Push Image
      │
Container Registry
      │
Cloud Server (AWS)
      │
Docker Run
      │
Application Running
```

---