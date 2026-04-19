# 🚀 Day 50 – Create Kubernetes Pod with Resource Limits

## 🎯 Objective

The Nautilus DevOps team needs to create a **Kubernetes Pod** with defined resource requests and limits to manage performance and resource utilization.

### Requirements:

* Pod name: `httpd-pod`
* Container name: `httpd-container`
* Image: `httpd:latest`
* Resource configuration:

  **Requests:**

  * Memory: `15Mi`
  * CPU: `100m`

  **Limits:**

  * Memory: `20Mi`
  * CPU: `100m`

---

## 🧠 Why This Task Matters

This task is important because:

* Prevents resource starvation in clusters
* Ensures fair resource allocation
* Helps Kubernetes scheduler make better decisions
* Critical for **production stability and performance**

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Tool**: `kubectl`
* **Pod Name**: `httpd-pod`

---

## 🛠️ Commands Used

* `kubectl apply`
* `kubectl get pods`
* `kubectl describe pod`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Create Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
```

---

### 🔹 Step 2: Apply Configuration

```bash
kubectl apply -f pod.yaml
```

---

## 🧪 Verification

### 🔹 Check Pod Status

```bash
kubectl get pods
```

Expected:

* `httpd-pod` → Running

---

### 🔹 Describe Pod

```bash
kubectl describe pod httpd-pod
```

Verify:

* Requests and Limits are correctly set
* Container is using `httpd:latest`

---

## 📌 Key Notes

* **Requests** → minimum guaranteed resources
* **Limits** → maximum allowed resources
* CPU `100m` = 0.1 CPU core
* Memory defined in Mi (Mebibytes)

---

## 📌 Real-World Use Cases

* Preventing noisy neighbor issues
* Cost optimization in clusters
* Ensuring app performance consistency
* Production workload management

---

## 🧠 Key Learnings

* Difference between requests vs limits
* How Kubernetes schedules pods based on resources
* Importance of resource planning
* Writing resource configs in YAML

---

## ❌ What Was Avoided

* ❌ Running pods without resource constraints
* ❌ Misconfigured CPU/memory units
* ❌ Skipping verification
* ❌ Over-allocating resources

---

## ✅ Final Status

✔ Pod `httpd-pod` created successfully
✔ Resource requests and limits configured
✔ Container running with defined constraints
✔ Pod verified

---
