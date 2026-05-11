### The task logic is exactly the same as earlier — only the container names changed from: (PLEASE FOLLOW BELOW COMMANDS IF YOU ARE USING, THERE IS CHANCE IN NAMES AS MENTIONED)

```text
master-redis-datacenter  → master-redis-xfusion
slave-redis-datacenter   → slave-redis-xfusion
php-redis-datacenter     → php-redis-xfusion
```

Here’s the updated YAML with the corrected names 👇

---

# 🔹 Redis Master Deployment

```yaml id="y8rk7n"
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
      - name: master-redis-xfusion
        image: redis

        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"

        ports:
        - containerPort: 6379
```

---

# 🔹 Redis Master Service

```yaml id="1axicq"
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

# 🔹 Redis Slave Deployment

```yaml id="6j39yq"
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
      - name: slave-redis-xfusion
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

# 🔹 Redis Slave Service

```yaml id="dxmmoh"
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

# 🔹 Frontend Deployment

```yaml id="9m2c3k"
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
      - name: php-redis-xfusion
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

# 🔹 Frontend Service

```yaml id="rzflwx"
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

# 🛠️ Apply All Files

```bash id="9t7z3x"
kubectl apply -f redis-master.yaml
kubectl apply -f redis-master-service.yaml

kubectl apply -f redis-slave.yaml
kubectl apply -f redis-slave-service.yaml

kubectl apply -f frontend.yaml
kubectl apply -f frontend-service.yaml
```

---

# 🧪 Verify

```bash id="q9hz8o"
kubectl get deployments
kubectl get pods
kubectl get svc
```

---

# 🌐 Access Application

```text id="a9jq8k"
http://<Node-IP>:30009
```

Or use the **App** button in the lab.
