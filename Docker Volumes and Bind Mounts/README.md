**Day 27 – Docker Bind Mounts and Volumes Explained (with Live Demo)**

---

Hello everyone, my name is Abhishek and welcome back to my channel. Today is **Day 27** of our complete DevOps journey. In this class, we will talk about **Docker Bind Mounts and Volumes**.

This is a topic where many people get confused and often think Docker is complicated. But I’ll break it down with simple real-world problems and demos so you’ll understand clearly *why* these features exist and *how* to use them properly.

---

**Section 1: Understanding the Problem**

Let’s start with a few problems that Docker Volumes and Bind Mounts help solve:

**🧪 Problem 1: Container Log Data Loss**
Imagine you installed **nginx** in a container. It logs IP addresses and user sessions. If the container is stopped or crashes, **all that log data is lost**—because containers are ephemeral (short-lived). Logs saved inside a container vanish when it goes down.

**🧪 Problem 2: Sharing Data Between Containers**
You have a **backend container** that writes a file and a **frontend container** that reads it. If the backend goes down and doesn’t persist that file, the frontend has nothing to read.

**🧪 Problem 3: Host-Generated Files**
Imagine a cron job running on the host machine is writing files. Your container app needs to read those files. But containers, by default, **can’t access host file paths**.

These are common real-world issues.

---

**Section 2: Solution – Bind Mounts and Volumes**

To solve these problems, Docker offers two ways to persist and share data:

### ✅ Option 1: Bind Mounts

* Binds a **host machine directory** to a **container directory**.
* Example: Host `/var/data` can be mounted inside container at `/app`.
* Use case: Simple development needs, local directory sharing.

### ✅ Option 2: Docker Volumes

* Created and managed **by Docker CLI**.
* Volume data is **not tied to specific host paths**.
* More portable and production-friendly.

---

**Section 3: Bind Mount Example**

```bash
docker run -d \
  --name my-nginx \
  -v /home/user/nginx-logs:/var/log/nginx \
  nginx
```

What this does:

* Mounts `/home/user/nginx-logs` from host to container's log path.
* Even if the container dies, logs are preserved on host.

---

**Section 4: Docker Volume Example**

### 🔨 Create Volume:

```bash
docker volume create mydata
```

### 🔍 Inspect Volume:

```bash
docker volume inspect mydata
```

### 🏗️ Run Container with Volume:

```bash
docker run -d \
  --name app1 \
  --mount source=mydata,target=/app \
  ubuntu
```

### 📋 List Volumes:

```bash
docker volume ls
```

### 🗑️ Remove Volumes (after stopping containers):

```bash
docker volume rm mydata
```

---

**Section 5: mount vs -v Syntax**

Two ways to mount:

### Option 1: `-v` (short form, compact but less readable)

```bash
docker run -v mydata:/app ubuntu
```

### Option 2: `--mount` (long form, more verbose and readable)

```bash
docker run --mount source=mydata,target=/app ubuntu
```

Use `--mount` in production or teams for clarity.

---

**Section 6: Demo Recap**

1. Created a volume:

   ```bash
   docker volume create abhishek
   ```
2. Ran a container with mounted volume:

   ```bash
   docker run -d --name vol-demo \
     --mount source=abhishek,target=/app \
     nginx
   ```
3. Verified mount via inspect:

   ```bash
   docker inspect vol-demo
   ```
4. Deleted volume after stopping container:

   ```bash
   docker stop vol-demo
   docker rm vol-demo
   docker volume rm abhishek
   ```

---

**Conclusion**

📌 Bind Mounts are useful for development.
📌 Volumes are best for production.
📌 Volumes are portable, easier to manage, and better for backups.

**Homework:** Try creating:

* A container with a bind mount
* A container with a named volume
* Share volume between two containers

If you liked this session, like, share and comment your questions with timestamps. I’ll be happy to respond.

Thanks for watching – see you in the next class! 🚀
