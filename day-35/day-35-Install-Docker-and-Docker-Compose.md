
# 🚀 Day 36 – Install Docker and Docker Compose

## 🎯 Objective

The Nautilus DevOps team aims to containerize applications. As part of the setup, the requirement is to:

* Install **Docker (docker-ce)** on App Server 1
* Install **Docker Compose**
* Start and enable the Docker service

---

# 🧠 Why This Task Matters

Docker is used to:

* Containerize applications
* Ensure environment consistency
* Simplify deployment
* Support CI/CD pipelines

Docker Compose helps:

* Run multi-container applications
* Manage services using a single configuration file

---

# 🧱 Environment Details

* **Server**: App Server 1 (`stapp01`)
* **User**: `tony`
* **Packages Required**:

  * Docker
  * Docker Compose

---

# 🛠️ Commands Used

* `yum`
* `systemctl`
* `docker`
* `docker-compose`

---

# 🧪 Initial Attempt (What Went Wrong)

### ❌ Issue Observed

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Error:

```text
No match for argument: docker-ce
No match for argument: docker-ce-cli
No match for argument: containerd.io
No match for argument: docker-compose-plugin
```

---

### 🔍 Root Cause

* Docker CE repository is **not configured**
* KodeKloud lab environment does **not include external repos**
* Hence `docker-ce` packages are unavailable

---

# ✅ Correct Fix (Lab-Safe Approach)

### 🔹 Step 1: Install Docker

```bash
sudo yum install -y docker
```

---

### 🔹 Step 2: Install Docker Compose

```bash
sudo yum install -y docker-compose
```

---

### 🔹 Step 3: Start Docker Service

```bash
sudo systemctl start docker
```

---

### 🔹 Step 4: Enable Docker on Boot

```bash
sudo systemctl enable docker
```

---

# 🧪 Step 5 – Verification

Check service status:

```bash
sudo systemctl status docker
```

Expected:

```text
active (running)
```

---

Check Docker version:

```bash
docker --version
```

---

Check Docker Compose:

```bash
docker-compose --version
```

---

# 📌 Real-World vs Lab Difference

| Scenario           | Installation Method        |
| ------------------ | -------------------------- |
| Real-world servers | docker-ce repo required    |
| KodeKloud lab      | `yum install docker` works |

---

# 📌 Real-World Use Cases

* Application containerization
* Microservices architecture
* CI/CD pipelines
* Cloud-native deployments

---

# 🧠 Key Learnings

* Package availability depends on repositories
* Labs may use simplified packages
* Always validate available packages before installing
* Docker service must be running before use

---

# ❌ What Was Avoided

* ❌ Adding external repositories unnecessarily
* ❌ Repeated failed installations
* ❌ Skipping service start
* ❌ Ignoring environment differences

---

# ✅ Final Status

✔ Docker installed successfully
✔ Docker Compose installed
✔ Docker service running
✔ Docker enabled on boot

---
