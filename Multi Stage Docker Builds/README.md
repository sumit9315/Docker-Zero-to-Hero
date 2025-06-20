**Day 26 – Multi-Stage Docker Builds and Distroless Images Explained (with Practical Demo)**

---

hello everyone my name is Abhishek and welcome back to my channel so today we are at Day 26 of our complete DevOps course and this video is going to be both practical as well as theoretical. This is a very important session because I am going to explain some **production issues** related to Docker and containers, and also how to answer **real interview questions** like:

> “What are some Docker issues you've faced in production, and how did you solve them?”

So let’s begin.

---

**What Will We Learn Today?**

1. What is a Multi-Stage Docker Build?
2. What are Distroless Images?
3. Why do we need them in production?
4. Real-world demo using Go language app
5. Drastic image size reduction

All code is available in my GitHub repo: **`docker-0-to-hero`** → folder: `examples/golang-multistage-docker-build`

---

**Understanding the Problem**

Suppose you're containerizing a Python calculator app. Normally, you'd write a Dockerfile like:

```dockerfile
FROM ubuntu
WORKDIR /app
RUN apt update && apt install -y python3 pip
COPY . .
RUN pip install -r requirements.txt
CMD ["python3", "calculator.py"]
```

🚨 Problem: This image includes Ubuntu, apt, curl, and more—just to run a small script. Final image becomes 800MB+.

**But do we need all of it?** NO.

For running, we only need:

* Python runtime
* Our code

We don’t need all build tools, system packages, or unused binaries.

---

**Enter: Multi-Stage Docker Builds**

🔧 Multi-stage builds split the build process and the runtime into multiple layers:

1. **Stage 1: Build Stage**

```dockerfile
FROM ubuntu AS build
RUN apt update && apt install -y golang
COPY . .
RUN go build -o calculator calculator.go
```

2. **Stage 2: Runtime Stage (minimal image)**

```dockerfile
FROM scratch
COPY --from=build /calculator /calculator
ENTRYPOINT ["/calculator"]
```

👉 `scratch` is the smallest base image (\~0MB) — it has **nothing**.

💡 You copy just the binary (or `.pyc`, `.jar`, `.ear`, etc.), making the final image extremely small.

---

**Benefits**

* ✅ Significantly smaller images
* ✅ Fewer vulnerabilities
* ✅ Faster image pull & deploy
* ✅ Only includes what’s absolutely needed

---

**Distroless Images – What & Why?**

📌 A **Distroless image** is an ultra-minimal image containing **only** the required runtime (like Python or Java).

* It **doesn't** include:

  * shell (`/bin/sh`)
  * curl, wget
  * apt, yum
  * even basic tools like `ls`, `find`

* Result: Very high security & tiny image size

💬 Interview Answer:

> "We moved to Distroless images for production because they contain no package manager or shell, reducing OS-level vulnerabilities. Our Go-based images went from 861MB to just 1.83MB."

---

**Real Demo: Golang App – With & Without Multi-Stage**

We are using a Go calculator app that takes input like `2*100`, `5/2`, etc.

1. **Without Multi-Stage (with Ubuntu):**

```dockerfile
FROM ubuntu
RUN apt update && apt install -y golang
COPY . .
RUN go build -o calculator calculator.go
ENTRYPOINT ["/calculator"]
```

🔍 Resulting image: `861MB`

2. **With Multi-Stage + Scratch (Distroless)**

```dockerfile
FROM ubuntu AS build
RUN apt update && apt install -y golang
COPY . .
RUN go build -o calculator calculator.go

FROM scratch
COPY --from=build /calculator /calculator
ENTRYPOINT ["/calculator"]
```

🔍 Resulting image: `1.83MB`

💥 800x smaller image

---

**Best Practices**

* Always separate build and run stages
* Use minimal or Distroless base images (e.g., `scratch`, `gcr.io/distroless/python3`)
* Use `COPY --from=build` to pull only what's needed
* Avoid installing packages in final image

---

**Distroless Image Reference**

👉 Google maintains distroless images:

* [https://github.com/GoogleContainerTools/distroless](https://github.com/GoogleContainerTools/distroless)

Example for Java:

```dockerfile
FROM gcr.io/distroless/java:11
COPY app.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

---

**Interview Tip:**

> "We initially used Ubuntu base images, but they were bloated and had security flaws. So we migrated to multistage builds and distroless images like `scratch` and `gcr.io/distroless/python3`. This reduced image size by 10x–100x and enhanced container security."

---

**Conclusion**

* ✅ Use multi-stage builds to split build and runtime
* ✅ Use distroless or scratch to minimize final image size
* ✅ Significantly reduce security risk

---

📌 **Homework**:

* Try creating a multistage Docker build for your Node, Python, or Java app.
* Replace final stage with a distroless image (from Google distroless repo)
* Observe size difference and performance

Thank you for watching! Please like, share, and subscribe 🙏

See you in Day 27 where we will cover Docker Networking in depth! 👋
