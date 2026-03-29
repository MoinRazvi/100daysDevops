# 🚀 Day 36 – Deploy Nginx Container on App Server 2

## 🎯 Objective

The Nautilus DevOps team requires deploying an **Nginx container** on Application Server 2 with the following requirements:

* Create a container named **`nginx_2`**
* Use **nginx image with alpine tag**
* Ensure the container is **running**

---

## 🧠 Why This Task Matters

Container deployment is a core DevOps skill because:

* Enables quick application setup
* Ensures lightweight and fast deployments (Alpine images)
* Standardizes environments across servers
* Helps in testing deployment pipelines

---

## 🧱 Environment Details

* **Server**: App Server 2 (`stapp02`)
* **User**: `tony`
* **Container Name**: `nginx_2`
* **Image**: Nginx (`nginx:alpine`)

---

## 🛠️ Commands Used

* `docker`
* `docker ps`
* `docker images`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 2

```bash
ssh tony@stapp02
```

---

### 🔹 Step 2: Pull Nginx Alpine Image (if not present)

```bash
docker pull nginx:alpine
```

---

### 🔹 Step 3: Run the Container

```bash
docker run -d --name nginx_2 nginx:alpine
```

---

## 🧪 Verification

### 🔹 Check Running Containers

```bash
docker ps
```

Expected output should include:

* Container name: `nginx_2`
* Status: `Up`

---

### 🔹 Optional: Check Logs

```bash
docker logs nginx_2
```

---

## 📌 Key Notes

* `-d` → Runs container in **detached mode**
* `--name` → Assigns custom container name
* `nginx:alpine` → Lightweight version of Nginx

---

## 📌 Real-World Use Cases

* Hosting web applications
* Reverse proxy setups
* Load balancing
* Static content serving

---

## 🧠 Key Learnings

* How to run containers using specific image tags
* Importance of lightweight images like Alpine
* Verifying container status using `docker ps`
* Naming containers for easier management

---

## ❌ What Was Avoided

* ❌ Running container without name
* ❌ Using heavy base images unnecessarily
* ❌ Forgetting detached mode
* ❌ Not verifying container status

---

## ✅ Final Status

✔ Nginx Alpine image used
✔ Container `nginx_2` created
✔ Container is running successfully

---
