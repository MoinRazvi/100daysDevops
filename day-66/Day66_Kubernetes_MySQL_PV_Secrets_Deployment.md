# 🚀 Day 66 – Deploying MySQL with Persistent Volume & Secrets in Kubernetes

## 🎯 Objective

The Nautilus DevOps team needed to deploy a MySQL database on Kubernetes using:

* Persistent Storage
* Secrets
* Environment Variables
* NodePort Service

---

# 🧠 Why This Task Matters

This task combines multiple production-level Kubernetes concepts:

* PersistentVolumes (PV)
* PersistentVolumeClaims (PVC)
* Secrets management
* Stateful application deployment
* Environment variable injection
* Service exposure

---

# 🧱 Architecture Overview

```text id="9n53ui"
Secrets
   ↓
Environment Variables
   ↓
MySQL Deployment
   ↓
PersistentVolumeClaim
   ↓
PersistentVolume
   ↓
Host Storage
```

---

# 🛠️ Implementation

---

# 🔹 Step 1: Create PersistentVolume

```yaml id="8gkg55"
apiVersion: v1
kind: PersistentVolume

metadata:
  name: mysql-pv

spec:
  capacity:
    storage: 250Mi

  accessModes:
    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:
    path: /mnt/mysql-data
```

---

# 🔹 Step 2: Create PersistentVolumeClaim

```yaml id="sjlwmx"
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: mysql-pv-claim

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 250Mi
```

---

# 🔹 Step 3: Create Secrets

---

## MySQL Root Password Secret

```bash id="4ebj76"
kubectl create secret generic mysql-root-pass \
--from-literal=password=YUIidhb667
```

---

## MySQL User Secret

```bash id="b1qf7q"
kubectl create secret generic mysql-user-pass \
--from-literal=username=kodekloud_gem \
--from-literal=password=ksH85UJjhb
```

---

## MySQL Database Secret

```bash id="qlszb9"
kubectl create secret generic mysql-db-url \
--from-literal=database=kodekloud_db3
```

---

# 🔹 Step 4: Create MySQL Deployment

```yaml id="8pw01o"
apiVersion: apps/v1
kind: Deployment

metadata:
  name: mysql-deployment

spec:
  replicas: 1

  selector:
    matchLabels:
      app: mysql

  template:
    metadata:
      labels:
        app: mysql

    spec:
      containers:
      - name: mysql-container
        image: mysql:5.7

        ports:
        - containerPort: 3306

        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password

        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database

        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username

        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password

        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql

      volumes:
      - name: mysql-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```

---

# 🔹 Step 5: Create NodePort Service

```yaml id="1a0r9q"
apiVersion: v1
kind: Service

metadata:
  name: mysql

spec:
  type: NodePort

  selector:
    app: mysql

  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
```

---

# 🛠️ Commands Used

```bash id="7jlwm7"
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get svc
kubectl get secrets
```

---

# 🧪 Verification

---

## Verify PV & PVC

```bash id="i6ax7q"
kubectl get pv
kubectl get pvc
```

Expected:

```text id="hqj8vt"
STATUS = Bound
```

---

## Verify Pod

```bash id="yg2t3r"
kubectl get pods
```

Expected:

```text id="ql8yrc"
STATUS = Running
```

---

## Verify Secrets

```bash id="t0pvci"
kubectl get secrets
```

---

# 🌐 Access MySQL

```text id="bhz2fr"
<Node-IP>:30007
```

---

# 🔍 How It Works

```text id="f36dl5"
Secrets
   ↓
Injected as Environment Variables
   ↓
MySQL Container
   ↓
Persistent Storage Mounted
   ↓
Data Persisted
```

---

# 📌 Key Concepts Covered

* PersistentVolumes
* PersistentVolumeClaims
* Secrets
* Environment variables
* Stateful applications
* NodePort services

---

# 🧠 Key Learnings

* Secrets securely manage sensitive credentials
* PV/PVC separate storage from containers
* Environment variables can consume secret values
* Stateful apps require persistent storage

