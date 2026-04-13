# 🚀 Day 46 – Deploy Multi-Service Application using Docker Compose

## 🎯 Objective

The Nautilus DevOps team needs to deploy a **multi-container application stack** using Docker Compose.

### Requirements:

* Create compose file: `/opt/data/docker-compose.yml`
* Deploy two services:

  * **Web (PHP Apache)**
  * **Database (MariaDB)**
* Configure ports, volumes, and environment variables correctly

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates **multi-container orchestration**
* Shows **application + database integration**
* Introduces **real-world stack deployment**
* Core concept for microservices and DevOps pipelines

---

## 🧱 Environment Details

* **Server**: App Server 3 (`stapp03`)
* **User**: `tony`
* **Compose File**: `/opt/data/docker-compose.yml`

### Services:

#### 🔹 Web Service

* **Container Name**: `php_blog`
* **Image**: PHP (`php:apache`)
* **Port Mapping**: `6000:80`
* **Volume**: `/var/www/html → /var/www/html`

#### 🔹 DB Service

* **Container Name**: `mysql_blog`
* **Image**: MariaDB (`latest`)
* **Port Mapping**: `3306:3306`
* **Volume**: `/var/lib/mysql → /var/lib/mysql`
* **Database**: `database_blog`

---

## 🛠️ Commands Used

* `mkdir`
* `vi` / `tee`
* `docker-compose up`
* `docker ps`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 3

```bash
ssh tony@stapp03
```

---

### 🔹 Step 2: Create Directory

```bash
sudo mkdir -p /opt/data
```

---

### 🔹 Step 3: Create Docker Compose File

```bash
sudo vi /opt/data/docker-compose.yml
```

---

### 🔹 Step 4: Add Configuration

```yaml
version: '3'
services:

  web:
    image: php:apache
    container_name: php_blog
    ports:
      - "6000:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    image: mariadb:latest
    container_name: mysql_blog
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_blog
      MYSQL_USER: blog_user
      MYSQL_PASSWORD: Str0ngP@ss123
      MYSQL_ROOT_PASSWORD: RootP@ss123
```

---

## 🧪 Deploy the Stack

```bash
cd /opt/data
docker-compose up -d
```

---

## 🧪 Verification

### 🔹 Check Containers

```bash
docker ps
```

Expected:

* `php_blog` → running
* `mysql_blog` → running

---

### 🔹 Test Application

```bash
curl http://localhost:6000
```

Expected:

* PHP Apache default page or app content

---

## 📌 Key Notes

* `container_name` ensures exact naming
* Volumes provide **data persistence**
* Environment variables configure DB automatically
* Both services run in same Docker network by default

---

## 📌 Real-World Use Cases

* WordPress / PHP applications
* Web + DB architecture
* Microservices deployments
* Dev/test environments

---

## 🧠 Key Learnings

* Multi-service deployment using Docker Compose
* Linking web and database containers
* Volume mapping for persistent storage
* Environment variable configuration

---

## ❌ What Was Avoided

* ❌ Using root user for DB connections
* ❌ Missing volume mappings
* ❌ Incorrect port configuration
* ❌ Skipping environment variables

---

## ✅ Final Status

✔ Docker Compose file created
✔ Web and DB containers deployed
✔ Ports mapped correctly
✔ Volumes configured
✔ Application accessible on port 6000

---
