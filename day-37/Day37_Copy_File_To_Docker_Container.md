# 🚀 Day 37 – Copy File from Host to Docker Container

## 🎯 Objective

The Nautilus DevOps team needs to securely transfer an encrypted file from the Docker host to a running container.

### Requirements:

* Copy file: `/tmp/nautilus.txt.gpg`
* Destination container: `ubuntu_latest`
* Target path inside container: `/home/`
* Ensure **file integrity (no modification)**

---

## 🧠 Why This Task Matters

This task is important because:

* Secure data transfer is critical in production
* Containers often require external configs or secrets
* Maintains integrity of encrypted files
* Common in backup, restore, and secret management workflows

---

## 🧱 Environment Details

* **Server**: App Server 3 (`stapp03`)
* **User**: `tony`
* **Container**: `ubuntu_latest`
* **Source File**: `/tmp/nautilus.txt.gpg`
* **Destination Path**: `/home/`

---

## 🛠️ Commands Used

* `docker cp`
* `docker ps`
* `ls`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 3

```bash
ssh tony@stapp03
```

---

### 🔹 Step 2: Verify Container is Running

```bash
docker ps
```

Ensure `ubuntu_latest` is listed.

---

### 🔹 Step 3: Copy File to Container

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```

---

## 🧪 Verification

### 🔹 Verify File Inside Container

```bash
docker exec ubuntu_latest ls /home/
```

Expected output:

```
nautilus.txt.gpg
```

---

## 📌 Key Notes

* `docker cp` preserves file content (no modification)
* Works for both directions (host ↔ container)
* No need to enter container manually

---

## 📌 Real-World Use Cases

* Copying SSL certificates into containers
* Transferring encrypted backups
* Injecting configuration files
* Debugging by copying logs

---

## 🧠 Key Learnings

* How to transfer files between host and container
* Importance of verifying container status before operations
* Secure handling of encrypted files
* Efficient use of `docker cp` instead of manual methods

---

## ❌ What Was Avoided

* ❌ Manually copying via SSH inside container
* ❌ Modifying encrypted file
* ❌ Using incorrect container path
* ❌ Copying into stopped container

---

## ✅ Final Status

✔ File copied successfully
✔ File integrity maintained
✔ Container verified and running
✔ File available at `/home/nautilus.txt.gpg` inside container

---
