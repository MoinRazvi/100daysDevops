# 🚀 Day 53 – Fix Nginx + PHP-FPM Path Mismatch Issue in Kubernetes

## 🎯 Objective

The Nautilus DevOps team encountered an issue where a **Nginx + PHP-FPM application** was returning a **502 Bad Gateway** error.

### Requirements:

* Pod: `nginx-phpfpm`
* ConfigMap: `nginx-config`
* Identify and fix the issue
* Copy `index.php` into container
* Ensure application is accessible

---

## 🧠 Why This Task Matters

This task is important because:

* Represents a **real-world debugging scenario**
* Involves **multi-container pod troubleshooting**
* Requires understanding of **volumes + config + request flow**
* Builds strong **production-level troubleshooting skills**

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Pod**: `nginx-phpfpm`
* **Containers**:

  * Nginx
  * PHP-FPM
* **ConfigMap**: `nginx-config`

---

## 🛠️ Commands Used (Actual Debug Flow)

```bash
# Export existing pod configuration
kubectl get pod nginx-phpfpm -o yaml > nginx-phpfpm.yml

# Edit pod configuration
vi nginx-phpfpm.yml

# Verify ConfigMap configuration
kubectl get configmap nginx-config -o yaml

# Delete existing pod
kubectl delete pod nginx-phpfpm

# Recreate pod with updated configuration
kubectl apply -f nginx-phpfpm.yml

# Copy application file into container
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

---

## 🧪 Issue Observed

* Application returning:

```text
502 Bad Gateway
```

* Pod and containers were:

  * Running ✅
  * Healthy ✅
* Logs:

  * No clear errors ❌

---

## 🔍 Investigation Steps

### 🔹 Step 1: Checked Pod & Configuration

```bash
kubectl get pod nginx-phpfpm -o yaml
```

👉 Verified:

* Volume mounts
* Container paths

---

### 🔹 Step 2: Verified ConfigMap

```bash
kubectl get configmap nginx-config -o yaml
```

👉 Checked:

* Nginx root path
* PHP configuration

---

### 🔹 Step 3: Identified Mismatch

Found mismatch between:

* Nginx container path
* ConfigMap configuration path

---

## ❗ Root Cause

* Volume mounted at:

```text
/var/www/html
```

* But Nginx container configuration was pointing to a **different path**

👉 Result:

* Nginx could not locate PHP files
* PHP-FPM communication failed
* Application returned **502 Bad Gateway**

---

## ✅ Fix Applied

### 🔹 Updated Pod Configuration

Edited:

```bash
vi nginx-phpfpm.yml
```

✔ Updated Nginx container paths to:

```text
/var/www/html
```

---

### 🔹 Recreated Pod

```bash
kubectl delete pod nginx-phpfpm
kubectl apply -f nginx-phpfpm.yml
```

---

### 🔹 Copied Application File

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

---

## 🧪 Verification

```bash
kubectl get pods
```

✔ Pod running

Access application:

✔ Website accessible
✔ PHP file executed successfully
✔ 502 error resolved

---

## 📌 Key Notes

* Volume mount path must match application config
* Nginx root must align with mounted directory
* Pod recreation required after configuration changes
* Logs may not always indicate configuration mismatch

---

## 📌 Real-World Use Cases

* Debugging web + backend integration
* Fixing reverse proxy issues
* Kubernetes ConfigMap troubleshooting
* Multi-container architecture debugging

---

## 🧠 Key Learnings

* Always validate:

  * Volume mounts
  * Application paths
  * Config consistency

* Debugging flow should be:

  * Pod → Logs → Config → Paths

* 502 errors often indicate:

  * Backend communication failure
  * Misconfigured paths

---

## ❌ What Was Avoided

* ❌ Rebuilding images unnecessarily
* ❌ Changing application code
* ❌ Blind troubleshooting

---

## ✅ Final Status

✔ Root cause identified (path mismatch)
✔ Configuration corrected
✔ Pod recreated successfully
✔ Application working
✔ 502 error resolved

---
