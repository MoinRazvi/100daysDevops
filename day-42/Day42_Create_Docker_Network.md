# 🚀 Day 42 – Create Custom Docker Network

## 🎯 Objective

The Nautilus DevOps team needs to create a custom Docker network with specific configurations.

### Requirements:

* Network name: `official`
* Driver: `bridge`
* Subnet: `172.168.0.0/24`
* IP Range: `172.168.0.0/24`

---

## 🧠 Why This Task Matters

Custom Docker networks are important because:

* Enable container-to-container communication
* Allow IP range control
* Help isolate environments
* Useful in microservices architecture

---

## 🧱 Environment Details

* **Server**: App Server 3 (`stapp03`)
* **User**: `tony`
* **Network Name**: `official`
* **Driver**: `bridge`
* **Subnet**: `172.168.0.0/24`

---

## 🛠️ Commands Used

* `docker network create`
* `docker network ls`
* `docker network inspect`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 3

```bash
ssh tony@stapp03
```

---

### 🔹 Step 2: Create Docker Network

```bash
docker network create \
  --driver bridge \
  --subnet 172.168.0.0/24 \
  --ip-range 172.168.0.0/24 \
  official
```

---

## 🧪 Verification

### 🔹 Check Network List

```bash
docker network ls
```

---

### 🔹 Inspect Network

```bash
docker network inspect official
```

Expected:

* Driver: `bridge`
* Subnet: `172.168.0.0/24`
* IP range configured correctly

---

## 📌 Key Notes

* `bridge` is default Docker network driver
* `--subnet` defines network range
* `--ip-range` limits container IP allocation range

---

## 📌 Real-World Use Cases

* Microservices communication
* Multi-container applications
* Network isolation between environments
* Service discovery setups

---

## 🧠 Key Learnings

* Creating custom Docker networks
* Understanding subnet vs IP range
* Network inspection for validation
* Importance of controlled networking

---

## ❌ What Was Avoided

* ❌ Using default network blindly
* ❌ Incorrect subnet configuration
* ❌ Skipping verification
* ❌ IP conflicts

---

## ✅ Final Status

✔ Docker network `official` created
✔ Bridge driver configured
✔ Subnet and IP range set correctly
✔ Network verified successfully

---
