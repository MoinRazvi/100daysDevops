# 🛠️ Website Media Backup Automation – Final Solution

## 🎯 Objective

Create a **bash script** to back up website media files from **App Server 1** and securely copy the backup to the **Nautilus Backup Server**, ensuring:

* No password prompt
* Correct user-to-user SSH authentication
* No use of `sudo` inside the script
* Script is executable by the application server user

---

## 🏗️ Environment Details

### App Server 1

* User: `tony`
* Media directory: `/var/www/html/media`
* Script location: `/scripts`
* Local backup location: `/backup`

### Nautilus Backup Server

* IP: `172.16.238.16`
* User: `clint`
* Remote backup location: `/backup`

---

## 📦 Prerequisite (Outside the Script)

Install `zip` package on **App Server 1**:

```bash
sudo yum install -y zip
```

> ⚠️ As per requirement, this is done **outside** the script.

---

## 🔐 SSH Configuration (Critical Requirement)

Password-less SSH must be configured from:

```
tony (App Server 1)  ➜  clint (Backup Server)
```

### Generate SSH key (as tony)

```bash
su - tony
ssh-keygen -t rsa -b 4096
```

### Copy public key to Backup Server

```bash
ssh-copy-id clint@172.16.238.16
```

### Verify (must NOT ask for password)

```bash
ssh clint@172.16.238.16
```

---

## 📜 Final Script: `media_backup.sh`

**Location:** `/scripts/media_backup.sh`

```bash
#!/bin/bash

# Variables
SOURCE_DIR="/var/www/html/media"
BACKUP_NAME="xfusioncorp_media.zip"
LOCAL_BACKUP_DIR="/backup"
REMOTE_USER="clint"
REMOTE_HOST="172.16.238.16"
REMOTE_BACKUP_DIR="/backup"

# Create zip archive of media directory
zip -r ${LOCAL_BACKUP_DIR}/${BACKUP_NAME} ${SOURCE_DIR}

# Copy archive to Nautilus Backup Server
scp ${LOCAL_BACKUP_DIR}/${BACKUP_NAME} \
${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP_DIR}
```

---

## 🔑 Permissions & Ownership

Ensure the script can be executed by `tony`:

```bash
chmod 755 /scripts/media_backup.sh
chown tony:tony /scripts/media_backup.sh
```

Verify:

```bash
ls -l /scripts/media_backup.sh
```

Expected:

```text
-rwxr-xr-x 1 tony tony media_backup.sh
```

---

## ▶️ Execute & Verify

### Run the script

```bash
/scripts/media_backup.sh
```

### Verify on App Server 1

```bash
ls -l /backup/xfusioncorp_media.zip
```

### Verify on Nautilus Backup Server

```bash
ls -l /backup/xfusioncorp_media.zip
```

---

## 🧠 Key Learnings (Very Important)

* SSH authentication is **user-specific** (`tony → clint`)
* `scp` uses SSH internally — password-less SSH is mandatory
* Correct remote user is **critical** (wrong user = guaranteed failure)
* Script permissions and directory permissions both matter
* No `sudo` should ever be used inside automation scripts
* Always test `ssh` and `scp` manually before relying on scripts
* This mirrors **real production DevOps practices**

---

## ✅ Final Checklist

✔ Script located in `/scripts`
✔ Backup created in `/backup` locally
✔ Backup copied to `/backup` on Nautilus Backup Server
✔ No password prompt during execution
✔ No `sudo` used inside script
✔ Executed successfully by `tony`


