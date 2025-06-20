**Day 25 – Containerizing a Django Web Application with Docker**

---

hello everyone my name is Abhishek and welcome back to my channel so today is day 25 of our complete DevOps course and in this class we will deploy our first Django web application as a Docker container.

In the previous classes:

* Day 23: We understood containers, Docker lifecycle, and why containers are lightweight compared to VMs.
* Day 24: We explored Dockerfiles, built sample CLI apps, and understood entrypoint vs CMD.

Today is the next big step—containerizing a real-world Django web app.

---

**Section 1: Why Learn This?**

Before we dive in, make sure you’ve seen Day 23 and Day 24. Those videos are the stepping stones.
Today, we are taking a basic Django app and making it run inside a Docker container. This is a common real-world scenario for DevOps engineers.

---

**Section 2: Django App Basics**

I wrote a basic Django app with a single-page frontend. Even if it had multiple pages, the containerization flow is the same.

You don’t need to be a developer, but as a DevOps engineer, you should:

* Understand where the app’s requirements come from
* Know what part connects frontend/backend
* Know where templates are rendered

This app contains:

* Project folder (`devops/`) created via `django-admin startproject`
* App folder (`demo/`) created via `python manage.py startapp demo`
* `views.py` has a Python function `index()` that renders an HTML template
* Template folder contains the frontend HTML

All backend config like allowed hosts, DB settings, middleware, etc., is inside `settings.py`. URLs are routed via `urls.py`, and the app is attached to `/demo` route.

---

**Section 3: The Classic Problem Before Docker**

Earlier, QA teams manually set up dependencies listed in `requirements.txt`. But due to OS differences (Windows, Mac, Linux), things often broke. Docker solves this by bundling everything together.

To run this app with Docker, a user only needs two commands:

```bash
docker build -t my-django-app .
docker run -p 8000:8000 my-django-app
```

No matter what OS the user has, it just works.

---

**Section 4: Writing the Dockerfile**

Here’s what the Dockerfile looks like:

```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY requirements.txt requirements.txt
RUN apt update && apt install -y python3 python3-pip
RUN pip3 install -r requirements.txt
COPY . .
ENTRYPOINT ["python3"]
CMD ["manage.py", "runserver", "0.0.0.0:8000"]
```

Explanation:

* Use `ubuntu` as base image (you can use `python` image too)
* Set working directory to `/app`
* Copy dependencies first to leverage Docker cache
* Install Python and pip
* Install required Python packages
* Copy app source code
* Set `ENTRYPOINT` to python3 (not overrideable)
* Set `CMD` to `manage.py runserver` (configurable)

This lets users override just the host/port if needed.

---

**Section 5: Build and Run the App**

Run the following:

```bash
# Clone the repo
git clone <repo-url>
cd python-web-app

# Build the Docker image
docker build -t my-django-app .

# Run with port mapping
docker run -p 8000:8000 my-django-app
```

Now open `http://<EC2-PUBLIC-IP>:8000/demo` in your browser.

---

**Section 6: Important Notes**

Make sure to:

* Add port 8000 to your EC2 instance’s **Security Group > Inbound Rules**.
* Choose type = Custom TCP, Port = 8000, Source = 0.0.0.0/0 (or your IP)

This step is critical. Without this, the port is blocked and app won’t be accessible.

---

**Section 7: Summary**

We’ve now seen how to:

* Build a Django web app
* Understand its structure
* Write a Dockerfile for it
* Run it inside a Docker container

Tomorrow we’ll cover:

* Docker networking
* Docker CLI
* Reducing image size using **multi-stage builds**

💡 **Quiz for You**: How many EC2 instances can you run under AWS Free Tier? Drop your answers in the comments.

👍 If this was helpful, hit like and subscribe. Comment below if you faced any issues.

See you in the next class 👋
