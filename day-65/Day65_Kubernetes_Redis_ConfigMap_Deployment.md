# 🚀 Day 65 – Deploying Redis with ConfigMap & Volumes in Kubernetes

## 🎯 Objective

The Nautilus DevOps team planned to deploy Redis as an in-memory caching utility for application performance testing in Kubernetes.

### Requirements:

* Create ConfigMap → `my-redis-config`
* Deploy Redis using `redis:alpine`
* Use ConfigMap for Redis configuration
* Mount:

  * EmptyDir volume
  * ConfigMap volume
* Expose Redis port `6379`
* CPU request → `1`

---

# 🧠 Why This Task Matters

This task introduces important Kubernetes production concepts:

* Externalized configuration using ConfigMaps
* In-memory caching architecture
* Volume mounting
* Resource requests
* Stateful service preparation

---

# 🧱 Architecture Overview

```text id="9ydb9p"
ConfigMap (Redis Config)
        ↓
Mounted into Redis Pod
        ↓
Redis Container
        ↓
Data Stored in EmptyDir Volume
```

---

# 🛠️ Implementation

---

## 🔹 Step 1: Create ConfigMap

```yaml id="vjlwmq"
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
```

---

## 🔹 Step 2: Create Redis Deployment

```yaml id="45c60v"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine

        resources:
          requests:
            cpu: "1"

        ports:
        - containerPort: 6379

        volumeMounts:
        - name: data
          mountPath: /redis-master-data

        - name: redis-config
          mountPath: /redis-master

      volumes:
      - name: data
        emptyDir: {}

      - name: redis-config
        configMap:
          name: my-redis-config
```

---

# 🛠️ Commands Used

```bash id="ltvfvn"
kubectl apply -f configmap.yaml
kubectl apply -f redis-deployment.yaml

kubectl get configmap
kubectl get deployments
kubectl get pods
```

---

# 🧪 Verification

```bash id="z2ruha"
kubectl get pods
kubectl describe pod <pod-name>
```

Expected:

```text id="u0ok6u"
STATUS = Running
```

---

## 🔹 Verify ConfigMap Mount

```bash id="9u8j57"
kubectl exec -it <pod-name> -- sh
```

```bash id="2yv3eh"
cd /redis-master
ls
cat redis-config
```

Expected:

```text id="7h0jxf"
maxmemory 2mb
```

---

# 🔍 How It Works

```text id="x2gn0s"
ConfigMap
    ↓
Mounted into Pod
    ↓
Redis Reads Configuration
    ↓
Redis Uses Memory Limit
```

---

# 📌 Key Concepts Covered

* ConfigMaps
* Redis deployment
* Volume mounting
* EmptyDir storage
* Resource requests

---

# 🧠 Key Learnings

* ConfigMaps separate config from container image
* Redis commonly used for caching
* EmptyDir useful for temporary pod storage
* Resource requests help scheduler allocate resources

---

# ❌ Common Mistakes Avoided

* ❌ Wrong ConfigMap name
* ❌ Incorrect mount paths
* ❌ Missing volume mounts
* ❌ Missing CPU requests

---

# 🛠️ Important YAML Sections

## Resource Requests

```yaml id="zclh9y"
resources:
  requests:
    cpu: "1"
```

---

## ConfigMap Volume Mount

```yaml id="ynhn5f"
volumes:
- name: redis-config
  configMap:
    name: my-redis-config
```

---

## EmptyDir Volume

```yaml id="r53a7l"
volumes:
- name: data
  emptyDir: {}
```

---

# ✅ Final Status

✔ ConfigMap created
✔ Redis deployment created
✔ Volumes mounted successfully
✔ Redis container running
✔ Redis configuration accessible

---

# 🚀 Why This Day Was Important

This task combined:

* Application deployment
* Configuration management
* Storage concepts
* Resource allocation


# 🎨 Day 65 Infographic

![Image](https://images.openai.com/static-rsc-4/fgw1Z-2b8uDVf8NrS8SeSoFJa0ZRyTafwqS9t6GNh8CyqRxAv8nKW63PtjL92DZWdFZDmycrXSmQFiaEIVQ8HbdpEZmW-todM16YPFPN4WMxkEKdZK-qgKTpn3DWW_Tq1mMGyDEC5B4HCqL2HQAcwVwPkR45yKaHGnlDcPzr4RDNYY_Zt6jUQ6Z9WAbURcfu?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/llYFs5ngRk8e6IK4L08ePnk9HtfvBE9018bBpM5a397chJ1xPb5Toh_FCKOyK8fFOddsdr0xp4CuHlOAGQRn4Xl3KzVxuz6Zf45YN06C2czGBjUWXQWRGpDirAP1lKOAwoLYhOqWYhNpTQ2ez4Wy_WNDPIDMAWHXZVMGpvwYvhdMXVpBWm9MzaXtjVnQHq88?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Oq3jgKVPhXfLgJDJJrAgcP_5lmFN2n6v5cvDXixPkqBjcdbBBil9OFKvAb9CMkcOxfEDzasmC6skIWifVSYC12-9cz5j2vzO5YqB99PzilVMSysYxL9QhMHUNqyXzZZdscZZuRVHSg7pokRZKejArBEfoUZSmzB_FDL3V4KFED-SNNaAtaolpbXdQL4IDKeQ?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/lDdy0XE0ksDoO3m7j-zTqWWpFnuRuOiPfNsC5AYX9K2fCfKoWKYqSiiyIHDLTpoeiiENrfhKo6ogxu13bP1Z2fBn8lCLu1bnt9W5RkH_xppl4K-7m8yN9y5gFw7EDb4CZWGzaLoQ7nivgVBVH5VPz4sgabhwzuvEJ-IoxZ82yD9LiGBHS-zk_GN-7mGRVcWV?purpose=fullsize)
