**Day 28 – Docker Networking (with Deep Dive and Live Demo)**

---

Hello everyone, my name is Abhishek and welcome back to my channel. Today is **Day 28** of our complete DevOps course. In this class, we’ll cover a very important topic: **Docker Networking**.

This is also known as **Container Networking**, but since we've been using Docker to understand containers from the beginning, we'll focus on Docker’s networking features to help you practice and apply this knowledge.

If you haven’t watched the previous videos, please check them out. We follow a structured path that will help you when we transition to Kubernetes.

---

**Section 1: Why Docker Networking?**

🔍 **Problem Scenarios:**

1. One container (frontend) needs to talk to another container (backend)
2. One container (login) should be **isolated** from another (finance) for security reasons

💡 Networking in Docker enables:

* Communication between containers
* Isolation of containers from each other or the host

🧱 In VMs, each has its own OS and networking (e.g., 172.16.x.x), making them naturally isolated.
🪶 Containers are lightweight and don’t come with full OS, so Docker provides networking features for control and isolation.

---

**Section 2: How Containers Talk to Host?**

* Host has interface `eth0` (e.g., 192.168.x.x)
* Container gets an IP (e.g., 172.17.x.x) using its own `eth0`
* These are different subnets

📡 **Bridge Network (Default):**
Docker creates a virtual ethernet interface (veth) called `docker0`, which acts as a **bridge** between host and container.

Without `docker0`, container cannot talk to host ➝ application becomes unreachable.

So:

* 🔗 `docker0` bridges host ↔ container
* This is called **Bridge Networking**

---

**Section 3: Types of Docker Networking**

1. **Bridge Network (default):**

   * Every container is connected via `docker0`
   * All containers can talk to each other

2. **Host Network:**

   * Containers use the host's network directly
   * Less secure because host and container share network stack

3. **Overlay Network:**

   * Used in Swarm or Kubernetes for multi-host container communication
   * Not needed in basic Docker setups

---

**Section 4: Custom Bridge Networks for Isolation**

If all containers use the default bridge, they can talk to each other. This is bad for sensitive apps.

✅ **Solution:** Create custom bridge networks

Example:

* login + logout containers ➝ default bridge
* finance container ➝ secure custom bridge

This breaks the communication path between them.

```bash
# Create a custom bridge network
$ docker network create secure-network

# Run finance container with custom network
$ docker run -d --name finance --network secure-network nginx

# Run login container with default network
$ docker run -d --name login nginx
```

Now `login` cannot ping `finance`, but can still ping `logout` if on same default bridge.

---

**Section 5: Live Demo Walkthrough**

1. **List Default Networks:**

```bash
docker network ls
```

You’ll see `bridge`, `host`, and `none` by default

2. **Create and Run Containers:**

```bash
# Run login container
$ docker run -d --name login nginx

# Run logout container
$ docker run -d --name logout nginx

# Ping logout from login
$ docker exec -it login bash
$ apt update && apt install iputils-ping
$ ping <logout-container-ip>
```

3. **Create Secure Network and Isolate:**

```bash
$ docker network create secure-network
$ docker run -d --name finance --network secure-network nginx
```

4. **Verify Isolation:**

```bash
# Try to ping finance from login container (should fail)
$ ping <finance-container-ip>
```

5. **Run Container with Host Network:**

```bash
$ docker run -d --name host-demo --network=host nginx
$ docker inspect host-demo
# You’ll see no custom IP, because it uses host networking
```

---

**Conclusion**

🎯 Networking in Docker helps us:

* Allow or restrict container-to-container communication
* Connect containers to the host
* Improve security with custom bridge networks

👨‍🏫 Next class we’ll either:

* Cover Docker Interview Questions, or
* Move on to Kubernetes

Stay tuned and comment your queries below.

👉 If you liked the video, please like, share, and subscribe.
👉 Share with friends pursuing DevOps careers – we’ve already covered 28 sessions!

Thanks for watching. See you in the next class! 🚀
