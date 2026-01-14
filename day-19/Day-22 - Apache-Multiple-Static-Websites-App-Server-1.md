# 🚀 Day 19 of 100 – Hosting Multiple Static Websites on Apache (App Server 1)

## 🎯 Objective

Prepare **App Server 1** to host **multiple static websites** using **Apache**, each accessible via a unique URL path on a custom port.

This setup reflects a **real-world staging / pre-production configuration**, where multiple applications are served from a single web server.

---

## 🧩 Problem Statement

xFusionCorp Industries plans to host two static websites (`blog` and `apps`) on **App Server 1**.

The requirements were to:

1. Install Apache on App Server 1
2. Configure Apache to listen on **port 3004**
3. Deploy two website backups from the **jump host**
4. Serve each site under a separate URL path
5. Validate access using `curl`

---

## 🏗️ Environment Details

| Component          | Value                                |
| ------------------ | ------------------------------------ |
| Server             | App Server 1                         |
| Web Server         | Apache (`httpd`)                     |
| Apache Port        | 3004                                 |
| Websites           | blog, apps                           |
| Source (Jump Host) | `/home/thor/blog`, `/home/thor/apps` |
| Target Path        | `/var/www/html/`                     |

---

## 🛠️ Tools & Commands Used

* `httpd`
* `systemctl`
* `scp`
* `vi`
* `curl`
* `ss`
* `chown`
* `chmod`
* `apachectl`

---

## 🧪 Step-by-Step Implementation

---

### ✅ Step 1: Install Apache on App Server 1

```bash
sudo yum install -y httpd
```

Enable and start Apache:

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

Verify:

```bash
sudo systemctl status httpd
```

---

### ✅ Step 2: Configure Apache to Listen on Port 3004

Edit Apache main config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Update:

```apache
Listen 3004
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

Verify port:

```bash
sudo ss -tulnp | grep 3004
```

---

### ✅ Step 3: Copy Website Backups from Jump Host

From **jump host**:

```bash
scp -r /home/thor/blog tony@stapp01:/tmp/
scp -r /home/thor/apps tony@stapp01:/tmp/
```

---

### ✅ Step 4: Deploy Websites Under Apache Document Root

On **App Server 1**:

```bash
sudo mv /tmp/blog /var/www/html/
sudo mv /tmp/apps /var/www/html/
```

Set correct ownership and permissions:

```bash
sudo chown -R apache:apache /var/www/html/blog /var/www/html/apps
sudo chmod -R 755 /var/www/html/blog /var/www/html/apps
```

---

### ✅ Step 5: Configure Apache to Serve Subdirectories

> ⚠️ **Important Note:**
> The file `multi-sites.conf` does **not exist by default**.
> Apache automatically loads **any `.conf` file** placed under `/etc/httpd/conf.d/`.

Create a new configuration file:

```bash
sudo vi /etc/httpd/conf.d/multi-sites.conf
```

Add:

```apache
<VirtualHost *:3004>
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

Verify loaded virtual hosts:

```bash
sudo apachectl -S
```

---

## 🧪 Step 6: Validation (On App Server 1)

Test blog site:

```bash
curl http://localhost:3004/blog/
```

Test apps site:

```bash
curl http://localhost:3004/apps/
```

✅ Both should return valid HTML output.

---

## 🚧 Issues Faced & Resolutions

### ❌ Issue 1: `multi-sites.conf` file not found

**Cause:**
Apache does not create custom config files automatically

**Fix:**
Manually created a new `.conf` file under `/etc/httpd/conf.d/`

---

### ❌ Issue 2: Apache not responding on custom port

**Cause:**
Apache listening on default port only

**Fix:**
Updated `Listen 3004` in `httpd.conf`

---

### ❌ Issue 3: Permission denied while accessing sites

**Cause:**
Incorrect directory ownership

**Fix:**
Assigned ownership to `apache:apache`

---

## 📌 Real-World Use Cases

* Hosting multiple static websites on one Apache server
* Path-based routing without DNS changes
* Pre-production application staging
* Shared infrastructure optimization
* Lightweight content hosting

---

## 🧠 Key Learnings

* Apache loads **all `.conf` files** from `/etc/httpd/conf.d/`
* Missing config files are **created**, not searched for
* Custom ports must be explicitly configured
* File ownership directly impacts content delivery
* `curl` is a fast and reliable validation tool

---

## ✅ Outcome

✔ Apache installed and running on App Server 1
✔ Apache configured on port 3004
✔ Two static websites deployed successfully
✔ Sites accessible via `/blog/` and `/apps/`
✔ Verified using curl

---
<img width="476" height="278" alt="image" src="https://github.com/user-attachments/assets/caaab365-b960-4139-8b85-b2ec99d2785f" />

