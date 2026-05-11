# 🚀 Day 67 – Deploying a Multi-Tier Guestbook Application on Kubernetes

## 🎯 Objective

The Nautilus DevOps team deployed a complete **Guestbook Application** using a multi-tier Kubernetes architecture:

* Redis Master
* Redis Slave
* Frontend PHP Application

---

# 🧠 Why This Task Matters

This task demonstrates real-world Kubernetes application architecture:

* Multi-tier deployments
* Backend + frontend communication
* Internal service discovery
* Scaling replicas
* Resource requests
* NodePort exposure

---

# 🧱 Application Architecture

```text id="j40sz0"
User
  ↓
Frontend Service (NodePort:30009)
  ↓
Frontend Pods (PHP Guestbook)
  ↓
Redis Slave Service
  ↓
Redis Master Service
  ↓
Redis Master Pod
```

---

# 🛠️ Implementation

---

# 🔹 BACK-END TIER

---

## 1️⃣ Redis Master Deployment

```yaml id="jw1qaf"
apiVersion: apps/v1
kind: Deployment

metadata:
  name: redis-master

spec:
  replicas: 1

  selector:
    matchLabels:
      app: redis-master

  template:
    metadata:
      labels:
        app: redis-master

    spec:
      containers:
      - name: master-redis-datacenter
        image: redis

        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"

        ports:
        - containerPort: 6379
```

---

## 2️⃣ Redis Master Service

```yaml id="4l7n6g"
apiVersion: v1
kind: Service

metadata:
  name: redis-master

spec:
  selector:
    app: redis-master

  ports:
  - port: 6379
    targetPort: 6379
```

---

## 3️⃣ Redis Slave Deployment

```yaml id="54gwxa"
apiVersion: apps/v1
kind: Deployment

metadata:
  name: redis-slave

spec:
  replicas: 2

  selector:
    matchLabels:
      app: redis-slave

  template:
    metadata:
      labels:
        app: redis-slave

    spec:
      containers:
      - name: slave-redis-datacenter
        image: gcr.io/google_samples/gb-redisslave:v3

        env:
        - name: GET_HOSTS_FROM
          value: dns

        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"

        ports:
        - containerPort: 6379
```

---

## 4️⃣ Redis Slave Service

```yaml id="1u0x80"
apiVersion: v1
kind: Service

metadata:
  name: redis-slave

spec:
  selector:
    app: redis-slave

  ports:
  - port: 6379
    targetPort: 6379
```

---

# 🔹 FRONT-END TIER

---

## 5️⃣ Frontend Deployment

```yaml id="j8y0xf"
apiVersion: apps/v1
kind: Deployment

metadata:
  name: frontend

spec:
  replicas: 3

  selector:
    matchLabels:
      app: guestbook-frontend

  template:
    metadata:
      labels:
        app: guestbook-frontend

    spec:
      containers:
      - name: php-redis-datacenter
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff

        env:
        - name: GET_HOSTS_FROM
          value: dns

        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"

        ports:
        - containerPort: 80
```

---

## 6️⃣ Frontend Service

```yaml id="zbrggs"
apiVersion: v1
kind: Service

metadata:
  name: frontend

spec:
  type: NodePort

  selector:
    app: guestbook-frontend

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
```

---

# 🛠️ Commands Used

```bash id="6a09jlwm"
kubectl apply -f redis-master.yaml
kubectl apply -f redis-master-service.yaml

kubectl apply -f redis-slave.yaml
kubectl apply -f redis-slave-service.yaml

kubectl apply -f frontend.yaml
kubectl apply -f frontend-service.yaml

kubectl get deployments
kubectl get pods
kubectl get svc
```

---

# 🧪 Verification

---

## Verify Pods

```bash id="lbj9mg"
kubectl get pods
```

Expected:

```text id="6rx4lm"
redis-master → 1 pod
redis-slave → 2 pods
frontend → 3 pods
```

---

## Verify Services

```bash id="65mpj4"
kubectl get svc
```

Expected:

```text id="vjexpt"
frontend → NodePort 30009
```

---

# 🌐 Access Guestbook App

