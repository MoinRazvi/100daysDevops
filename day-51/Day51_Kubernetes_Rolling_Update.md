# 🚀 Day 51 – Perform Rolling Update in Kubernetes Deployment

## 🎯 Objective

The Nautilus DevOps team needs to perform a **rolling update** on an existing deployment.

### Requirements:

* Deployment name: `nginx-deployment`
* Update image to: Nginx (`nginx:1.17`)
* Ensure all pods are **running and updated successfully**

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **zero-downtime deployments**
* Allows safe application updates
* Core concept for production-grade systems
* Enables rollback if something goes wrong

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Tool**: `kubectl`
* **Deployment**: `nginx-deployment`

---

## 🛠️ Commands Used

* `kubectl set image`
* `kubectl rollout status`
* `kubectl get pods`
* `kubectl describe deployment`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Update Deployment Image

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.17
```

> ⚠️ `nginx` = container name inside the deployment

---

### 🔹 Step 2: Monitor Rolling Update

```bash
kubectl rollout status deployment/nginx-deployment
```

Expected:

```text
deployment "nginx-deployment" successfully rolled out
```

---

## 🧪 Verification

### 🔹 Check Pods

```bash
kubectl get pods
```

* Old pods terminate gradually
* New pods come up with updated image

---

### 🔹 Verify Image Version

```bash
kubectl describe deployment nginx-deployment
```

Check:

* Image: `nginx:1.17`

---

## 📌 Key Notes

* Rolling update replaces pods **one by one**
* No downtime if configured properly
* Old pods are terminated only after new ones are ready

---

## 📌 Real-World Use Cases

* Application version upgrades
* Security patch deployment
* Feature rollouts
* Blue-green / rolling strategies

---

## 🧠 Key Learnings

* How to update running applications safely
* Using `kubectl set image`
* Monitoring rollout status
* Ensuring application availability

---

## ❌ What Was Avoided

* ❌ Deleting deployment and recreating
* ❌ Causing downtime
* ❌ Not verifying rollout
* ❌ Updating wrong container name

---

## ✅ Final Status

✔ Deployment updated to `nginx:1.17`
✔ Rolling update completed successfully
✔ All pods running with new image
✔ Application remains available

---
