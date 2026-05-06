# 🚀 Day 62 – Kubernetes Secrets + Real Debugging Scenario

## 🎯 Objective

The Nautilus DevOps team needed to securely store sensitive data using **Kubernetes Secrets** and mount it inside a container.

### Requirements:

* Create Secret: `ecommerce` from `/opt/ecommerce.txt`
* Create Pod: `secret-xfusion`
* Mount secret at `/opt/demo`
* Verify inside container

---

## 🧠 Why This Task Matters

* Secrets are critical for **secure configuration**
* Prevents exposing credentials
* Widely used in **production environments**
* Introduces secure data handling in Kubernetes

---

## 🧱 Environment Details

* Secret Type: generic
* Image: `ubuntu:latest`
* Mount Path: `/opt/demo`

---

# 🛠️ Implementation

---

## 🔹 Step 1: Create Secret

```bash
kubectl create secret generic ecommerce \
--from-file=/opt/ecommerce.txt
```

---

## 🔹 Step 2: Pod YAML (Final Working)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-xfusion
spec:
  containers:
  - name: secret-container-xfusion
    image: ubuntu:latest
    command: ["/bin/bash", "-c", "sleep 3600"]
    volumeMounts:
    - name: secret-volume
      mountPath: /opt/demo

  volumes:
  - name: secret-volume
    secret:
      secretName: ecommerce
```

---

## 🛠️ Commands Used

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl exec -it secret-xfusion -- bash
ls /opt/demo
cat /opt/demo/ecommerce.txt
```

---

# 🧪 Issues Faced & Fixes (REAL DEBUGGING)

---

## ❌ Issue 1: Missing `kind`

```text
error validating data: kind not set
```

✔ Fix:

```yaml
kind: Pod
```

---

## ❌ Issue 2: Wrong Case (`Kind`)

```yaml
Kind: Pod ❌
kind: Pod ✅
```

---

## ❌ Issue 3: Wrong Field Name

```yaml
container ❌
containers ✅
```

---

## ❌ Issue 4: Typo in mountPath

```yaml
mounthPath ❌
mountPath ✅
```

---

## ❌ Issue 5: Volumes outside spec

```yaml
volumes: ❌ (wrong level)
```

✔ Fix → Move inside `spec`

---

## ❌ Issue 6: Wrong Secret Reference

```yaml
secretName: ecommerce ❌ (direct)
```

✔ Fix:

```yaml
secret:
  secretName: ecommerce ✅
```

---

## ❌ Issue 7: YAML Syntax Errors

```text
mapping values are not allowed
```

👉 Root cause:

* Indentation issues
* Formatting mistakes

✔ Fix:

* Proper spacing
* Correct structure

---

## 🔍 Debugging Approach

1. Read error messages carefully
2. Fixed one issue at a time
3. Validated YAML structure
4. Re-applied and verified

---

## 🧪 Verification

```bash
kubectl get pods
kubectl exec -it secret-xfusion -- bash
```

```bash
ls /opt/demo
cat /opt/demo/ecommerce.txt
```

✔ Secret file available inside container

---

# 🔍 How It Works

```text
Secret (ecommerce)
        ↓
Mounted as Volume
        ↓
Container (/opt/demo)
        ↓
File accessible securely
```

---

## 📌 Key Notes

* Secrets are base64 encoded
* Mounted as files in container
* Safer than plain-text configs

---

## 📌 Real-World Use Cases

* API keys
* DB credentials
* TLS certificates
* License keys

---

## 🧠 Key Learnings

* Kubernetes is strict about schema
* YAML syntax matters a lot
* Secrets should never be hardcoded
* Debugging requires attention to detail

---

## ❌ What Was Avoided

* ❌ Hardcoding sensitive data
* ❌ Ignoring validation errors
* ❌ Blind debugging

---

## ✅ Final Status

✔ Secret created successfully
✔ Pod running
✔ Secret mounted correctly
✔ Data verified inside container

---