```text id="l8ydx7"
http://<Node-IP>:30009
```

---

# 🔍 How It Works

```text id="qdfb8o"
Frontend Pods
      ↓
Redis Slave
      ↓
Redis Master
      ↓
Stores Guestbook Data
```

---

# 📌 Key Concepts Covered

* Multi-tier applications
* Redis architecture
* Frontend/backend communication
* Services
* NodePort exposure
* Resource requests
* Replica scaling

---

# 🧠 Key Learnings

* Services enable pod communication
* Replica scaling improves availability
* NodePort exposes apps externally
* Redis Master/Slave architecture improves performance

---

# ❌ Common Mistakes Avoided

* ❌ Wrong service selectors
* ❌ Incorrect NodePort
* ❌ Missing environment variables
* ❌ Port mismatches
* ❌ Incorrect image references

---

# 🛠️ Important Kubernetes Concepts

## Resource Requests

```yaml id="0m9olc"
resources:
  requests:
    cpu: "100m"
    memory: "100Mi"
```

---

## Environment Variable

```yaml id="5ey9y0"
env:
- name: GET_HOSTS_FROM
  value: dns
```

---

## NodePort Service

```yaml id="9hhc7h"
type: NodePort
nodePort: 30009
```

---

# ✅ Final Status

✔ Redis Master deployed
✔ Redis Slave deployed
✔ Frontend deployed
✔ Services created successfully
✔ Guestbook application accessible

---

# 🚀 Why This Day Was Important

This was a true production-style application architecture involving:

* Multiple deployments
* Internal networking
* Service discovery
* Scaling
* Stateful backend communication


# 🎨 Day 67 Infographic

![Image](https://images.openai.com/static-rsc-4/fMGSZdG-79ed9NkaVP-lkxxyhVFtaX_4rx1tfOhIZl2VwNOlAW8cQL_grHEUZLaWJ8vQtodtaLQjyKoKooNxjGDFuWos5dnVj5YdqMYMZtVsvGBOoDu7IRXzKALzl3CFjRxMRpR5LevYoA1gk7uWme0K83T2LjTUkYadfxoVXKD7c6XyTBtmU10mCQ2di7cQ?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Ffv3MHYZOSg03cwrTZAeT6EKFn0C69jXPJHjiWXhvYqHNSc2fJRYmVl9KTCW-hkQ12M1B7OGREunOWH3erCNXMRCSMz-ZjtxjHUY3kbxh_y2I0aJYwWE3_jgtLvkp-Sucz8RqJ55xlrZ5mJvxampR6qYlJAua5YT96uXuC-VxnSulDZ_f56aCudWKuRdLcRF?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/BjD-dq1PyV61W1by1sI8Po5vVAbUws7zU7iys1eeB1pKq04WZcej1XsZgvHnVJdgm0kqxlqcPcJUfrY9W64ZdT4guKzkp7vxQAlBjQ1wGQGBJ2a3cY-XpvLOfaJD-M0l-puXRY-V-ZmCrqSC-GNS7EZ9Uem0BPWIqE4EkzBZV4305UP8_1ORke0zudVJyX3M?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/h1TpnszsjAuD4vRxuTqj_1XV7Pa3YEfrWT83FyIaVwSnd9x0jBXQYk-9xMRnlxZGLRlYnH6WWmHlQ3K6koNag99kPLDrDBv1dTtvDU44EZLQtinieLmgEbA8UnJRMQSqnZUNmsLvyy02xDdpZIcFHph14jtI_nltyW4Z3eB6zfhjAYRO6RxGFnSEnBX8l4OK?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/wkIjKFyfCBOrKo-oNlYM0KVhC0fHFK7nom0QFGRhQePDHvLHJRNIylyXNQ4vyPZMRAQfmsdZUm2I9GiqIGjtTl82sAOEHJQPMVFG6uY8zBT4WR0ZtSXfrzWC6faSsWPPaO9mCJIJdseb7bJEfpVJL4BWS-7NFlzCAWKaXPNKjE98iXUprEPjrwn6MfjbPK-N?purpose=fullsize)
