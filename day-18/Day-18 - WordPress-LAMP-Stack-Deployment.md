# 🚀 Day 18 of 100 – WordPress LAMP Stack Deployment (Apache, PHP & MariaDB)

## 🎯 Objective

Deploy the required **LAMP stack components** to host a **WordPress application** on Nautilus infrastructure, ensuring correct integration between **App Servers**, **Database Server**, shared storage, and **Load Balancer (LBR)**.

This task simulates a **real-world production deployment** involving web, application runtime, and database layers.

---

## 🧩 Problem Statement

xFusionCorp Industries planned to host a **WordPress website** on their infrastructure.

A shared directory `/var/www/html` was already mounted from a storage server across all app hosts.

The requirements were to:

1. Install Apache, PHP, and dependencies on all app servers
2. Configure Apache to listen on **port 3000**
3. Install and configure MariaDB on the DB server
4. Create a database and user with full permissions
5. Validate application-to-database connectivity via **LBR**

---

## 🏗️ Environment Details

| Component        | Value                     |
| ---------------- | ------------------------- |
| App Servers      | stapp01, stapp02, stapp03 |
| Web Server       | Apache (`httpd`)          |
| Apache Port      | 3000                      |
| Runtime          | PHP                       |
| DB Server        | MariaDB                   |
| Database         | `kodekloud_db7`           |
| DB User          | `kodekloud_rin`           |
| Password         | `TmPcZjtRQx`              |
| Shared Directory | `/var/www/html`           |
| Access Point     | LBR                       |

---

## 🛠️ Tools & Packages Used

* `httpd`
* `php`
* `php-mysqlnd`
* `mariadb-server`
* `systemctl`
* `mysql`
* `vi`
* `curl`
* `ss`

---

## 🧪 Step-by-Step Implementation

---

### ✅ Step 1: Install Apache, PHP & Dependencies (All App Servers)

Run on **each app server**:

```bash
sudo yum install -y httpd php php-mysqlnd
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

### ✅ Step 2: Configure Apache to Listen on Port 3000

Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Update:

```apache
Listen 3000
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

Verify:

```bash
sudo ss -tulnp | grep 3000
```

---

### ✅ Step 3: Install and Start MariaDB (DB Server)

```bash
sudo yum install -y mariadb-server
```

Enable and start:

```bash
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

Verify:

```bash
sudo systemctl status mariadb
```

---

### ✅ Step 4: Access MariaDB (Important Authentication Detail)

❌ These commands failed:

```bash
mysql -u root
mysql -u peter
```

Error:

```text
ERROR 1698 (28000): Access denied for user
```

✅ Correct method (socket authentication):

```bash
sudo mysql
```

This logs in as **MariaDB root** without password.

---

### ✅ Step 5: Create Database and User

Inside MariaDB shell:

```sql
CREATE DATABASE kodekloud_db7;
```

```sql
CREATE USER 'kodekloud_rin'@'%' IDENTIFIED BY 'TmPcZjtRQx';
```

```sql
GRANT ALL PRIVILEGES ON kodekloud_db7.* TO 'kodekloud_rin'@'%';
FLUSH PRIVILEGES;
```

Exit:

```sql
EXIT;
```

---

### ✅ Step 6: Final Application Validation via LBR

Access the application using the **LBR App button**.

Expected message:

```text
App is able to connect to the database using user kodekloud_rin
```

This confirms:

* Apache + PHP working
* Shared storage mounted correctly
* Database connectivity successful
* Load balancer routing functional

---

## 🚧 Issues Faced & Resolutions

### ❌ Issue 1: MariaDB root login denied

**Cause:**
MariaDB configured with **unix_socket authentication**

**Fix:**
Logged in using:

```bash
sudo mysql
```

---

### ❌ Issue 2: Apache not reachable initially

**Cause:**
Apache listening on default port

**Fix:**
Configured:

```apache
Listen 3000
```

---

### ❌ Issue 3: Database connectivity failure

**Cause:**
Missing privileges

**Fix:**
Granted full privileges and flushed permissions

---

## 📌 Real-World Use Cases

* WordPress and CMS deployments
* LAMP stack setup
* Shared storage for multi-node apps
* Secure database provisioning
* Load-balanced application hosting

---

## 🧠 Key Learnings

* MariaDB may use socket-based authentication by default
* `sudo mysql` is the correct admin access method in such setups
* Apache port configuration must align with LBR routing
* Shared storage simplifies multi-app deployments
* End-to-end validation is essential for full-stack readiness

---

## ✅ Outcome

✔ Apache & PHP installed on all app servers
✔ Apache configured on port 3000
✔ MariaDB installed and running
✔ Database and user created with proper access
✔ Application successfully connected to database
✔ Website accessible via LBR

---

<img width="479" height="274" alt="image" src="https://github.com/user-attachments/assets/dbd1bf26-ebd0-4b58-9801-230096d86e26" />

