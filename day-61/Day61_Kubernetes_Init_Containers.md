# 🚀 Day 61 – Using Init Containers in Kubernetes

## 🎯 Objective

The Nautilus DevOps team needs to prepare application data **before the main container starts**, using **Init Containers**.

### Requirements:

* Deployment: `ic-deploy-xfusion`
* Replicas: `1`
* Label: `app=ic-xfusion`
* Use init container to write data
* Main container reads that data
* Shared volume: `emptyDir`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **Init Containers (very important concept)**
* Enables **pre-processing before app starts**
* Common in:

  * Config preparation
  * Data initialization
  * Dependency checks

---

## 🧱 Environment Details

* **Volume Type**: `emptyDir`
* **Images Used**: `debian:latest`

---

# 🛠️ Implementation

---

## 🔹 Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}

      initContainers:
      - name: ic-msg-xfusion
        image: debian:latest
        command: ["/bin/bash", "-c", "echo Init Done - Welcome to xFusionCorp Industries > /ic/official"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      containers:
      - name: ic-main-xfusion
        image: debian:latest
        command: ["/bin/bash", "-c", "while true; do cat /ic/official; sleep 5; done"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic
```

---

## 🛠️ Commands Used

```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl logs <pod-name>
```

---

## 🧪 Verification

```bash
kubectl logs <pod-name>
```

Expected Output:

```text
Init Done - Welcome to xFusionCorp Industries
```

---

# 🔍 How It Works

```text
Init Container (runs first)
        ↓
Writes file → /ic/official
        ↓
Main Container starts
        ↓
Reads file continuously
```

---

## 📌 Key Notes

* Init container runs **before main container**
* If init fails → pod will not start
* Shared volume used for communication

---

## 📌 Real-World Use Cases

* Database migrations
* Config file generation
* Waiting for dependencies
* Secrets/config preparation

---

## 🧠 Key Learnings

* Init containers ensure **order of execution**
* Containers can communicate via volumes
* Helps avoid modifying base images

---

## ❌ What Was Avoided

* ❌ Hardcoding config inside image
* ❌ Manual pre-setup steps
* ❌ Complex entrypoint scripts

---

## ✅ Final Status

✔ Deployment created
✔ Init container executed successfully
✔ File created and shared
✔ Main container reading data

---

# 🎨 Visual (Day 61 Infographic)

![Image](https://images.openai.com/static-rsc-4/EPU8SJ6mmHDEC9wpcIVeXk6EeqrMqipmUPV_LiX7H6wfUGJHOG3_C13ffcUy5DqKktlv5It9xkjH2EgKg1AVERl9aZiXrqmj4PJyrzzE67WwOGMU_wLd6lAQnFqHpq6cXPiD3tyehTy4Af21oDL-0U85FmGNZnNvOaKvTeVxgNEHQOUbSy4B4dif5rCxmNkb?purpose=fullsize)

---
