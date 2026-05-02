# 🚀 Day 58 – Deploy Grafana on Kubernetes with NodePort

## 🎯 Objective

The Nautilus DevOps team plans to deploy **Grafana** on Kubernetes to visualize and analyze application metrics.

### Requirements:

* Deployment: `grafana-deployment-devops`
* Image: Grafana (any valid tag)
* Service:

  * Type: NodePort
  * NodePort: `32000`
* Access Grafana login page

---

## 🧠 Why This Task Matters

This task is important because:

* Introduces **monitoring & observability tools**
* Grafana is widely used in **production systems**
* Demonstrates **real-world DevOps stack setup**
* Foundation for metrics visualization

---

## 🧱 Environment Details

* **Environment**: Kubernetes Cluster
* **Deployment**: `grafana-deployment-devops`
* **Service**: NodePort (32000)

---

## 🛠️ Commands Used

* `kubectl apply`
* `kubectl get pods`
* `kubectl get svc`
* `kubectl describe svc`

---

## 🛠️ Implementation Steps

---

### 🔹 Step 1: Create Deployment YAML

```yaml id="g7r4n1"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana-container
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
```

---

### 🔹 Step 2: Apply Deployment

```bash id="y2c9pt"
kubectl apply -f deployment.yaml
```

---

### 🔹 Step 3: Create Service YAML

```yaml id="q4z6xm"
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000
```

---

### 🔹 Step 4: Apply Service

```bash id="h5t8vn"
kubectl apply -f service.yaml
```

---

## 🧪 Verification

```bash id="n8k2wd"
kubectl get pods
kubectl get svc
```

---

### 🔹 Access Grafana

```bash id="s3l7rq"
http://<node-ip>:32000
```

✔ Grafana login page accessible

(Default credentials: admin / admin)

---

## 📌 Key Notes

* Grafana runs on port **3000**
* NodePort exposes it externally
* No internal configuration required for this task

---

## 📌 Real-World Use Cases

* Monitoring dashboards
* Metrics visualization
* Alerting systems
* Observability pipelines

---

## 🧠 Key Learnings

* Deploying third-party tools on Kubernetes
* Exposing services using NodePort
* Observability stack basics
* Service-to-pod communication

---

## ❌ What Was Avoided

* ❌ Complex Grafana configuration
* ❌ Persistent storage setup
* ❌ Overcomplicating deployment

---

## ✅ Final Status

✔ Deployment created
✔ Pod running successfully
✔ Service exposed on NodePort 32000
✔ Grafana login page accessible

---
