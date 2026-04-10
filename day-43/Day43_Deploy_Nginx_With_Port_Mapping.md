# 🚀 Day 43 – Deploy Nginx Container with Port Mapping

## 🎯 Objective

The Nautilus DevOps team needs to deploy an Nginx-based container with port mapping.

### Requirements:

* Pull image: `nginx:alpine-perl`
* Create container: `cluster`
* Map port: `5002 (host) → 80 (container)`
* Ensure container is running

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates **port mapping between host and container**
* Enables external access to containerized applications
* Common in web hosting and microservices
* Essential for exposing services in Docker

---

## 🧱 Environment Details

* **Server**: App Server 2 (`stapp02`)
* **User**: `tony`
* **Container Name**: `cluster`
* **Image**: Nginx (`nginx:alpine-perl`)
* **Port Mapping**: `5002:80`

---

## 🛠️ Commands Used

* `docker pull`
* `docker run`
* `docker ps`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 2

```bash
ssh tony@stapp02
```

---

### 🔹 Step 2: Pull the Image

```bash
docker pull nginx:alpine-perl
```

---

### 🔹 Step 3: Run the Container with Port Mapping

```bash
docker run -d --name cluster -p 5002:80 nginx:alpine-perl
```

---

## 🧪 Verification

### 🔹 Check Running Container

```bash
docker ps
```

Expected:

* Container name: `cluster`
* Status: `Up`
* Port mapping: `0.0.0.0:5002->80/tcp`

---

### 🔹 Optional: Test Access

```bash
curl http://localhost:5002
```

Expected:

* Nginx default welcome page

---

## 📌 Key Notes

* `-p 5002:80` maps host port to container port
* Container must be running to access service
* `alpine-perl` is a lightweight variant with Perl support

---

## 📌 Real-World Use Cases

* Hosting web applications
* API exposure
* Reverse proxy setups
* Load balancing services

---

## 🧠 Key Learnings

* Port mapping in Docker
* Running containers with custom names
* Pulling specific image variants
* Verifying service accessibility

---

## ❌ What Was Avoided

* ❌ Forgetting port mapping
* ❌ Using wrong image tag
* ❌ Not verifying container status
* ❌ Running container in foreground

---

## ✅ Final Status

✔ Image `nginx:alpine-perl` pulled successfully
✔ Container `cluster` created
✔ Port 5002 mapped to container port 80
✔ Container running and accessible

---
