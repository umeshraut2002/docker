# Docker Internals

*(What actually happens when you run `docker run nginx`)*

Imagine you are a **DevOps engineer deploying a web server** for a company.
You type a simple command:

```bash
docker run nginx
```

It looks simple. But inside the system, **many components work together step-by-step**.

Let’s walk through the **full internal journey**.

---

# Scene 1 — The Developer Runs the Command

You open the terminal and run:

```bash
docker run nginx
```

At this moment the request goes to the **Docker Client**.

Architecture:

```
User (CLI)
   │
   ▼
Docker Client
   │
   ▼
Docker Daemon
```

The Docker client sends a request to the background service called **dockerd**.

The daemon now begins a **multi-step orchestration process**.

---

# Scene 2 — Docker Checks if the Image Exists

First question Docker asks:

> "Do I already have the nginx image locally?"

The daemon checks the **local image cache**.

Possible situations:

### Case 1 — Image already exists

Docker uses the existing image.

### Case 2 — Image not found

Docker automatically pulls the image from **Docker Hub**.

Internal process:

```
Docker Daemon
      │
      ▼
Docker Registry (Docker Hub)
      │
      ▼
Download image layers
```

Important concept:

Docker images are **not single files**.

They are **layers**.

Example nginx image layers:

```
Layer 1 → Linux base OS
Layer 2 → system libraries
Layer 3 → nginx installation
Layer 4 → configuration
```

Docker downloads only missing layers.

This is where the **Union File System** comes in.

---

# Scene 3 — Union File System Assembles the Image

Docker uses a **Union File System** like **OverlayFS**.

Its job is to **merge multiple layers into one virtual filesystem**.

Example:

```
Image Layers

Layer 4 → nginx config
Layer 3 → nginx binary
Layer 2 → libraries
Layer 1 → base OS
```

Union filesystem merges them:

```
Merged Root Filesystem
```

Then Docker creates a **new writable layer on top**.

```
Container Writable Layer
      │
Image Layers (Read-only)
```

Why?

Because **images must remain immutable**.

Any change inside the container is stored in the **top writable layer**.

---

# Scene 4 — Container Creation Begins

Now Docker starts creating the container.

Container lifecycle begins here.

Lifecycle states:

```
Created → Running → Paused → Stopped → Deleted
```

After `docker run`, the container enters:

```
Created → Running
```

But before running, Docker must **build the container environment**.

This involves **Linux kernel features**.

---

# Scene 5 — Namespaces Create Isolation

Containers need isolation.

Docker uses **Linux Namespaces**.

Namespaces create the illusion that each container has **its own system**.

Types of namespaces used:

### PID Namespace

Process isolation.

Inside the container:

```
PID 1 → nginx
```

Even though the host has thousands of processes.

The container only sees its own processes.

---

### Network Namespace

Each container gets its own:

```
IP address
network interfaces
routing table
```

Example:

```
Container Network

eth0
IP: 172.17.0.2
```

This is separate from the host network.

---

### Mount Namespace

Each container sees its own filesystem.

Example:

```
/
├── bin
├── etc
├── usr
└── nginx
```

Even though the real filesystem belongs to the host.

---

### UTS Namespace

Container has its own:

```
hostname
domain name
```

Example:

```
container-id
```

---

### IPC Namespace

Controls interprocess communication.

Prevents containers from interfering with each other's memory.

---

# Scene 6 — cgroups Apply Resource Limits

Now Docker controls resource usage using **Control Groups (cgroups)**.

Without limits, a container could consume the entire server.

cgroups allow restrictions:

Example:

```
docker run --memory=512m nginx
```

This creates limits like:

```
CPU usage
Memory usage
Disk IO
Network bandwidth
```

Example structure:

```
cgroup
 ├── cpu
 ├── memory
 ├── blkio
 └── network
```

This ensures containers share server resources fairly.

---

# Scene 7 — Networking is Created

Now Docker connects the container to a network.

Default network:

```
bridge network
```

Architecture:

```
Host Machine
   │
Docker Bridge (docker0)
   │
Container (172.17.0.2)
```

Steps internally:

1. Docker creates a **virtual ethernet pair (veth pair)**.

```
veth-host <----> veth-container
```

2. One end connects to container.

3. Other end connects to the bridge.

Result:

```
Container → Docker Bridge → Host → Internet
```

If ports are exposed:

Example:

```
docker run -p 80:80 nginx
```

Docker configures **iptables rules**.

Traffic flow:

```
User Browser
      │
Host Port 80
      │
Container Port 80
```

---

# Scene 8 — Container Runtime Starts the Container

Now Docker hands control to the container runtime.

Runtime stack:

```
Docker
  │
containerd
  │
runc
```

### containerd

Manages container lifecycle.

### runc

Actually **creates the container process**.

This follows the **OCI runtime specification**.

Created by **Open Container Initiative**.

---

# Scene 9 — The nginx Process Starts

Inside the container:

```
PID 1 → nginx
```

This becomes the **main container process**.

Important rule:

> A container lives as long as its main process runs.

If nginx stops:

```
container stops
```

---

# Scene 10 — Container is Running

Final state:

```
User Request
     │
     ▼
Host Port 80
     │
Docker NAT
     │
Container Network
     │
nginx process
```

Your web server is now live.

---

# Full Internal Flow (Simplified)

When running:

```
docker run nginx
```

The system performs:

```
1 Check image locally
2 Pull from registry if missing
3 Merge image layers (Union FS)
4 Create container metadata
5 Setup namespaces
6 Apply cgroups limits
7 Configure networking
8 Create writable layer
9 containerd starts runtime
10 runc launches process
11 nginx becomes PID 1
12 Container enters RUNNING state
```

---
---
