
# 🚀 Day 74 – Jenkins Automated Database Backup with Troubleshooting

## 🎯 Objective

The xFusionCorp DevOps team automated MySQL database backups using Jenkins.

The task involved:

* Creating a Jenkins automation job
* Generating MySQL dumps from App Server
* Transferring backups to Storage Server
* Scheduling automated backups every 10 minutes

---

# 🧠 Why This Task Matters

Database backups are one of the most critical operational tasks in production environments.

This task introduced:

* Jenkins automation
* mysqldump usage
* SCP/SSH automation
* Scheduled jobs
* Infrastructure troubleshooting

---

# 🧱 Architecture Workflow

```text id="d74repo1"
Jenkins Server
      ↓
SSH to App Server (stapp01)
      ↓
Create MySQL Backup
      ↓
Copy Backup to Jenkins
      ↓
Transfer Backup to Storage Server
      ↓
Centralized Backup Storage
```

---

# 🛠️ Environment Details

| Component        | Details                  |
| ---------------- | ------------------------ |
| App Server       | stapp01                  |
| App User         | tony                     |
| App Password     | <Correct Password>       |
| Database         | kodekloud_db01           |
| DB User          | kodekloud_roy            |
| DB Password      | asdfgdsd                 |
| Storage Server   | ststor01                 |
| Storage User     | natasha                  |
| Storage Password | Bl@kW                    |
| Backup Directory | /home/natasha/db_backups |

---

# 🛠️ Jenkins Job Configuration

---

## 🔹 Job Name

```text id="d74repo2"
database-backup
```

---

## 🔹 Build Trigger

Enabled:

```text id="d74repo3"
Build periodically
```

Cron expression:

```text id="d74repo4"
*/10 * * * *
```

✔ Runs every 10 minutes

---

# 🔹 Final Working Execute Shell Script

```bash id="d74repo5"
DATE=$(date +%F)

sshpass -p 'APP_SERVER_PASSWORD' ssh -o StrictHostKeyChecking=no \
tony@stapp01 \
"mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/db_${DATE}.sql"

sshpass -p 'APP_SERVER_PASSWORD' scp -o StrictHostKeyChecking=no \
tony@stapp01:/tmp/db_${DATE}.sql \
/tmp/db_${DATE}.sql

sshpass -p 'STORAGE_SERVER_PASSWORD' scp -o StrictHostKeyChecking=no \
/tmp/db_${DATE}.sql \
natasha@ststor01:/home/natasha/db_backups/db_${DATE}.sql
```
OR
```bash id="d74repo5" 
DATE=$(date +%F)

sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01 "mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/db_${DATE}.sql"

sshpass -p 'Ir0nM@n' scp -o StrictHostKeyChecking=no tony@stapp01:/tmp/db_${DATE}.sql /tmp/db_${DATE}.sql

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no /tmp/db_${DATE}.sql natasha@ststor01:/home/natasha/db_backups/db_${DATE}.sql
  
```
---

# 🛠️ Additional Setup

---

## Install sshpass on Jenkins Server

```bash id="d74repo6"
apt install sshpass -y
```

---

## Create Backup Directory

```bash id="d74repo7"
ssh natasha@ststor01
mkdir -p /home/natasha/db_backups
```

---

# 🧪 Verification Commands

---

## Verify Jenkins Build

Expected:

```text id="d74repo8"
Finished: SUCCESS
```

---

## Verify Backup File

```bash id="d74repo9"
ssh natasha@ststor01
```

```bash id="d74repo10"
ls -l /home/natasha/db_backups
```

Expected:

```text id="d74repo11"
db_2026-05-16.sql
```

---

# ❌ Issues Faced & Troubleshooting

---

# 🔴 Issue 1 — Host Key Verification Failed

### Error

```text id="d74repo12"
Host key verification failed
```

### Root Cause

SSH host fingerprint was not trusted.

### Fix

Added:

```bash id="d74repo13"
-o StrictHostKeyChecking=no
```

---

# 🔴 Issue 2 — mysqldump Password Syntax Error

### Wrong Command

```bash id="d74repo14"
-p asdfgdsd
```

### Problem

MySQL interpreted:

* `-p` → interactive password prompt
* `asdfgdsd` → extra argument

### Fix

Used:

```bash id="d74repo15"
-pasdfgdsd
```

---

# 🔴 Issue 3 — Remote Command Redirection Confusion

### Problem

Initially:

```bash id="d74repo16"
> /tmp/db.sql
```

executed on Jenkins server instead of remote server.

### Root Cause

Missing remote command quotes.

### Fix

Used:

```bash id="d74repo17"
"mysqldump ... > /tmp/db.sql"
```

---

# 🔴 Issue 4 — SCP Authentication Failure

### Error

```text id="d74repo18"
Permission denied
```

### Root Cause

SSH authentication failed.

### Fix

Used:

```bash id="d74repo19"
sshpass
```

for non-interactive password authentication.

---

# 🔴 Issue 5 — Incorrect App Server Password

### Error

```text id="d74repo20"
Permission denied, please try again
```

### Root Cause

Wrong password used for:

```text id="d74repo21"
tony@stapp01
```

### Fix

Updated to correct app server password.

---

# 🔍 Final Working Flow

```text id="d74repo22"
stapp01
   ↓
mysqldump creates backup
   ↓
Backup copied to Jenkins
   ↓
Backup transferred to ststor01
   ↓
Centralized backup storage
```

---

# 📌 Key Concepts Covered

* Jenkins scheduled jobs
* mysqldump
* SSH automation
* SCP automation
* sshpass
* Cron scheduling
* Backup automation
* Linux troubleshooting

---

# 🧠 Key Learnings

* Automation failures happen layer by layer
* Authentication is often the first failure point
* SSH command context matters
* mysqldump password syntax is critical
* Backup automation is essential in DevOps

---

# ✅ Final Status

✔ Jenkins job created
✔ Scheduled backup configured
✔ MySQL dump automated
✔ Backup transferred successfully
✔ Storage server backup verified
✔ All authentication & automation issues resolved

---
