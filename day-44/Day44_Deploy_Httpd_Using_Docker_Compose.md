# 🚀 Day 44 – Deploy HTTPD Container Using Docker Compose

## 🎯 Objective

The Nautilus DevOps team needs to deploy an **httpd container** using Docker Compose with specific configurations.

### Requirements:

* Create Docker Compose file: `/opt/docker/docker-compose.yml`
* Container name: `httpd`
* Image: `httpd:latest`
* Port mapping: `5003 (host) → 80 (container)`
* Volume mapping:
  `/opt/finance (host)` → `/usr/local/apache2/htdocs (container)`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **Docker Compose for multi-service management**
* Demonstrates **volume mounting for persistent data**
* Enables **static website hosting**
* Common in real-world deployments

---

## 🧱 Environment Details

* **Server**: App Server 2 (`stapp02`)
* **User**: `tony`
* **Compose File**: `/opt/docker/docker-compose.yml`
* **Container Name**: `httpd`
* **Image**: Apache HTTP Server (`httpd:latest`)
* **Port Mapping**: `5003:80`
* **Volume Mapping**: `/opt/finance → /usr/local/apache2/htdocs`

---

## 🛠️ Commands Used

* `mkdir`
* `vi` / `tee`
* `docker-compose up`
* `docker ps`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 2

```bash
ssh tony@stapp02
```

---

### 🔹 Step 2: Create Directory

```bash
sudo mkdir -p /opt/docker
```

---

### 🔹 Step 3: Create Docker Compose File

```bash
sudo vi /opt/docker/docker-compose.yml
```

---

### 🔹 Step 4: Add Configuration

```yaml
version: '3'
services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "5003:80"
    volumes:
      - /opt/finance:/usr/local/apache2/htdocs
```

---

## 🧪 Deploy the Container

```bash
cd /opt/docker
docker-compose up -d
```

---

## 🧪 Verification

### 🔹 Check Running Container

```bash
docker ps
```

Expected:

* Container name: `httpd`
* Port mapping: `0.0.0.0:5003->80/tcp`

---

### 🔹 Test Application

```bash
curl http://localhost:5003
```

Expected:

* Static website content from `/opt/finance`

---

## 📌 Key Notes

* `container_name` ensures exact naming requirement
* Volume mapping allows **host files to serve as web content**
* No modification required in `/opt/finance`

---

## 📌 Real-World Use Cases

* Hosting static websites
* Serving frontend applications
* Mounting shared storage
* Content delivery systems

---

## 🧠 Key Learnings

* Writing Docker Compose files
* Volume mapping for persistent data
* Port exposure in Compose
* Managing containers using Compose

---

## ❌ What Was Avoided

* ❌ Changing existing data in `/opt/finance`
* ❌ Incorrect container naming
* ❌ Wrong port mapping
* ❌ Running container without Compose

---

## ✅ Final Status

✔ Docker Compose file created
✔ httpd container deployed
✔ Port 5003 mapped successfully
✔ Volume mounted correctly
✔ Static site accessible

---
