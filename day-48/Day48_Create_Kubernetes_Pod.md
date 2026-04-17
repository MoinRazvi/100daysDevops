# 🚀 Day 48 – Create Kubernetes Pod (Nginx)

## 🎯 Objective

The Nautilus DevOps team needs to create a **Kubernetes Pod** with specific configurations.

### Requirements:

* Pod name: `pod-nginx`
* Image: Nginx (`nginx:latest`)
* Label: `app=nginx_app`
* Container name: `nginx-container`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **Kubernetes basics**
* Pods are the **smallest deployable units**
* Foundation for Deployments, Services, and scaling
* First step into container orchestration

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Tool**: `kubectl`
* **Pod Name**: `pod-nginx`
* **Container Name**: `nginx-container`

---

## 🛠️ Commands Used

* `kubectl run`
* `kubectl get pods`
* `kubectl describe pod`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Create the Pod

```bash
kubectl run pod-nginx \
  --image=nginx:latest \
  --labels=app=nginx_app \
  --restart=Never \
  --overrides='{
    "spec": {
      "containers": [
        {
          "name": "nginx-container",
          "image": "nginx:latest"
        }
      ]
    }
  }'
```

---

## 🧪 Verification

### 🔹 Check Pod Status

```bash
kubectl get pods
```

Expected:

* `pod-nginx` → Running

---

### 🔹 Describe Pod

```bash
kubectl describe pod pod-nginx
```

Verify:

* Container name: `nginx-container`
* Image: `nginx:latest`
* Label: `app=nginx_app`

---

## 📌 Key Notes

* `--restart=Never` ensures it's a Pod (not Deployment)
* Labels help in grouping and service selection
* Container name must be explicitly defined

---

## 📌 Alternative (YAML – Recommended in Real World)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```

Apply using:

```bash
kubectl apply -f pod.yaml
```

---

## 📌 Real-World Use Cases

* Testing containers in Kubernetes
* Running standalone workloads
* Debugging environments
* Base for Deployments and ReplicaSets

---

## 🧠 Key Learnings

* Creating Pods using `kubectl`
* Understanding labels and container naming
* Difference between Pod and Deployment
* Basic Kubernetes object structure

---

## ❌ What Was Avoided

* ❌ Missing image tag (`latest`)
* ❌ Incorrect container name
* ❌ Not adding labels
* ❌ Using Deployment instead of Pod

---

## ✅ Final Status

✔ Pod `pod-nginx` created successfully
✔ Correct image and tag used
✔ Label applied (`nginx_app`)
✔ Container name set properly
✔ Pod running in cluster

---
