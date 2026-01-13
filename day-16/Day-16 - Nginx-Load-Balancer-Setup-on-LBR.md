# 🚀 Day 16 of 100 – Nginx Load Balancer Configuration (High Availability)

## 🎯 Objective

Configure the **LBR (Load Balancer) server** using **Nginx** to distribute HTTP traffic across multiple **App Servers**, completing a **high-availability architecture** for a growing production application.

This task reflects a **real-world scaling and performance optimization scenario** handled by DevOps and Production Support teams.

---

## 🧩 Problem Statement

Due to increasing traffic and performance degradation, the Nautilus team decided to migrate the application to a **high availability stack**.
All backend App Servers were ready — the **final missing piece was the Load Balancer (LBR) configuration**.

---

## 🏗️ Environment Details

| Component           | Value                           |
| ------------------- | ------------------------------- |
| Load Balancer       | LBR Server (`stlb01`)           |
| Web Server on LBR   | Nginx                           |
| App Servers         | `stapp01`, `stapp02`, `stapp03` |
| App Server Port     | `8088`                          |
| Nginx Config File   | `/etc/nginx/nginx.conf`         |
| Load Balancing Type | HTTP (Round Robin – default)    |

---

## 🛠️ Tools & Commands Used

* `yum`
* `systemctl`
* `nginx`
* `nginx -t`
* `vi`
* `curl`
* `ss`

---

## 🧪 Step-by-Step Implementation

### ✅ Step 1: Install Nginx on LBR Server

```bash
sudo yum install -y nginx
```

Enable and start Nginx:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

---

### ✅ Step 2: Configure Load Balancing in Main Nginx Config

> ⚠️ **Important requirement:**
> Only the **main Nginx configuration file** must be updated.

Edit the file:

```bash
sudo vi /etc/nginx/nginx.conf
```

---

### ✅ Step 3: Define Upstream Block (HTTP Context)

Inside the `http { }` block:

```nginx
upstream nautilus_app_servers {
    server stapp01:8088;
    server stapp02:8088;
    server stapp03:8088;
}
```

---

### ✅ Step 4: Configure Server Block to Use Load Balancer

Also inside the `http { }` block:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://nautilus_app_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

### ✅ Step 5: Validate and Restart Nginx

```bash
sudo nginx -t
```

Expected:

```text
syntax is ok
test is successful
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 🧪 Step 6: Backend Verification from LBR

```bash
curl http://stapp01:8088
curl http://stapp02:8088
curl http://stapp03:8088
```

✅ All backends reachable

---

### ✅ Step 7: Final Testing from Jump Host / Browser

```bash
curl http://<lbr-ip>
```

Traffic successfully routed via the load balancer.

---

## 🚧 Issues Faced & How They Were Resolved

### ❌ Issue 1: nginx error page (50x) on LBR

**Symptom:**
Default Nginx error page:

> *“The page you are looking for is temporarily unavailable”*

**Root Cause:**

* `upstream` block missing or misconfigured
* Incorrect backend port used
* `proxy_pass` not pointing to upstream

**Fix:**

* Added correct `upstream` block
* Used correct backend port (`8088`)
* Linked `proxy_pass` to upstream

---

### ❌ Issue 2: Tried managing Apache (`httpd`) on LBR

```bash
systemctl restart httpd
Unit httpd.service not found
```

**Root Cause:**

* LBR uses **Nginx**, not Apache

**Fix:**

* Managed `nginx` service instead of `httpd`

---

### ❌ Issue 3: Backend services not reachable

**Root Cause:**

* App servers were not verified before load balancer testing

**Fix:**

* Verified backend ports using:

```bash
ss -tulnp | grep 8088
```

---

## 📌 Real-World Use Cases

* High-availability web architectures
* Traffic distribution and scaling
* Nginx as a Layer-7 load balancer
* Eliminating single points of failure
* Production performance optimization

---

## 🧠 Key Learnings (Very Important)

* Load balancers fail **loudly but clearly**
* Nginx error pages often mean backend issues
* `upstream` blocks must live inside `http {}`
* `proxy_pass` must reference the upstream name
* Always verify backend reachability first
* App servers and LBR run **different services**
* `nginx -t` prevents broken deployments

---

## ✅ Outcome

✔ Nginx installed on LBR
✔ Load balancing configured correctly
✔ All App Servers added to upstream pool
✔ Traffic distributed successfully
✔ High-availability setup completed

---
<img width="472" height="275" alt="image" src="https://github.com/user-attachments/assets/c6295b4e-b8e2-49c3-af5a-c3e296722202" />

