
---

# 1. Install Docker on EC2 (Ubuntu)

Connect to your EC2 instance using SSH.

```bash
ssh -i sunflower_new.pem ubuntu@<your-ec2-public-ip>
```

Update packages:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io -y
```

Start Docker service:

```bash
sudo systemctl start docker
```

Enable Docker on boot:

```bash
sudo systemctl enable docker
```

Check Docker version:

```bash
docker --version
```

Check Docker service:

```bash
sudo systemctl status docker
```

---

### Run Docker without sudo

```bash
sudo usermod -aG docker $USER
```

Then reconnect SSH.

---

# 2. Basic Docker Commands (Practice + Explanation)

### Run container in foreground

```bash
docker run nginx
```

Explanation:

* Downloads the **Nginx** image if not present.
* Runs container in **foreground**.
* Terminal becomes attached to container logs.

---

### Run container in background

```bash
docker run -d nginx
```

Explanation:

* `-d` = **detached mode**
* Container runs in background.

---

### List running containers

```bash
docker ps
```

Shows:

* container ID
* image
* port
* status
* container name

---

### Run container with random port

```bash
docker run -d -P nginx
```

Explanation:

* `-P` exposes container ports to **random host ports**

Example:

```
0.0.0.0:32768->80/tcp
```

---

### Run container with custom name

```bash
docker run -d --name nginx-cbz nginx
```

Explanation:

* `--name` assigns container name.

---

### Run container with port mapping

```bash
docker run -d -p 8080:80 nginx
```

Explanation:

```
8080 → host port
80 → container port
```

Open in browser:

```
http://EC2-PUBLIC-IP:8080
```

You will see **Nginx welcome page**.

---

### Generic port mapping format

```bash
docker run -d -p host-port:container-port nginx
```

Example:

```
9000:80
5000:80
```

---

# 3. Docker Image Commands

### List images

```bash
docker images
```

Shows:

* repository
* tag
* image ID
* size

---

### Pull image manually

```bash
docker pull nginx
```

Downloads image from **Docker Hub.

---

### Remove image

```bash
docker rmi nginx
```

---

# 4. Container Lifecycle Commands

### Stop container

```bash
docker stop <container-id>
```

Gracefully stops container.

---

### Kill container

```bash
docker kill <container-id>
```

Immediately stops container.

---

### Start container

```bash
docker start <container-id>
```

Starts stopped container.

---

### Restart container

```bash
docker restart <container-id>
```

Stop + start.

---

### Remove container

```bash
docker rm <container-id>
```

Removes stopped container.

---

### Force remove running container

```bash
docker rm -f <container-id>
```

---

### Remove all containers

```bash
docker rm -f $(docker ps -aq)
```

Explanation:

```
docker ps -aq → all container IDs
$( ) → command substitution
```

---

# 5. Container Logs

View logs:

```bash
docker logs <container-id>
```

Example:

```bash
docker logs e5a7ed8b8c55
```

Follow logs (like tail):

```bash
docker logs -f <container-id>
```

---

# 6. Execute Command Inside Container

Open shell:

```bash
docker exec -it <container-id> bash
```

or

```bash
docker exec -it <container-id> sh
```

Explanation:

```
-i → interactive
-t → terminal
```

---

# 7. Environment Variables

Run container with env variable:

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=redhat123 mysql
```

Explanation:

* Sets environment variable for **MySQL container**.

---

# 8. Container Size

```bash
docker ps -s
```

Shows container storage usage.

---

# 9. Inspect Container

Detailed information:

```bash
docker inspect <container-id>
```

Shows:

* IP address
* network
* mounts
* config

---

# 10. Docker System Commands

### Disk usage

```bash
docker system df
```

---

### Remove unused resources

```bash
docker system prune
```

Removes:

* stopped containers
* unused networks
* dangling images

---

# 11. Docker Networking Commands

List networks:

```bash
docker network ls
```

Inspect network:

```bash
docker network inspect bridge
```

---

# 12. Docker Volumes

Create volume:

```bash
docker volume create myvolume
```

List volumes:

```bash
docker volume ls
```

Run container with volume:

```bash
docker run -d -v myvolume:/data nginx
```

---

# 13. Mini Practice Lab (Recommended)

Practice this sequence on EC2.

### Step 1

```bash
docker run -d -p 8080:80 --name web1 nginx
```

---

### Step 2

Check containers

```bash
docker ps
```

---

### Step 3

Check logs

```bash
docker logs web1
```

---

### Step 4

Enter container

```bash
docker exec -it web1 bash
```

---

### Step 5

Stop container

```bash
docker stop web1
```

---

### Step 6

Remove container

```bash
docker rm web1
```

---

# 14. Very Important DevOps Commands

You should also practice:

```
docker build
docker tag
docker push
docker pull
docker-compose
docker stats
docker cp
docker history
docker top
```