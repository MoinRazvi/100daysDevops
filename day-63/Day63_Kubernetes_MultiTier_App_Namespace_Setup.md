# 🚀 Day 63 – Multi-Tier Application Deployment with Namespace in Kubernetes

## 🎯 Objective

The Nautilus DevOps team needs to deploy a **multi-tier application (frontend + database)** inside a dedicated namespace.

### Requirements:

* Create Namespace → `iron-namespace-datacenter`
* Deploy:

  * Web App (Iron Gallery)
  * Database (MariaDB)
* Use volumes, environment variables
* Expose services

---

## 🧠 Why This Task Matters

* Introduces **namespace isolation**
* Multi-tier architecture (App + DB)
* Resource management & limits
* Production-style deployment

---

## 🧱 Architecture Overview

```text
User → NodePort Service (Gallery)
        ↓
Iron Gallery Pod
        ↓
ClusterIP Service (DB)
        ↓
MariaDB Pod
```

---

# 🛠️ Implementation

---

## 🔹 Step 1: Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-datacenter
```

---

## 🔹 Step 2: Iron Gallery Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-datacenter
  namespace: iron-namespace-datacenter
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
      - name: iron-gallery-container-datacenter
        image: kodekloud/irongallery:2.0
        resources:
          limits:
            memory: "100Mi"
            cpu: "50m"
        volumeMounts:
        - name: config
          mountPath: /usr/share/nginx/html/data
        - name: images
          mountPath: /usr/share/nginx/html/uploads

      volumes:
      - name: config
        emptyDir: {}
      - name: images
        emptyDir: {}
```

---

## 🔹 Step 3: Iron DB Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-datacenter
  namespace: iron-namespace-datacenter
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
      - name: iron-db-container-datacenter
        image: kodekloud/irondb:2.0
        env:
        - name: MYSQL_DATABASE
          value: database_apache
        - name: MYSQL_ROOT_PASSWORD
          value: StrongPass@123
        - name: MYSQL_PASSWORD
          value: StrongPass@123
        - name: MYSQL_USER
          value: appuser
        volumeMounts:
        - name: db
          mountPath: /var/lib/mysql

      volumes:
      - name: db
        emptyDir: {}
```

---

## 🔹 Step 4: DB Service (ClusterIP)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  type: ClusterIP
  selector:
    db: mariadb
  ports:
  - port: 3306
    targetPort: 3306
    protocol: TCP
```

---

## 🔹 Step 5: Gallery Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  type: NodePort
  selector:
    run: iron-gallery
  ports:
  - port: 80
    targetPort: 80
    nodePort: 32678
    protocol: TCP
```

---

# 🛠️ Commands Used

```bash
kubectl apply -f namespace.yaml
kubectl apply -f iron-gallery.yaml
kubectl apply -f iron-db.yaml
kubectl apply -f db-service.yaml
kubectl apply -f gallery-service.yaml

kubectl get all -n iron-namespace-datacenter
```

---

## 🧪 Verification

```bash
kubectl get pods -n iron-namespace-datacenter
kubectl get svc -n iron-namespace-datacenter
```

---

## 🌐 Access Application

```bash
http://<node-ip>:32678
```

---

# 🔍 Key Concepts Covered

* Namespace isolation
* Multi-tier architecture
* Services (ClusterIP + NodePort)
* Resource limits
* Volume mounts

---

## 🧠 Key Learnings

* Namespaces isolate workloads
* Services enable communication between components
* Labels are critical for service discovery
* Resource limits prevent resource overuse

---

## ❌ Common Mistakes Avoided

* ❌ Namespace mismatch
* ❌ Label mismatch
* ❌ Missing resource limits
* ❌ Wrong service type

---

## ✅ Final Status

✔ Namespace created
✔ Gallery deployment running
✔ DB deployment running
✔ Services configured
✔ Application accessible

---
