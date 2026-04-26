# 🚀 Day 55 – Fix Sidecar Logging Setup in Kubernetes

## 🎯 Objective

The Nautilus DevOps team implemented a **sidecar pattern** to ship logs from an Nginx container using a shared volume.

### Requirements:

* Pod name: `webserver`
* Two containers:

  * `nginx-container`
  * `sidecar-container`
* Shared volume: `shared-logs` (`emptyDir`)
* Logs should be accessible by sidecar container

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates **sidecar pattern (production concept)**
* Shows **log handling architecture**
* Enforces **separation of concerns**
* Common in observability pipelines

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Pod**: `webserver`
* **Volume Type**: `emptyDir`

---

## 🛠️ Commands Used (Actual Debug Flow)

```bash
# Check pod status
kubectl get pods

# Check logs from sidecar
kubectl logs webserver -c sidecar-container

# Describe pod (verify mounts and images)
kubectl describe pod webserver

# Delete incorrect pod
kubectl delete pod webserver

# Recreate pod with correct configuration
kubectl apply -f pod.yaml
```

---

## 🧪 Issue Observed

```text
cat: /var/log/nginx/access.log: No such file or directory
cat: /var/log/nginx/error.log: No such file or directory
```

Additionally, lab validation errors:

* ❌ Incorrect image used for nginx container
* ❌ Shared volume not mounted correctly

---

## 🔍 Investigation Steps

### 🔹 Step 1: Checked Sidecar Logs

```bash
kubectl logs webserver -c sidecar-container
```

👉 Observed missing log files

---

### 🔹 Step 2: Verified Pod Configuration

```bash
kubectl describe pod webserver
```

Checked:

* Image name
* Volume mounts
* Mount paths

---

### 🔹 Step 3: Identified Issues

#### ❌ Issue 1: Incorrect Image Tag

* Used:

```yaml
image: nginx
```

* Required:

```yaml
image: nginx:latest
```

---

#### ❌ Issue 2: Volume Mount Misconfiguration

* Volume not correctly mounted in sidecar container
* Path mismatch or missing volumeMount

---

## ❗ Root Cause

* Nginx logs were not accessible because:

  * Incorrect image specification
  * Shared volume not mounted properly

👉 Sidecar container could not read logs

---

## ✅ Fix Applied

### 🔹 Correct Pod Configuration

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
  - name: shared-logs
    emptyDir: {}

  containers:
  - name: nginx-container
    image: nginx:latest
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/nginx

  - name: sidecar-container
    image: ubuntu:latest
    command: ["sh", "-c", "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/nginx
```

---

### 🔹 Recreated Pod

```bash
kubectl delete pod webserver
kubectl apply -f pod.yaml
```

---

### 🔹 Generated Traffic (to create logs)

```bash
kubectl exec -it webserver -c nginx-container -- curl localhost
```

---

## 🧪 Verification

```bash
kubectl logs webserver -c sidecar-container
```

✔ Access logs visible
✔ Error logs visible
✔ Sidecar reading logs successfully

---

## 📌 Key Notes

* Nginx creates logs only after receiving traffic
* Both containers must mount the same volume
* Image tag must match exactly in lab environments

---

## 📌 Real-World Use Cases

* Log shipping to ELK / Splunk
* Monitoring pipelines
* Sidecar-based observability
* Microservices logging

---

## 🧠 Key Learnings

* Sidecar pattern enables clean separation

* Shared volumes are critical for inter-container communication

* Debugging requires checking:

  * Image
  * Volume
  * Paths

* Logs not present ≠ failure (may require traffic)

---

## ❌ What Was Avoided

* ❌ Adding logging logic inside main container
* ❌ Using persistent storage unnecessarily
* ❌ Ignoring lab constraints

---

## ✅ Final Status

✔ Correct image used (`nginx:latest`)
✔ Shared volume mounted properly
✔ Sidecar container reading logs
✔ Logging pipeline working

---
