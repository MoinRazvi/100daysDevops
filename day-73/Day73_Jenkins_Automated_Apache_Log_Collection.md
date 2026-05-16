# 🚀 Day 73 – Jenkins Automated Apache Log Collection & Troubleshooting

## 🎯 Objective

The xFusionCorp DevOps team needed a temporary centralized logging solution before implementing a complete logging stack.

The task involved:

* Creating a Jenkins automation job
* Scheduling periodic builds every 9 minutes
* Copying Apache logs from App Server 3
* Storing logs on the Storage Server

---

# 🧠 Why This Task Matters

This task demonstrates real-world DevOps operational automation:

* Jenkins scheduled jobs
* Remote log collection
* SCP/SSH automation
* Infrastructure troubleshooting
* Centralized log management preparation

---

# 🧱 Architecture Workflow

```text id="d73flow1"
App Server 3 (stapp03)
        ↓
Apache Logs
(access_log / error_log)
        ↓
Jenkins Automation Job
        ↓
Temporary Storage on Jenkins
        ↓
Storage Server (ststor01)
        ↓
Centralized Logs
```

---

# 🛠️ Environment Details

| Component        | Details        |
| ---------------- | -------------- |
| App Server       | stapp03        |
| App User         | banner         |
| App Password     | BigGr33n       |
| Storage Server   | ststor01       |
| Storage User     | natasha        |
| Storage Password | Bl@kW          |
| Source Logs      | /var/log/httpd |
| Destination      | /usr/src/data  |

---

# 🛠️ Jenkins Job Configuration

---

## 🔹 Job Name

```text id="d73job1"
copy-logs
```

---

## 🔹 Build Trigger

Enabled:

```text id="d73trigger1"
Build periodically
```

Cron Expression:

```text id="d73cron1"
*/9 * * * *
```

✔ Runs every 9 minutes

---

## 🔹 Execute Shell Script

```bash id="d73shell1"
sshpass -p 'BigGr33n' scp -o StrictHostKeyChecking=no \
banner@stapp03:/var/log/httpd/access_log \
/tmp/access_log

sshpass -p 'BigGr33n' scp -o StrictHostKeyChecking=no \
banner@stapp03:/var/log/httpd/error_log \
/tmp/error_log

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no \
/tmp/access_log \
natasha@ststor01:/home/natasha/access_log

sshpass -p 'Bl@kW' scp -o StrictHostKeyChecking=no \
/tmp/error_log \
natasha@ststor01:/home/natasha/error_log

sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 \
"sudo mv /home/natasha/access_log /usr/src/data/access_log && \
 sudo mv /home/natasha/error_log /usr/src/data/error_log"
```

---

# 🛠️ Additional Setup

---

## Install sshpass on Jenkins Server

```bash id="d73setup1"
apt install sshpass -y
```

---

## Create Destination Directory

```bash id="d73setup2"
ssh natasha@ststor01
mkdir -p /usr/src/data
```

---

# 🧪 Verification Commands

---

## Verify Jenkins Build

Expected:

```text id="d73verify1"
Finished: SUCCESS
```

---

## Verify Logs on Storage Server

```bash id="d73verify2"
ssh natasha@ststor01
```

```bash id="d73verify3"
ls -l /usr/src/data
```

Expected:

```text id="d73verify4"
access_log
error_log
```

---

# ❌ Errors Faced & Troubleshooting

---

# 🔴 Issue 1 — Host Key Verification Failed

### Error

```text id="d73err1"
Host key verification failed
```

### Root Cause

SSH connection required first-time host acceptance.

### Fix

Added:

```bash id="d73fix1"
-o StrictHostKeyChecking=no
```

---

# 🔴 Issue 2 — Permission Denied (Authentication Failure)

### Error

```text id="d73err2"
Permission denied (publickey,password)
```

### Root Cause

Jenkins cannot provide interactive passwords.

### Fix

Used:

```bash id="d73fix2"
sshpass
```

---

# 🔴 Issue 3 — SCP Destination Directory Error

### Error

```text id="d73err3"
No such file or directory
```

### Root Cause

Destination directory missing on Jenkins server.

### Fix

Created directory:

```bash id="d73fix3"
mkdir -p /usr/src/devops
```

---

# 🔴 Issue 4 — SCP Direction Confusion

### Problem

Initial SCP command copied files incorrectly because SCP executes from Jenkins server context.

### Learning

Always understand:

```text id="d73learn1"
scp source destination
```

from the execution server perspective.

---

# 🔴 Issue 5 — Permission Denied on Storage Server

### Error

```text id="d73err4"
Permission denied
```

### Root Cause

`natasha` lacked write permissions directly on:

```text id="d73err5"
/usr/src/data
```

### Fix

Used intermediate transfer:

```text id="d73fix4"
Transfer → Home Directory → sudo mv
```

---

# 🔍 How Final Solution Works

```text id="d73flow2"
stapp03
   ↓
Copy Logs Using SCP
   ↓
Jenkins Temporary Files (/tmp)
   ↓
Transfer to ststor01 Home Directory
   ↓
sudo mv to /usr/src/data
```

---

# 📌 Key Concepts Covered

* Jenkins scheduled jobs
* SCP automation
* SSH authentication
* sshpass usage
* Cron scheduling
* Linux permissions
* Log management
* Infrastructure troubleshooting

---

# 🧠 Key Learnings

* Automation debugging happens layer by layer
* SSH automation often fails due to authentication
* SCP direction matters
* Linux permissions are critical in automation
* Jenkins jobs run from Jenkins server context

---

# ✅ Final Status

✔ Jenkins job created
✔ Periodic trigger configured
✔ Apache logs copied successfully
✔ Logs stored on storage server
✔ All authentication & permission issues resolved

---
