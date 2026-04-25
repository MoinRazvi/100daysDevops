# 🚀 Day 54 – Share Volume Between Containers in a Pod (emptyDir)

## 🎯 Objective

The Nautilus DevOps team needs to create a **multi-container pod** where containers share a common volume.

### Requirements:

* Pod name: `volume-share-datacenter`
* Two containers:

  * `volume-container-datacenter-1`
  * `volume-container-datacenter-2`
* Image: Ubuntu (`ubuntu:latest`)
* Use `emptyDir` volume
* Share data between containers

---

## 🧠 Why This Task Matters

This task is important because:

* Demonstrates **intra-pod communication**
* Introduces **shared storage in Kubernetes**
* Useful for sidecar patterns and temporary data sharing
* Common in logging, caching, and processing pipelines

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Pod**: `volume-share-datacenter`
* **Volume Type**: `emptyDir`

---

## 🛠️ Commands Used

* `kubectl apply`
* `kubectl exec`
* `kubectl get pods`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Create Pod YAML

```yaml id="v6qv4z"
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter
spec:
  volumes:
  - name: volume-share
    emptyDir: {}

  containers:
  - name: volume-container-datacenter-1
    image: ubuntu:latest
    command: ["/bin/sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/official

  - name: volume-container-datacenter-2
    image: ubuntu:latest
    command: ["/bin/sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/apps
```

---

### 🔹 Step 2: Apply Pod

```bash id="u4u7yq"
kubectl apply -f pod.yaml
```

---

## 🧪 Step 3 – Verification

### 🔹 Check Pod Status

```bash id="z7r3ul"
kubectl get pods
```

Expected:

* `volume-share-datacenter` → Running

---

### 🔹 Step 4: Create File in First Container

```bash id="m5r6jc"
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 -- sh
```

Inside container:

```bash id="l8z4pn"
echo "Welcome to xFusionCorp Industries" > /tmp/official/official.txt
exit
```

---

### 🔹 Step 5: Verify in Second Container

```bash id="9c0q3p"
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-2 -- cat /tmp/apps/official.txt
```

Expected Output:

```text id="q9b3wn"
Welcome to xFusionCorp Industries
```

---

## 📌 Key Notes

* `emptyDir` is created when pod starts
* Shared across all containers in the pod
* Data is **temporary** (deleted when pod is removed)

---

## 📌 Real-World Use Cases

* Sidecar containers (logging, monitoring)
* Temporary file sharing between services
* Data processing pipelines
* Cache sharing

---

## 🧠 Key Learnings

* Containers in same pod share:

  * Network
  * Storage (if configured)

* `emptyDir`:

  * Exists as long as pod runs
  * Not persistent

---

## ❌ What Was Avoided

* ❌ Using separate volumes
* ❌ Misconfigured mount paths
* ❌ Missing sleep command (container exit issue)

---

## ✅ Final Status

✔ Pod created successfully
✔ Volume shared between containers
✔ File created in one container
✔ File accessible in second container
✔ Shared storage working as expected

---
