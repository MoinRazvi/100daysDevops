# Day-80-Jenkins-Chained-Builds-Automation

## 📌 Task Objective

Automate application deployment using **Jenkins Chained Builds**.

### Requirements:

* Create Jenkins job **xfusion-app-deployment**
* Pull latest code from Gitea repository
* Create downstream job **manage-services**
* Restart Apache only after successful deployment
* Validate application availability

---

# 🏗 Architecture

```plaintext
Gitea Repository
      ↓
xfusion-app-deployment
      ↓
Successful Build
      ↓
manage-services
      ↓
Apache Restart
      ↓
Application Live
```

---

# Step 1: Verify Repository on App Server

```bash
ssh tony@stapp01
cd /var/www/html
git status
```

---

# Step 2: Create Deployment Job

## Job Name

```plaintext
xfusion-app-deployment
```

### Type

```plaintext
Freestyle Project
```

---

## Source Code Management

```plaintext
Git
```

Repository URL:

```bash
http://gitea:3000/sarah/web.git
```

Branch:

```bash
*/master
```

---

## Build Step

```bash
cd /var/www/html && git reset --hard origin/master && git pull origin master
```

---

# Step 3: Configure Downstream Trigger

Add Post Build Action:

```plaintext
Build other projects
```

Project:

```plaintext
manage-services
```

Condition:

```plaintext
Trigger only if build is stable
```

---

# Step 4: Create Downstream Job

## Job Name

```plaintext
manage-services
```

### Type

```plaintext
Freestyle Project
```

---

## Build Trigger

```plaintext
Build after other projects are built
```

Project:

```plaintext
xfusion-app-deployment
```

Condition:

```plaintext
Stable
```

---

## Build Step

```bash
echo $STAPP01 | sudo -S systemctl restart httpd
```

---

# Issues Faced & Resolved

## Issue 1 — Exit Status 128

### Error

```bash
Exec exit status not zero. Status [128]
```

### Fix

```bash
git reset --hard origin/master
```

---

## Issue 2 — Permission Denied

### Error

```bash
cannot open '.git/FETCH_HEAD': Permission denied
```

### Fix

```bash
sudo chown -R tony:tony /var/www/html
```

---

## Issue 3 — Local Changes Conflict

### Error

```bash
Your local changes would be overwritten by merge
```

### Fix

```bash
git reset --hard origin/master
git pull origin master
```

---

## Issue 4 — sudo Password Required

### Error

```bash
sudo: a password is required
```

### Fix

Parameterized Jenkins password:

```bash
echo $STAPP01 | sudo -S systemctl restart httpd
```

---

## Issue 5 — Commands Not Executing

### Wrong

```bash
ssh tony@stapp01
cd /var/www/html
git pull origin master
```

### Correct

```bash
cd /var/www/html && git reset --hard origin/master && git pull origin master
```

---

# Final Validation

Check Apache:

```bash
systemctl status httpd
```

Check App:

```plaintext
Open App Button
```

Expected:

```plaintext
Application loads successfully
```

---

# Key Learnings

✅ Jenkins Chained Builds
✅ Downstream Trigger Automation
✅ Git Conflict Resolution
✅ Jenkins Parameterized Variables
✅ Apache Service Automation
✅ Deployment Workflow Automation

---
