# 🚀 Day 56 – Deploy Scalable Nginx Application with NodePort Service

## 🎯 Objective

The Nautilus DevOps team needs to deploy a **highly available and scalable web application** using Kubernetes.

### Requirements:

* Deployment name: `nginx-deployment`
* Image: Nginx (`nginx:latest`)
* Container name: `nginx-container`
* Replicas: `3`
* Service:

  * Name: `nginx-service`
  * Type: `NodePort`
  * NodePort: `30011`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **scaling using deployments**
* Demonstrates **service exposure using NodePort**
* Enables **high availability**
* Core concept for production workloads

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Deployment**: `nginx-deployment`
* **Service**: `nginx-service`

---

## 🛠️ Commands Used

* `kubectl apply`
* `kubectl get deployments`
* `kubectl get pods`
* `kubectl get svc`

---

## 🛠️ Implementation Steps

---

### 🔹 Step 1: Create Deployment YAML

```yaml id="4xg9c7"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```

---

### 🔹 Step 2: Apply Deployment

```bash id="5r1c3k"
kubectl apply -f deployment.yaml
```

---

## 🧪 Step 3 – Verify Deployment

```bash id="z2h8nq"
kubectl get deployments
kubectl get pods
```

Expected:

* 3 pods running

---

### 🔹 Step 4: Create Service YAML

```yaml id="8m7k2p"
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```

---

### 🔹 Step 5: Apply Service

```bash id="6p4t1j"
kubectl apply -f service.yaml
```

---

## 🧪 Step 6 – Verification

```bash id="3v9q2n"
kubectl get svc
```

Expected:

* NodePort → `30011`

---

### 🔹 Access Application

```bash id="y8r6n4"
http://<node-ip>:30011
```

✔ Application accessible

---

## 📌 Key Notes

* Deployment ensures:

  * High availability
  * Self-healing

* NodePort exposes service externally

* Selector must match pod labels

---

## 📌 Real-World Use Cases

* Public web applications
* Load-balanced services
* Scalable architectures
* Entry point for external traffic

---

## 🧠 Key Learnings

* Deployments manage replicas automatically
* Services expose applications to users
* NodePort allows external access
* Labels are critical for service discovery

---

## ❌ What Was Avoided

* ❌ Using incorrect image tag
* ❌ Missing replica configuration
* ❌ Label mismatch
* ❌ Incorrect service type

---

## ✅ Final Status

✔ Deployment created with 3 replicas
✔ Pods running successfully
✔ NodePort service created
✔ Application accessible externally

---
