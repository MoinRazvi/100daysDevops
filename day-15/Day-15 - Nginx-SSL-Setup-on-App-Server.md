# 🚀 Day 15 of 100 – Nginx Installation & SSL Configuration (HTTPS Setup)

## 🎯 Objective

Install and configure **Nginx** on **App Server 2**, deploy a **self-signed SSL certificate**, serve a basic web page, and validate secure HTTPS access from a remote host.

This task represents a **real-world HTTPS web server setup**, commonly handled by DevOps and Production Support teams.

---

## 🧩 Problem Statement

The requirements were to:

1. Install and configure **Nginx** on App Server 2
2. Deploy an existing **self-signed SSL certificate and key**
3. Serve a simple `index.html` page
4. Verify HTTPS connectivity from the **jump host**

---

## 🏗️ Environment Details

| Component       | Value               |
| --------------- | ------------------- |
| Server          | App Server 2        |
| Web Server      | Nginx               |
| SSL Certificate | `/tmp/nautilus.crt` |
| SSL Key         | `/tmp/nautilus.key` |
| HTTPS Port      | 443                 |
| Test Tool       | curl                |

---

## 🛠️ Tools & Commands Used

* `yum`
* `systemctl`
* `nginx`
* `nginx -t`
* `ss`
* `mv`
* `chmod`
* `vi`
* `curl`

---

## 🧪 Step-by-Step Implementation

### ✅ Step 1: Install Nginx on App Server 2

```bash
sudo yum install -y nginx
```

Enable and start the service:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

---

### ✅ Step 2: Move SSL Certificate and Key to Secure Location

Create SSL directory:

```bash
sudo mkdir -p /etc/nginx/ssl
```

Move certificate and key:

```bash
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

Set correct permissions:

```bash
sudo chmod 600 /etc/nginx/ssl/nautilus.key
sudo chmod 644 /etc/nginx/ssl/nautilus.crt
```

---

### ✅ Step 3: Configure Nginx for HTTPS

Edit Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Add / update the **server block**:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

### ✅ Step 4: Create index.html

```bash
sudo vi /usr/share/nginx/html/index.html
```

Content:

```html
Welcome!
```

---

### ✅ Step 5: Validate Nginx Configuration

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

### ✅ Step 6: Verify Nginx Is Listening on Port 443

```bash
sudo ss -tulnp | grep 443
```

Expected:

```text
LISTEN 0 128 0.0.0.0:443 nginx
```

---

### ✅ Step 7: Firewall Check (If Required)

Ensure HTTPS port is allowed:

```bash
sudo iptables -I INPUT 2 -p tcp --dport 443 -j ACCEPT
sudo service iptables save
```

---

### ✅ Step 8: Final Testing from Jump Host

```bash
curl -Ik https://<app-server-2-ip>/
```

Expected:

* HTTPS headers returned
* Self-signed certificate warning is acceptable
* No connection refused

---

## 📌 Real-World Use Cases

* Hosting HTTPS web applications
* SSL setup for internal tools
* Secure communication in non-production environments
* Nginx as a lightweight web server or reverse proxy

---

## 🧠 Key Learnings

* HTTPS failures often come from **service, port, or firewall issues**
* SSL certificates must be stored securely
* `nginx -t` prevents broken deployments
* `ss` / `netstat` confirm real port bindings
* Self-signed certificates are common in internal setups
* Always test from a **remote host**, not just localhost

---

## ✅ Outcome

✔ Nginx installed and running on App Server 2
✔ SSL certificate and key deployed correctly
✔ HTTPS enabled on port 443
✔ Web page served successfully
✔ Verified from jump host

---

<img width="482" height="280" alt="image" src="https://github.com/user-attachments/assets/6064fbd5-bbba-4705-8bcf-fc2c766363ee" />
