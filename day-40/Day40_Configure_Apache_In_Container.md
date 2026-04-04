# 🚀 Day 40 – Install and Configure Apache Inside Docker Container

## 🎯 Objective

The Nautilus DevOps team needs to complete pending configuration inside a running container.

### Requirements:

* Install **apache2** in container `kkloud`
* Configure Apache to listen on **port 5003**
* Ensure it listens on **all interfaces** (not restricted to IP)
* Start Apache service
* Keep container running

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates service configuration inside containers
* Shows how to modify default application ports
* Helps in running multiple services without port conflicts
* Common in microservices and containerized environments

---

## 🧱 Environment Details

* **Server**: App Server 1 (`stapp01`)
* **User**: `tony`
* **Container**: `kkloud`
* **Service**: Apache HTTP Server (`apache2`)
* **Port**: `5003`

---

## 🛠️ Commands Used

* `docker exec`
* `apt`
* `sed`
* `service`
* `netstat` / `ss`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 1

```bash
ssh tony@stapp01
```

---

### 🔹 Step 2: Access the Container

```bash
docker exec -it kkloud bash
```

---

### 🔹 Step 3: Update Packages

```bash
apt update
```

---

### 🔹 Step 4: Install Apache

```bash
apt install -y apache2
```

---

### 🔹 Step 5: Change Apache Port (Without Editor)

```bash
sed -i 's/Listen 80/Listen 5003/' /etc/apache2/ports.conf
```

```bash
sed -i 's/<VirtualHost \*:80>/<VirtualHost *:5003>/' /etc/apache2/sites-available/000-default.conf
```

---

### 🔹 Step 6: Start Apache Service

```bash
service apache2 start
```

---

## 🧪 Verification

### 🔹 Check Apache Status

```bash
service apache2 status
```

---

### 🔹 Check Listening Port

#### Option 1: Using netstat

```bash
apt install -y net-tools
netstat -tulnp | grep 5003
```

---

#### Option 2: Using ss (Modern Method)

```bash
ss -tulnp | grep 5003
```

---

## 🧯 Troubleshooting (Real Issue Faced)

### ❌ vim / vi not available

Error:

```text
vim: command not found
```

#### ✅ Fix:

```bash
apt install -y vim
```

OR use:

```bash
sed (recommended in labs)
```

---

### ❌ netstat not found

Error:

```text
netstat: command not found
Unable to locate package netstat
```

#### ✅ Fix:

```bash
apt install -y net-tools
```

---

## 📌 Key Notes

* Apache must listen on `0.0.0.0` (all interfaces)
* Both config files must be updated
* Minimal containers don’t include tools like `vim` or `netstat`

---

## 📌 Real-World vs Lab Difference

| Scenario       | Approach                    |
| -------------- | --------------------------- |
| Real-world     | Use Dockerfile / automation |
| Minimal images | No editors/tools installed  |
| Labs           | Install tools or use `sed`  |

---

## 📌 Real-World Use Cases

* Running multiple web servers on different ports
* Microservices architecture
* Containerized web hosting
* Internal application routing

---

## 🧠 Key Learnings

* Installing and configuring services inside containers
* Editing configs without editors using `sed`
* Troubleshooting missing tools
* Using `ss` as modern alternative to netstat
* Understanding minimal container design

---

## ❌ What Was Avoided

* ❌ Binding Apache to specific IP
* ❌ Using unavailable tools blindly
* ❌ Changing only one config file
* ❌ Skipping verification

---

## ✅ Final Status

✔ Apache installed successfully
✔ Port changed to `5003`
✔ Apache running inside container
✔ Verified using `netstat/ss`
✔ Container remains running

---
