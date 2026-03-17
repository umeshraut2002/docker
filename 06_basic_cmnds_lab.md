
---

# Docker Practice Set (15)

## Scenario 1 — Run Nginx in Background

You need to run an **nginx container in detached mode** so the terminal remains free.

**Task:**
Run an nginx container in the background.

**Expected command**

```bash
docker run -d nginx
```

---

# Scenario 2 — Run Container With Custom Name

You want the nginx container name to be **webserver** instead of a random name.

**Task**

Run nginx with container name `webserver`.

**Solution**

```bash
docker run -d --name webserver nginx
```

---

# Scenario 3 — Access Nginx from Browser

You deployed nginx but want to access it from the browser using **port 8080**.

**Task**

Map host port **8080** to container port **80**.

**Solution**

```bash
docker run -d -p 8080:80 nginx
```

Access:

```
http://localhost:8080
```

---

# Scenario 4 — View Running Containers

You want to see **all currently running containers**.

**Task**

Check running containers.

**Solution**

```bash
docker ps
```

---

# Scenario 5 — View All Containers

You suspect some containers exited.

**Task**

Show **running + stopped containers**.

**Solution**

```bash
docker ps -a
```

---

# Scenario 6 — Stop a Running Container

A container named `webserver` is consuming CPU.

**Task**

Stop the container safely.

**Solution**

```bash
docker stop webserver
```

---

# Scenario 7 — Restart a Stopped Container

The `webserver` container was stopped but you want to start it again.

**Solution**

```bash
docker start webserver
```

---

# Scenario 8 — Force Stop Container

A container is not stopping normally.

**Task**

Immediately terminate container `abc123`.

**Solution**

```bash
docker kill abc123
```

---

# Scenario 9 — Remove Stopped Container

You want to remove container `webserver` after stopping it.

**Solution**

```bash
docker rm webserver
```

---

# Scenario 10 — Remove Running Container

You want to remove a **running container forcefully**.

**Solution**

```bash
docker rm -f webserver
```

---

# Scenario 11 — Check Container Logs

Your application inside container is failing and you want to see logs.

**Task**

View logs of container `abc123`.

**Solution**

```bash
docker logs abc123
```

---

# Scenario 12 — Enter Running Container

You want to debug a container by entering its shell.

**Task**

Open bash shell inside container `abc123`.

**Solution**

```bash
docker exec -it abc123 bash
```

---

# Scenario 13 — Remove All Containers

Your Docker environment is messy and you want to delete **all containers**.

**Solution**

```bash
docker rm -f $(docker ps -a -q)
```

---

# Scenario 14 — Run MySQL Container

You want to start a MySQL container with root password `redhat123`.

**Solution**

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=redhat123 mysql
```

---

# Scenario 15 — See Container Storage Size

You want to check how much disk space containers are using.

**Solution**

```bash
docker ps -s
```

---

# Bonus Challenge (Real DevOps Scenario)

Deploy nginx with these conditions:

• Container name → `my-nginx`
• Run in background
• Map port **9000 → 80**

Try yourself first.

**Solution**

```bash
docker run -d --name my-nginx -p 9000:80 nginx
```

---
