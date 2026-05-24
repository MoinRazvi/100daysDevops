
# 🚀 Day 79 — Jenkins Auto Deployment Using Gitea Webhooks

## **📌 Task Objective**

The goal of this task was to configure a **CI/CD deployment pipeline** where changes pushed to the Git repository automatically trigger a Jenkins job and deploy the latest application files to **App Server 1**.

The deployment required:

* Updating the `index.html` file
* Pushing changes to the **master** branch
* Triggering Jenkins automatically
* Deploying the latest code to the web server
* Deploying the **entire repository content**, not just one file

---

# **🛠️ Environment Setup**

## **Servers Used**

* **App Server 1**

  * User: `sarah`

* **Jenkins Server**

  * User: `jenkins`

* **Git Repository**

  * Repository: `web`

---

# **🎯 Task Requirements**

Update:

```html id="s8wq5m"
index.html
```

Content should be:

```html id="9r29bi"
Welcome to the xFusionCorp Industries
```

Then:

* Commit changes
* Push to `master`
* Trigger Jenkins build
* Deploy to:

```bash id="cxg8tx"
/var/www/html/
```

---

# **🔄 Initial Steps Performed**

## **Step 1: SSH into App Server**

```bash id="w7w3dc"
ssh sarah@stapp01
```

---

## **Step 2: Navigate to Repository**

```bash id="97jzks"
cd /home/sarah/web
```

---

## **Step 3: Update Application File**

```bash id="d4quf6"
vi index.html
```

Updated content:

```html id="rjv7o8"
Welcome to the xFusionCorp Industries
```

---

## **Step 4: Commit Changes**

```bash id="p8r7mz"
git add .
git commit -m "updated the sentence"
```

---

## **Step 5: Push to Master**

```bash id="nx2smz"
git push origin master
```

---

# **❌ Issues Faced During Deployment**

---

# **Issue 1: Jenkins Triggered but Deployment Failed**

### Error

```bash id="c8f8ek"
sudo: a terminal is required to read the password
```

### Root Cause

Jenkins user had no sudo privileges.

### Attempted Fix

Tried modifying sudoers.

This failed because:

* Jenkins user wasn't configured for sudo
* Passwordless sudo wasn't available

---

# **Issue 2: Local Copy to `/var/www/html` Failed**

### Error

```bash id="0r9u0l"
cp: cannot create regular file '/var/www/html/': No such file or directory
```

### Root Cause

Jenkins was trying to copy files **locally**.

But:

* `/var/www/html` exists on **App Server 1**
* Jenkins runs on a separate server

---

# **Issue 3: Deploying Only `index.html`**

Initial command:

```bash id="ih5h8f"
cp -r index.html /var/www/html/
```

Problem:

* Copies only one file
* Does not deploy full repository

Task explicitly required deploying **entire repository content**

---

# **✅ Final Working Solution**

The successful Jenkins shell command:

```bash id="z9wd4m"
scp -o StrictHostKeyChecking=no -r * sarah@stapp01:/home/sarah/web/
ssh -o StrictHostKeyChecking=no sarah@stapp01 "cp -r /home/sarah/web/* /var/www/html/"
```

---

# **💡 Solution Breakdown**

## **Command 1**

```bash id="p0m5l2"
scp -o StrictHostKeyChecking=no -r * sarah@stapp01:/home/sarah/web/
```

Purpose:

* Copies all repository files from Jenkins workspace
* Transfers them to App Server 1

---

## **Command 2**

```bash id="l4q8mu"
ssh -o StrictHostKeyChecking=no sarah@stapp01 "cp -r /home/sarah/web/* /var/www/html/"
```

Purpose:

* SSH into App Server
* Deploy files to web root

---

# **🔐 Key Configuration Requirement**

Passwordless SSH between:

**Jenkins Server → App Server 1**

Setup:

```bash id="myb8c0"
ssh-keygen -t rsa
ssh-copy-id sarah@stapp01
```

---

# **🚀 Jenkins Job Configuration**

## **Source Code Management**

Repository:

```bash id="d4c85h"
https://3000-port-xxxx.labs.kodekloud.com/sarah/web.git
```

Branch:

```bash id="z1x3u4"
*/master
```

---

## **Build Trigger**

Configured to trigger automatically on Git push.

---

## **Build Step**

Execute shell:

```bash id="xf26it"
scp -o StrictHostKeyChecking=no -r * sarah@stapp01:/home/sarah/web/
ssh -o StrictHostKeyChecking=no sarah@stapp01 "cp -r /home/sarah/web/* /var/www/html/"
```

---

# **✅ Verification**

Verify deployed content:

```bash id="e1k6mz"
cat /var/www/html/index.html
```

Output:

```html id="dh4h0x"
Welcome to the xFusionCorp Industries
```

---

# **📚 Key Learnings**

Through this task, I learned:

* Jenkins Git webhook integration
* Remote deployment using SCP
* SSH-based automated deployment
* Troubleshooting Jenkins build failures
* Difference between local vs remote deployment
* Deploying full repository content

---

# **🏆 Final Outcome**

Successfully implemented:

✅ Git push trigger
✅ Jenkins automated build
✅ Remote deployment to App Server
✅ Entire repository deployment
✅ Automated CI/CD workflow

---

# **🔥 Real-World DevOps Concepts Practiced**

* Continuous Integration
* Continuous Deployment
* Remote Server Automation
* Jenkins Pipeline Debugging
* SSH Authentication
* Deployment Troubleshooting

---

