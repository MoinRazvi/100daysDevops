# 🚀 Day 39 – Create Docker Image from Running Container

## 🎯 Objective

The Nautilus DevOps team needs to create a **new Docker image** from an existing running container.

### Requirements:

* Source container: `ubuntu_latest`
* Create image: `blog:devops`
* Perform operation on App Server 1

---

## 🧠 Why This Task Matters

Creating images from containers is useful because:

* Preserves current state of a container
* Helps in backup and recovery
* Enables sharing of custom configurations
* Useful for debugging and testing environments

---

## 🧱 Environment Details

* **Server**: App Server 1 (`stapp01`)
* **User**: `tony`
* **Container**: `ubuntu_latest`
* **New Image**: `blog:devops`
* **Base OS**: Ubuntu (inside container)

---

## 🛠️ Commands Used

* `docker commit`
* `docker images`
* `docker ps`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 1

```bash
ssh tony@stapp01
```

---

### 🔹 Step 2: Verify Container is Running

```bash
docker ps
```

Ensure `ubuntu_latest` is listed and running.

---

### 🔹 Step 3: Create Image from Container

```bash
docker commit ubuntu_latest blog:devops
```

---

## 🧪 Verification

### 🔹 Check Created Image

```bash
docker images
```

Expected output should include:

* Repository: `blog`
* Tag: `devops`

---

## 📌 Key Notes

* `docker commit` captures the **current state** of the container
* Includes filesystem changes and configurations
* Does not include volumes data

---

## 📌 Real-World Use Cases

* Creating backup snapshots of containers
* Saving debugging states
* Building custom base images
* Quick prototyping before writing Dockerfile

---

## 🧠 Key Learnings

* How to create images from running containers
* Difference between Dockerfile vs commit
* Importance of verifying container state
* Image tagging conventions

---

## ❌ What Was Avoided

* ❌ Creating image from stopped container
* ❌ Incorrect naming format
* ❌ Skipping verification step
* ❌ Assuming volumes are included

---

## ✅ Final Status

✔ Container `ubuntu_latest` verified running
✔ Image `blog:devops` created successfully
✔ Image available in local repository
✔ Backup of container state completed

---
