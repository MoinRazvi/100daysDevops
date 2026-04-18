# 🚀 Day 49 – Create Kubernetes Deployment (Nginx)

## 🎯 Objective

The Nautilus DevOps team needs to create a **Kubernetes Deployment** to manage an application.

### Requirements:

* Deployment name: `nginx`
* Image: Nginx (`nginx:latest`)

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **Deployments in Kubernetes**
* Enables **scaling and self-healing**
* Manages Pods automatically
* Core concept for production workloads

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Tool**: `kubectl`
* **Deployment Name**: `nginx`

---

## 🛠️ Commands Used

* `kubectl create deployment`
* `kubectl get deployments`
* `kubectl get pods`
* `kubectl describe deployment`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Create Deployment

```bash
kubectl create deployment nginx --image=nginx:latest
```

---

## 🧪 Verification

### 🔹 Check Deployment

```bash
kubectl get deployments
```

Expected:

* `nginx` → Available

---

### 🔹 Check Pods

```bash
kubectl get pods
```

Expected:

* Pod created by deployment → Running

---

### 🔹 Describe Deployment

```bash
kubectl describe deployment nginx
```

Verify:

* Image: `nginx:latest`
* Replicas: 1 (default)

---

## 📌 Key Notes

* Deployment manages Pods automatically
* If Pod fails → Kubernetes recreates it
* Default replicas = 1

---

## 📌 Alternative (YAML – Recommended)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

---

## 📌 Real-World Use Cases

* Running scalable web applications
* Zero-downtime deployments
* Rolling updates
* High availability setups

---

## 🧠 Key Learnings

* Difference between Pod vs Deployment
* Self-healing behavior of Kubernetes
* Managing applications declaratively
* Basic scaling foundation

---

## ❌ What Was Avoided

* ❌ Missing image tag (`latest`)
* ❌ Creating standalone Pods instead of Deployment
* ❌ Skipping verification
* ❌ Incorrect deployment name

---

This is a **key Kubernetes milestone 🚀**

---
