# 🚀 Day 64 – Debugging a Python Flask Application on Kubernetes

## 🎯 Objective

The Nautilus DevOps team deployed a Python Flask application on Kubernetes, but the application was not accessible externally.

### Requirements:

* Deployment: `python-deployment-devops`
* Image: `poroko/flask-demo-app`
* Fix accessibility issues
* NodePort should be `32345`
* targetPort should match Flask default port

---

# 🧠 Why This Task Matters

This task simulated a **real-world production troubleshooting scenario** involving:

* Kubernetes Services
* Port mappings
* Deployment troubleshooting
* Image pull failures
* Networking validation

---

# 🧱 Architecture Overview

```text
User
  ↓
NodePort Service (32345)
  ↓
Kubernetes Service
  ↓
Flask Pod
  ↓
Flask Application (Port 5000)
```

---

# 🛠️ Debugging & Investigation

---

## 🔹 Step 1: Check Existing Resources

```bash
kubectl get deployments
kubectl get pods
kubectl get svc
```

---

## 🔹 Step 2: Verify Service Configuration

```bash
kubectl describe svc python-service-devops
```

---

# 🧪 Issue 1 – Incorrect targetPort

The Flask application uses default port:

```text
5000
```

But the Service was mapped incorrectly.

---

## ❌ Incorrect Service Configuration

```yaml
targetPort: wrong-port
```

---

## ✅ Fixed Service Configuration

```yaml
ports:
- nodePort: 32345
  port: 5000
  protocol: TCP
  targetPort: 5000

selector:
  app: python_app

type: NodePort
```

---

## 🔹 Step 3: Apply Service Changes

```bash
kubectl edit svc python-service-devops
```

---

# 🧪 Issue 2 – Application Still Not Accessible

Even after fixing the Service, the application was still inaccessible.

---

## 🔍 Investigation

Checked pods:

```bash
kubectl get pods -w
```

Observed:

```text
ImagePullBackOff
```

---

# ❗ Root Cause Found

Pods were failing because Kubernetes could not pull the image.

---

## 🔹 Step 4: Describe Pod

```bash
kubectl describe pod python-deployment-devops-7dd8f6ddf8-bpcm2
```

---

## 🔍 Events Observed

```text
Failed to pull image
ImagePullBackOff
```

---

# 🧪 Issue 3 – Image Misconfiguration

The deployment image configuration was incorrect/missing proper tag.

---

## 🔹 Step 5: Verify Deployment Image

```bash
kubectl get deployment python-deployment-devops -o yaml | grep image
```

---

## 🔹 Step 6: Fix Deployment Image

```bash
kubectl edit deployment python-deployment-devops
```

Updated image:

```yaml
image: poroko/flask-demo-app:latest
```

---

## 🔄 Step 7: Restart Deployment

```bash
kubectl rollout restart deployment python-deployment-devops
```

---

## 🔹 Step 8: Verify Pods

```bash
kubectl get pods -w
```

Expected:

```text
STATUS = Running
READY = 1/1
```

---

## 🔹 Step 9: Verify Service Endpoints

```bash
kubectl get endpoints
```

Expected:

```text
python-service-devops → pod-ip:5000
```

---

# 🌐 Access Application

```text
http://<node-ip>:32345
```

✔ Flask application accessible successfully

---

# 🔍 Full Debugging Flow

```text
Application Not Accessible
        ↓
Checked Service
        ↓
Found Wrong targetPort
        ↓
Fixed Service Port Mapping
        ↓
Application Still Failed
        ↓
Checked Pods
        ↓
ImagePullBackOff Found
        ↓
Verified Deployment Image
        ↓
Fixed Image Configuration
        ↓
Restarted Deployment
        ↓
Application Working
```

---

# 📌 Key Concepts Covered

* Kubernetes Services
* NodePort exposure
* targetPort mapping
* Pod troubleshooting
* ImagePullBackOff debugging
* Deployment reconciliation

---

# 🧠 Key Learnings

* Applications can fail due to:

  * Networking issues
  * Service misconfiguration
  * Image issues

* Fixing one issue may expose another hidden issue

* `kubectl describe pod` is one of the most important debugging commands

---

# ❌ Common Mistakes Avoided

* ❌ Debugging only Service
* ❌ Ignoring pod status
* ❌ Blindly restarting without checking events
* ❌ Assuming application issue

---

# 🛠️ Commands Used

```bash
kubectl get deployments
kubectl get pods
kubectl get svc

kubectl describe svc python-service-devops

kubectl edit svc python-service-devops

kubectl get pods -w

kubectl describe pod <pod-name>

kubectl get deployment python-deployment-devops -o yaml | grep image

kubectl edit deployment python-deployment-devops

kubectl rollout restart deployment python-deployment-devops

kubectl get endpoints
```

---

# ✅ Final Status

✔ Service corrected
✔ targetPort fixed
✔ Image issue resolved
✔ Deployment restarted
✔ Pods running successfully
✔ Application accessible

---
