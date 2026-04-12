# 🚀 Day 45 – Fix Dockerfile Build Issue

## 🎯 Objective

The Nautilus DevOps team encountered a failure while building a Docker image from an existing Dockerfile.

### Requirements:

* Fix the Dockerfile located at `/opt/docker/Dockerfile`
* Do NOT change:

  * Base image
  * Application data (e.g., `index.html`)
  * Core configuration logic
* Ensure Docker image builds successfully

---

## 🧠 Why This Task Matters

This task is important because:

* Debugging Dockerfiles is a **real-world DevOps skill**
* Helps understand **build context and filesystem behavior**
* Teaches how to fix **common Docker mistakes**
* Critical for CI/CD pipeline reliability

---

## 🧱 Environment Details

* **Server**: App Server 1 (`stapp01`)
* **User**: `tony`
* **Dockerfile Path**: `/opt/docker/Dockerfile`
* **Base Image**: Apache HTTP Server (`httpd:2.4.43`)

---

## 🛠️ Commands Used

* `docker build`
* `ls`
* `vi` / `sed`

---

## 🧪 Issues Faced

### ❌ Issue 1: Incorrect File Paths

```dockerfile
conf/httpd.conf
```

Error:

* File not found during build

---

### ❌ Issue 2: Wrong Command (`RUN cp`)

```dockerfile
RUN cp certs/server.crt ...
```

Error:

```text
cp: cannot stat 'certs/server.crt'
```

---

### ❌ Issue 3: Case Sensitivity Issue

```dockerfile
COPY Certs/server.crt ...
```

Error:

```text
"/Certs/server.crt": not found
```

---

## 🔍 Root Cause

* Docker build runs in an isolated environment
* Relative paths were incorrect
* `RUN cp` cannot access host files
* Linux is case-sensitive (`certs ≠ Certs`)

---

## ✅ Fix Applied

### 🔹 Fix 1: Use Absolute Paths

```dockerfile
/usr/local/apache2/conf/httpd.conf
```

---

### 🔹 Fix 2: Replace `RUN cp` with `COPY`

```dockerfile
COPY certs/server.crt /usr/local/apache2/conf/server.crt
```

---

### 🔹 Fix 3: Correct Directory Case

```dockerfile
certs/server.crt
```

---

## 🛠️ Final Dockerfile

```dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
COPY html/index.html /usr/local/apache2/htdocs/
```

---

## 🧪 Verification

### 🔹 Build Image

```bash
cd /opt/docker
docker build -t apache-fixed .
```

---

### 🔹 Expected Output

* Build completes successfully
* No file/path errors

---

## 📌 Key Notes

* `COPY` is used to transfer files from host → image
* Absolute paths are required for system configs
* Docker build context must contain required files

---

## 📌 Real-World Use Cases

* Debugging CI/CD pipeline failures
* Fixing broken Docker builds
* Maintaining production-ready images
* Handling secure file inclusion (certs, configs)

---

## 🧠 Key Learnings

* Difference between `RUN` vs `COPY`
* Importance of build context
* Linux case sensitivity
* Reading and debugging Docker errors

---

## ❌ What Was Avoided

* ❌ Changing base image
* ❌ Modifying application data
* ❌ Ignoring error logs
* ❌ Rewriting entire Dockerfile

---

## ✅ Final Status

✔ Dockerfile issues identified and fixed
✔ Image builds successfully
✔ No changes to base image or data
✔ Real-world debugging scenario completed

---
