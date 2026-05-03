# 🚀 Day 59 – Fix Broken Redis Deployment in Kubernetes

## 🎯 Objective

The Nautilus DevOps team reported that an existing Redis deployment stopped working after recent changes.

### Requirements:

* Deployment: `redis-deployment`
* Pods not running
* Identify root cause
* Fix deployment

---

## 🧠 Why This Task Matters

* Real-world **production debugging scenario**
* Multiple issues combined → common in real incidents
* Builds **root cause analysis mindset**
* Strengthens Kubernetes troubleshooting skills

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster
* **Deployment**: `redis-deployment`

---

## 🛠️ Commands Used (Actual Debug Flow)

```bash
# Check pods
kubectl get pods

# Describe pod (MOST IMPORTANT)
kubectl describe pod <pod-name>

# Check deployment YAML
kubectl get deployment redis-deployment -o yaml

# Edit deployment
kubectl edit deployment redis-deployment

# Restart rollout
kubectl rollout restart deployment redis-deployment

# Verify
kubectl get pods
```

---

## 🧪 Issues Observed

### ❌ Issue 1: ConfigMap Not Found

```text
MountVolume.SetUp failed for volume "config":
configmap "redis-conig" not found
```

👉 Root cause:

* Typo in ConfigMap name

```text
redis-conig ❌
redis-config ✅
```

---

### ❌ Issue 2: Image Pull Failure

```text
Failed to pull image "redis:alpin"
```

👉 Root cause:

* Incorrect image tag

```text
redis:alpin ❌
redis:alpine ✅
```

---

## 🔍 Investigation Summary

| Problem         | Impact              |
| --------------- | ------------------- |
| ConfigMap typo  | Volume mount failed |
| Wrong image tag | Image pull failed   |
| Combined effect | Pods not starting   |

---

## ✅ Fix Applied

### 🔹 Step 1: Edit Deployment

```bash
kubectl edit deployment redis-deployment
```

---

### 🔹 Step 2: Fix ConfigMap

```yaml
name: redis-config
```

---

### 🔹 Step 3: Fix Image

```yaml
image: redis:alpine
```

---

### 🔹 Step 4: Restart Deployment

```bash
kubectl rollout restart deployment redis-deployment
```

---

## 🧪 Verification

```bash
kubectl get pods
```

✔ Pods in Running state
✔ No errors in events

---

## 📌 Key Notes

* Use `kubectl describe pod` for root cause
* Logs won’t work if container hasn’t started
* Small typos can break entire deployments

---

## 🧠 Key Learnings

* Debugging > Deployment
* Always check **events first**
* Validate configs before applying
* Multiple small issues can combine into one failure

---

## ❌ What Was Avoided

* ❌ Recreating deployment blindly
* ❌ Guessing without checking events
* ❌ Ignoring root cause

---

## ✅ Final Status

✔ ConfigMap issue fixed
✔ Image issue fixed
✔ Deployment restored
✔ Pods running successfully

---
