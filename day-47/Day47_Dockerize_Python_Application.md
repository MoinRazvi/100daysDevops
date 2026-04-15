# 🚀 Day 47 – Dockerize and Deploy Python Application

## 🎯 Objective

The Nautilus DevOps team needs to **Dockerize a Python application** and deploy it on App Server 2.

### Requirements:

* Create Dockerfile under `/python_app`
* Use Python base image
* Install dependencies from `requirements.txt`
* Expose port `3002`
* Run `server.py`
* Build image: `nautilus/python-app`
* Run container: `pythonapp_nautilus`
* Map port: `8099 (host) → 3002 (container)`

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates **application containerization**
* Shows dependency management using `requirements.txt`
* Core skill for deploying backend services
* Foundation for microservices architecture

---

## 🧱 Environment Details

* **Server**: App Server 2 (`stapp02`)
* **User**: `tony`
* **App Directory**: `/python_app`
* **Dependencies**: `/python_app/src/requirements.txt`
* **App File**: `server.py`
* **Language**: Python

---

## 🛠️ Commands Used

* `vi` / `tee`
* `docker build`
* `docker run`
* `docker ps`
* `curl`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 2

```bash
ssh tony@stapp02
```

---

### 🔹 Step 2: Create Dockerfile

```bash
vi /python_app/Dockerfile
```

---

### 🔹 Step 3: Add Dockerfile Content

```dockerfile
FROM python:3.9

WORKDIR /app

COPY src/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY src/ .

EXPOSE 3002

CMD ["python", "server.py"]
```

---

## 🧪 Build the Image

```bash
cd /python_app
docker build -t nautilus/python-app .
```

---

## 🧪 Run the Container

```bash
docker run -d --name pythonapp_nautilus -p 8099:3002 nautilus/python-app
```

---

## 🧪 Verification

### 🔹 Check Running Container

```bash
docker ps
```

Expected:

* Container name: `pythonapp_nautilus`
* Port mapping: `8099->3002`

---

### 🔹 Test Application

```bash
curl http://localhost:8099
```

Expected:

* Python app response

---

## 📌 Key Notes

* `WORKDIR` sets working directory inside container
* `COPY` ensures app files are included
* `--no-cache-dir` reduces image size
* Port must match app’s listening port

---

## 📌 Real-World Use Cases

* Backend API deployment
* Microservices architecture
* Containerized Python apps (Flask, Django)
* CI/CD pipelines

---

## 🧠 Key Learnings

* Writing Dockerfile for Python apps
* Dependency management with `pip`
* Containerizing application code
* Port mapping and service exposure

---

## ❌ What Was Avoided

* ❌ Missing dependencies installation
* ❌ Incorrect working directory
* ❌ Wrong port exposure
* ❌ Running container without mapping ports

---

## ✅ Final Status

✔ Dockerfile created successfully
✔ Image `nautilus/python-app` built
✔ Container `pythonapp_nautilus` deployed
✔ Port mapping configured correctly
✔ Application accessible via port 8099

---
