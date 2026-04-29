# 🚀 Day 57 – Use Environment Variables in Kubernetes Pod

## 🎯 Objective

The Nautilus DevOps team needs to configure a pod that prints a greeting message using **environment variables**.

### Requirements:

* Pod name: `print-envars-greeting`
* Container name: `print-env-container`
* Image: bash (use bash image)
* Environment variables:

  * `GREETING = Welcome to`
  * `COMPANY = Stratos`
  * `GROUP = Datacenter`
* Command:

  ```bash
  ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  ```
* Restart policy: `Never`

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **environment variables in Kubernetes**
* Demonstrates **dynamic configuration**
* Common in real-world applications
* Helps avoid hardcoding values

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster (via jump-host)
* **Pod**: `print-envars-greeting`

---

## 🛠️ Commands Used

* `kubectl apply`
* `kubectl get pods`
* `kubectl logs`

---

## 🛠️ Implementation Steps

---

### 🔹 Step 1: Create Pod YAML

```yaml id="k8z3q1"
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
  - name: print-env-container
    image: bash:latest
    command: ["/bin/sh", "-c", "echo \"$GREETING $COMPANY $GROUP\""]
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "Stratos"
    - name: GROUP
      value: "Datacenter"
```

---

### 🔹 Step 2: Apply Pod

```bash id="v2q7xk"
kubectl apply -f pod.yaml
```

---

## 🧪 Step 3 – Verification

```bash id="p9x4nt"
kubectl get pods
```

Expected:

* Pod completes successfully

---

### 🔹 Step 4: Check Logs

```bash id="h7c3lm"
kubectl logs -f print-envars-greeting
```

Expected Output:

```text id="y3v8pz"
Welcome to Stratos Datacenter
```

---

## 📌 Key Notes

* Environment variables are injected into container at runtime
* `restartPolicy: Never` prevents restart loop
* Commands can dynamically use environment variables

---

## 📌 Real-World Use Cases

* Application configuration
* API keys and secrets (via secrets/configmaps)
* Environment-specific values (dev/staging/prod)

---

## 🧠 Key Learnings

* Kubernetes supports environment variables natively
* Avoid hardcoding values in applications
* Useful for flexible deployments
* Logs help verify execution

---

## ❌ What Was Avoided

* ❌ Hardcoding values in command
* ❌ Restart loops
* ❌ Incorrect env variable syntax

---

## ✅ Final Status

✔ Pod created successfully
✔ Environment variables configured
✔ Output printed correctly
✔ No restart loop

---