---

# ❌ Common Mistakes Avoided

* ❌ Wrong secret keys
* ❌ Missing volume mounts
* ❌ PVC not bound
* ❌ Incorrect NodePort configuration
* ❌ Missing selector labels

---

# 🛠️ Important Kubernetes Concepts

## SecretKeyRef Example

```yaml id="jyzj6m"
valueFrom:
  secretKeyRef:
    name: mysql-root-pass
    key: password
```

---

## PVC Volume Mount

```yaml id="r8pkl8"
volumes:
- name: mysql-storage
  persistentVolumeClaim:
    claimName: mysql-pv-claim
```

---

# ✅ Final Status

✔ PersistentVolume created
✔ PVC bound successfully
✔ Secrets configured
✔ MySQL deployment running
✔ NodePort service exposed
✔ Persistent storage mounted

---

# 🚀 Why This Day Was Important

This task covered core production database deployment concepts:

* Storage persistence
* Secure secret management
* Environment configuration
* Service exposure

# 🎨 Day 66 Infographic
---
![Image](https://images.openai.com/static-rsc-4/hVcywd68xBCKvxUkUH5BIoe_g7Q_lR5gnpPCMfEYr_DnboyzqiUtrzAJbaBIP6aUC2C0CbjmThea3AyS4M_KSTOO3maDvWL7-FwxXhs4bn4QxvwJLZ-27NkaCG38LU9tgcKc_qyqgGrc5T-FAdKToQQcrjHfzMFFPyMz3Ngj-wbuHLbvPxk_F8G91dAGnZeE?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/fnaRIImwCzt01doZfOKHGzu5mpyskh1y6JCq20epaP8wNl9SHYxJs4xL83M72f07C-TrFcrECdqfj9EbH44UzhJ2VXVgXN35U_3y_EtNai_sOJK0-6WWmOzcBrAWVOP9x3F1ZiN_3fNJ5QPIyjB1e67ezNQPBUo_qR0comfN_70ESdfwMA2kq-aZMMXKqzbR?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/w1KefnEXHbKOuA5TFDszFOQvTKkPnq8hVJDpAbS_dtnbrTxcskupWxiQqZ1XRJNyBRkFCEuAH0J4F7epy9nNNklwPrhzMSOhVl1KPLoJIOWLRvyYHnb2FlVGggRA7YvDc-IO7O08qJrrjoo7DkDW5hITuZYX8MiYytcE3CceXa3ux4ffOPqhbR7lLZEX-m5b?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/l3Wz19LhqmKjQRC9rJJvQAfZO6A8Wjuixy2NUMNdHWi1X34WKuqP0jStGaMztki5ney4JOq76tcKXYgWfJ9XuGgNl6EjRbx5y2ytV51vjyeEXPZ3VKXSmdD5gWyUZZHdD7KbBKGGVL74lFbkGoJOpKNsHsbS1Q7lErm3dSHqys6gTB9rUrD0-vvbtFn--sBv?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Bca17mOCbVYhyyk-QDDHwyK0z799Rd0sc0Uo4LXBliYoGV8QrU7j01-KJY8hqchuH_l_fIA-83phk5mq9_gX_yC_1Q5bLAE0y0yupsVbtThNDz8Am89lKFnRskGWEKMJMJXwaRfWte4sJtfBPTalLFFFI2_XoHVQ0JE671-39U3Wd51b_de5XdOBNByST2yh?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/4lLh6rebaEU2AASJA8SvVJCt6l_a2WePxJbUJFDstNxrG7rHlgFV1zg3D92xLf75oC3uJKeshuR-1N5TTO88EYgQqOB3Co0-5VBFSYjIizpNLDXCkLiUPQmpfOydu-mCpOPPLvOvTJegxNpTzpbOuUxi4MzGhAwbHINj6EfjLpZaa5wtZcWKXMpkhaH_JJ-M?purpose=fullsize)
