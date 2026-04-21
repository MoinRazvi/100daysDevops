# 🚀 Day 52 – Rollback Kubernetes Deployment

## 🎯 Objective

The Nautilus DevOps team needs to **rollback a deployment** to the previous stable version due to a bug in the latest release.

### Requirements:

* Deployment name: `nginx-deployment`
* Rollback to **previous revision**
* Ensure application is stable and pods are running

---

## 🧠 Why This Task Matters

This task is important because:

* Enables **quick recovery from failed releases**
* Critical for **production reliability**
* Minimizes downtime and user impact
* Core concept in CI/CD pipelines

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Tool**: `kubectl`
* **Deployment**: `nginx-deployment`

---

## 🛠️ Commands Used

* `kubectl rollout undo`
* `kubectl rollout history`
* `kubectl rollout status`
* `kubectl get pods`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Check Rollout History (Optional but Recommended)

```bash
kubectl rollout history deployment/nginx-deployment
```

👉 Shows available revisions

---

### 🔹 Step 2: Rollback to Previous Version

```bash
kubectl rollout undo deployment/nginx-deployment
```

---

### 🔹 Step 3: Verify Rollback Status

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

* Old stable pods should be running
* Buggy version pods should be replaced

---

### 🔹 Confirm Image Version

```bash
kubectl describe deployment nginx-deployment
```

Verify:

* Image reverted to previous version

---

## 📌 Key Notes

* `rollout undo` reverts to **last working revision**
* Kubernetes keeps deployment history
* No need to redeploy manually

---

## 📌 Real-World Use Cases

* Rolling back faulty releases
* Fixing production incidents
* Recovering from bad configurations
* Ensuring service continuity

---

## 🧠 Key Learnings

* How to rollback deployments safely
* Importance of revision history
* Zero-downtime recovery
* Production incident handling

---

## ❌ What Was Avoided

* ❌ Deleting deployment manually
* ❌ Rebuilding old image manually
* ❌ Causing downtime
* ❌ Ignoring rollout verification

---

## ✅ Final Status

✔ Deployment rolled back successfully
✔ Previous stable version restored
✔ Pods running without issues
✔ Application stabilized

---
