# Iron Gallery Kubernetes Deployment

This project demonstrates deployment of a two-tier application (frontend + database) on Kubernetes.

## 📦 Components

* Namespace: iron-namespace-datacenter
* Frontend: Iron Gallery (Nginx-based app)
* Backend: MariaDB database
* Services:

  * NodePort for frontend
  * ClusterIP for database

---

# 🚀 Complete YAML (All in One File)

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-datacenter
---
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
---
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
          value: StrongRoot@123
        - name: MYSQL_PASSWORD
          value: StrongUser@123
        - name: MYSQL_USER
          value: devuser
        volumeMounts:
        - name: db
          mountPath: /var/lib/mysql
      volumes:
      - name: db
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  selector:
    db: mariadb
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  selector:
    run: iron-gallery
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 32678
  type: NodePort
```

---

# ▶️ Apply It

```bash
kubectl apply -f iron-app.yaml
```

---

# 🔍 Verify

```bash
kubectl get all -n iron-namespace-datacenter
```

# Troubleshooting Guide

## ❌ Pods not visible

Check namespace:

```bash
kubectl get pods -n iron-namespace-datacenter
```

---

## ❌ Nothing shows up

Reapply YAML:

```bash
kubectl apply -f iron-app.yaml
```

---

## ❌ Pod CrashLoopBackOff

Check logs:

```bash
kubectl logs <pod-name> -n iron-namespace-datacenter
```

---

## ❌ Service not working

Check endpoints:

```bash
kubectl get svc -n iron-namespace-datacenter
kubectl describe svc iron-gallery-service-datacenter -n iron-namespace-datacenter
```

---

## ❌ Port not accessible

Check NodePort:

```bash
kubectl get svc -n iron-namespace-datacenter
```

# 📄 commands

```bash
# Apply resources
kubectl apply -f iron-app.yaml

# Check resources
kubectl get all -n iron-namespace-datacenter

# Describe deployment
kubectl describe deployment iron-gallery-deployment-datacenter -n iron-namespace-datacenter

# Logs
kubectl logs -n iron-namespace-datacenter <pod-name>

# Delete everything
kubectl delete -f iron-app.yaml
```
---

## 🧠 Key Concepts

* Namespaces isolate resources
* Deployments manage pods
* Services expose applications
* emptyDir is ephemeral storage

---

## ⚠️ Notes

* Database data is not persistent (emptyDir used)
* Not recommended for production use

---

## 🧠 Common Mistakes

* Wrong namespace
* Label mismatch
* Wrong image name
* Missing ports

---



