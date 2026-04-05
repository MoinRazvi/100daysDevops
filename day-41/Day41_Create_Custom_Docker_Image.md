# 🚀 Day 41 – Create Custom Docker Image using Dockerfile

## 🎯 Objective

The Nautilus DevOps team needs to create a **custom Docker image** using a Dockerfile with the following requirements:

* Base image: `ubuntu:24.04`
* Install **apache2**
* Configure Apache to run on **port 3000**
* Dockerfile location: `/opt/docker/Dockerfile`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **Dockerfile-based image creation**
* Helps automate container setup
* Ensures consistent and repeatable environments
* Foundation for CI/CD pipelines

---

## 🧱 Environment Details

* **Server**: App Server 3 (`stapp03`)
* **User**: `tony`
* **Dockerfile Path**: `/opt/docker/Dockerfile`
* **Base Image**: Ubuntu (`24.04`)
* **Service**: Apache HTTP Server
* **Port**: `3000`

---

## 🛠️ Commands Used

* `mkdir`
* `vi` / `cat`
* `docker build`
* `docker run`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 3

```bash
ssh tony@stapp03
```

---

### 🔹 Step 2: Create Directory

```bash
sudo mkdir -p /opt/docker
```

---

### 🔹 Step 3: Create Dockerfile

```bash
sudo vi /opt/docker/Dockerfile
```

---

### 🔹 Step 4: Add Dockerfile Content

```dockerfile
FROM ubuntu:24.04

RUN apt update && \
    apt install -y apache2 && \
    sed -i 's/Listen 80/Listen 3000/' /etc/apache2/ports.conf && \
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:3000>/' /etc/apache2/sites-available/000-default.conf

EXPOSE 3000

CMD ["apachectl", "-D", "FOREGROUND"]
```

---

## 🧪 Build the Image

```bash
cd /opt/docker
docker build -t apache:3000 .
```

---

## 🧪 Run the Container (Optional Verification)

```bash
docker run -d -p 3000:3000 apache:3000
```

---

## 🧪 Verification

### 🔹 Check Running Container

```bash
docker ps
```

---

### 🔹 Verify Apache Port

```bash
ss -tulnp | grep 3000
```

---

## 📌 Key Notes

* `EXPOSE 3000` documents the port (does not publish it)
* `CMD` keeps Apache running in foreground
* Used `sed` to modify configs (best practice for automation)

---

## 📌 Real-World Use Cases

* Building custom application images
* Automating infrastructure setup
* CI/CD pipelines
* Microservices deployments

---

## 🧠 Key Learnings

* Writing Dockerfiles from scratch
* Automating package installation
* Modifying configs during build
* Running services in foreground in containers

---

## ❌ What Was Avoided

* ❌ Manual configuration inside container
* ❌ Using editors instead of automation
* ❌ Forgetting foreground process
* ❌ Hardcoding unnecessary configs

---

## ✅ Final Status

✔ Dockerfile created at `/opt/docker/Dockerfile`
✔ Base image set to Ubuntu 24.04
✔ Apache installed and configured on port 3000
✔ Image successfully buildable

---
