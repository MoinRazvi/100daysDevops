# 🛠️ Day 09 – MariaDB Troubleshooting

## 🎯 Objective

Learn how to **troubleshoot MariaDB service failures**, analyze system logs, and restore database services — a **critical production support and DevOps skill**.

---

## 🐬 Why MariaDB Troubleshooting Matters?

MariaDB is a widely used **production-grade relational database**.
If the database service goes down:

* Applications may stop working
* Data access is disrupted
* Business operations can be impacted

Effective troubleshooting helps:

* Quickly identify root causes
* Restore service availability
* Reduce downtime in production

---

## 🛠️ Commands Used

* `systemctl`
* `journalctl`
* `mysql`
* `ps`
* `df`
* `ls`
* `cat`

---

## 🧪 Hands-on Commands Used

### ✅ Check MariaDB Service Status

```bash
systemctl status mariadb
```

Typical error observed:

```text
Job for mariadb.service failed because the control process exited with error code.
```

---

### ✅ Attempt to Start / Restart MariaDB Service

```bash
sudo systemctl start mariadb
```

```bash
sudo systemctl restart mariadb
```

---

### ✅ Analyze MariaDB Logs Using journalctl

```bash
sudo journalctl -xeu mariadb.service
```

📌 This command helps identify:

* Startup failures
* Permission issues
* Configuration errors
* Missing files or directories

---

### ✅ Check MariaDB Log Files

```bash
ls -l /var/log/mariadb/
```

```bash
cat /var/log/mariadb/mariadb.log
```

---

### ✅ Verify Disk Space (Common Failure Cause)

```bash
df -h
```

---

### ✅ Check Running MariaDB Processes

```bash
ps -ef | grep mariadb
```

---

### ✅ Verify MariaDB Configuration File

```bash
cat /etc/my.cnf
```

Or:

```bash
cat /etc/my.cnf.d/mariadb-server.cnf
```

---

### ✅ Login to MariaDB (After Service Recovery)

```bash
mysql -u root
```

---

## 📌 Real-World Use Cases

* Resolving production database outages
* Analyzing failed service startups
* Supporting application downtime incidents
* Performing root cause analysis (RCA)
* Improving monitoring and alerting

---

## 🧠 Key Learnings

* `systemctl status` is the **first troubleshooting step**
* `journalctl -xeu` provides detailed failure reasons
* Log files are essential for root cause analysis
* Disk space and permissions are common failure points
* Database recovery is a **high-priority DevOps task**

