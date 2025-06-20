**Day 24 – Dockerfile Explained (with Examples)**

---

**Introduction**
Hello everyone, my name is Abhishek and welcome back to my channel! Today is **Day 24** of our Complete DevOps course. In this class, we will understand the concept and syntax of a **Dockerfile** with a detailed breakdown and hands-on example.

In **Day 23**, we already covered:

* What is a Container?
* What is Docker?
* Dockerfile, Docker Image, and Docker Container
* Docker Lifecycle

If you haven’t watched Day 23 yet, please do that first to fully understand today’s class.

---

**What Will We Cover Today?**

1. What is a Dockerfile?
2. Dockerfile Syntax Explained Line-by-Line
3. Best Practices for Writing Dockerfiles
4. Build Docker Image from Dockerfile
5. Run a Docker Container

---

**Section 1: What is a Dockerfile?**

A **Dockerfile** is a simple text file that contains a list of instructions Docker uses to build an image.

It automates the process of creating Docker images.

Think of it like a recipe: Docker reads the instructions and creates a container image accordingly.

---

**Section 2: Dockerfile Syntax – Line-by-Line**

Here's a basic example:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

✅ **FROM** – sets the base image (e.g., Node.js on Alpine Linux)
✅ **WORKDIR** – sets the working directory inside the container
✅ **COPY . .** – copies current directory content into the container
✅ **RUN** – runs a command (e.g., installing dependencies)
✅ **EXPOSE** – informs Docker which port the app runs on
✅ **CMD** – defines the default command to run when the container starts

---

**Section 3: Best Practices**

* Use official base images (e.g., node:18-alpine)
* Minimize the number of layers
* Use `.dockerignore` file to avoid copying unnecessary files
* Pin dependencies and base image versions
* Use multi-stage builds (for production-ready images)

---

**Section 4: Build Docker Image from Dockerfile**

```bash
docker build -t my-node-app .
```

* `-t` specifies a tag (name) for your image
* `.` means the Dockerfile is in the current directory

---

**Section 5: Run the Docker Container**

```bash
docker run -p 3000:3000 my-node-app
```

* `-p` maps port 3000 on your local machine to port 3000 in the container

---

**Conclusion & Homework**

That wraps up Day 24 of the DevOps series!

🧠 **Assignment**:

* Write your own Dockerfile for a simple Node.js or Python app
* Build and run it locally using Docker CLI

If you liked this video, don’t forget to like, share, and subscribe.

If you get stuck, drop a comment with your Dockerfile and error log – I’ll help you debug it.

Thank you, and see you in the next video! 👋
