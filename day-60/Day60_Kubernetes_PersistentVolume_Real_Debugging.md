
# 🚀 Day 60 – Persistent Volume Setup + Real Debugging in Kubernetes

## 🎯 Objective

The Nautilus DevOps team needed to deploy a web application using **persistent storage** in Kubernetes.

### Requirements:

* Create **PersistentVolume** → `pv-xfusion`
* Create **PersistentVolumeClaim** → `pvc-xfusion`
* Create Pod → `pod-xfusion`
* Mount storage to web server
* Expose via NodePort service

---

## 🧠 Why This Task Matters

* Introduces **persistent storage (stateful workloads)**
* Real-world Kubernetes setup
* Involves **multiple components working together**
* Includes **real debugging scenarios**

---

## 🧱 Environment Details

* **Storage Type**: hostPath
* **Path**: `/mnt/dba`
* **Storage Class**: manual
* **Access Mode**: ReadWriteOnce

---

# 🛠️ Implementation

---

## 🔹 Step 1: PersistentVolume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-xfusion
spec:
  capacity:
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath:
    path: /mnt/dba
```

---

## 🔹 Step 2: PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-xfusion
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
  storageClassName: manual
```

---

## 🔹 Step 3: Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-xfusion
  labels:
    app: web
spec:
  containers:
  - name: container-xfusion
    image: httpd:latest
    volumeMounts:
    - name: web-storage
      mountPath: /usr/local/apache2/htdocs
  volumes:
  - name: web-storage
    persistentVolumeClaim:
      claimName: pvc-xfusion
```

---

## 🔹 Step 4: Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-xfusion
spec:
  type: NodePort
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30008
```

---

## 🛠️ Commands Used

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f pod.yaml
kubectl apply -f service.yaml

kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get svc
kubectl get endpoints web-xfusion
```

---

# 🧪 Issues Faced & Fixes (REAL DEBUGGING)

---

## ❌ Issue 1: Typo in mountPath

```yaml
mouthpath ❌
mountPath ✅
```

✔ Fix: Correct field name

---

## ❌ Issue 2: Service Type Case Error

```yaml
Nodeport ❌
NodePort ✅
```

✔ Fix: Correct case-sensitive value

---

## ❌ Issue 3: Volume Name Mismatch

```yaml
volumeMounts.name ≠ volumes.name ❌
```

✔ Fix: Keep same name (`web-storage`)

---

## ❌ Issue 4: Pod Label Missing

```bash
kubectl get pod --show-labels
→ <none>
```

👉 Service could not route traffic

✔ Fix:

```yaml
labels:
  app: web
```

---

## ❌ Issue 5: YAML Syntax Error

```text
mapping values are not allowed in this context
```

👉 Root cause:

* Wrong indentation
* Incorrect formatting

✔ Fix: Proper YAML structure

---

## 🔍 Debugging Approach

1. Checked resource status
2. Verified PV/PVC binding
3. Checked pod labels
4. Validated service selector
5. Fixed YAML errors

---

## 🧪 Verification

```bash
kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get svc
kubectl get endpoints web-xfusion
```

✔ PV → Bound
✔ PVC → Bound
✔ Pod → Running
✔ Service → Created
✔ Endpoints → Available

---

## 🌐 Access Application

```bash
http://<node-ip>:30008
```

✔ Web server accessible

---

# 🧠 Key Learnings

* PV + PVC = persistent storage
* Kubernetes is **strict about YAML & syntax**
* Labels are critical for service routing
* Debugging is about **systematic checks**
* Small mistakes can break entire setup

---

## ❌ What Was Avoided

* ❌ Recreating resources blindly
* ❌ Ignoring validation errors
* ❌ Guessing instead of debugging

---

## ✅ Final Status

✔ Persistent storage configured
✔ Pod successfully mounted volume
✔ Service exposed application
✔ Application accessible
✔ All issues resolved

---
