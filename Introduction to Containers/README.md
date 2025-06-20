**Day 23 – Introduction to Containers | DevOps Course**

---

Hello everyone, my name is Abhishek and welcome back to my channel.

Today is **Day 23** of our complete DevOps course and in this class, I'll introduce you to the world of **containers**.

---

### 🚨 Announcement:

I’ve created a **Telegram group** for this channel! I’ll use this to share important resources (articles, Medium blogs, tools, etc.). It’s easier than YouTube Community posts, which get buried or ignored.

📌 Use the link in the description to join the Telegram Channel.

---

### 🐳 Today's Topic: Containers

Today we’ll:

* Understand the **basics of containers** (crucial before diving into projects)
* Learn what **Docker** is (just an intro, no commands/projects yet)
* Touch on **Buildah**, a powerful tool for building containers

> This is an **introductory class**, so we won’t create projects or run commands yet. First, let's get our foundations strong.

---

### 🖥️ Prerequisite: Understand Virtual Machines

Please **watch Day 3** of this course first. It explains Virtual Machines.

Why?

* Virtual machines are an evolution over physical servers
* Containers are an evolution over virtual machines

Let’s revisit the Virtual Machine (VM) concept briefly:

* You buy a physical server (IBM, HP, even your laptop)
* You **rarely use full resources** (CPU, RAM, storage)
* Organizations pay for unused capacity ➡️ **inefficient**

**Solution** ➡️ Use a **hypervisor** (VMWare, Xen, KVM) to run **multiple virtual machines (VMs)** on a single physical server.

Each VM:

* Has its own **operating system** (OS)
* Runs its own **applications**
* Offers **isolation**, **security**, and better **resource utilization**

---

### 💡 Why Shift from VMs to Containers?

If VMs are good, why do we need containers?

Because:

* Even VMs **waste resources**
* Your application may never use full CPU/RAM

➡️ Example:

* You assign 25 GB RAM and 25 CPU to a VM
* App uses only 10 GB and 6 CPU at peak load
* Huge **resource wastage** = **money lost**

Think about 1 million EC2 instances wasting resources 😱

➡️ Enter **Containers**: Even more **lightweight** and **efficient** than VMs!

---

### 📊 Containers vs Virtual Machines

* Containers share the **host OS kernel**
* VMs have a **full OS per VM** ➡️ more secure but heavy
* Containers offer **faster startup**, **less overhead**, and **better efficiency**

---

### 🧱 Container Architecture

Two models to run containers:

**Model 1: On Physical Server**

* Physical Server

  * Host OS

    * Docker / Container Runtime

      * Multiple containers

**Model 2: On Virtual Machine** (More popular now)

* Physical Server (cloud vendor or on-prem)

  * Virtual Machine (e.g., EC2)

    * Host OS

      * Docker / Podman / CRI-O

        * Containers

📌 **Model 2** is more common today (cloud era = no data center maintenance).

---

### ⚙️ Why Containers Are Lightweight

Unlike VMs, containers:

* **Do NOT** have a full OS
* Have a **minimal OS** or **base image**
* Use **host OS** for shared libraries

> Container = Application + App Libraries + Minimal OS

### 💾 Storage Comparison:

* VM snapshot = \~1 GB to 3 GB
* Container image = \~100 MB to 500 MB (depends on base image)

Containers are:

* Easier to **transfer/deploy**
* More **portable**
* **Faster** to start and stop

---

### 🔁 Container Lifecycle

Using **Docker** as the platform:

1. **Write Dockerfile**
2. **Docker build** → Create Docker Image
3. **Docker run** → Launch Container

🧠 Behind the scenes:

* Docker Engine receives these commands and manages the entire lifecycle.

### 🔁 Lifecycle Commands

```bash
docker build -t myapp .
docker run myapp
```

---

### 🛑 Problem with Docker Engine (SPOF)

Docker heavily depends on **Docker Engine**:

* If the engine crashes, **all containers stop** ❌
* Docker Engine = **Single Point of Failure (SPOF)**

Also:

* Docker builds create **multiple layers**
* Storage overhead increases

---

### 🧰 Solution: Use Buildah

To solve Docker limitations, use **Buildah**:

* Works without Docker Engine
* Solves SPOF
* Supports **layerless builds**
* Compatible with **Docker images**

Instead of Dockerfile, you can:

* Write a **shell script**
* Use **Buildah commands** to build images

Works great with:

* Podman
* Skopeo

📌 In this course, we’ll also learn Buildah with hands-on examples

---

### ✅ Summary

* Containers are an **evolution over VMs**
* Offer better **efficiency, speed, and portability**
* Docker = most popular tool for containerization
* Buildah = newer tool solving Docker’s limitations

---

📌 **Homework / Action Items:**

* Rewatch Day 3 (Virtual Machines) if unclear
* Share your doubts with **timestamps** in comments
* Like, comment, and subscribe!

Thank you! See you in the next video 👋

---
