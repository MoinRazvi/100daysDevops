
# Day 81 — Jenkins Pipeline Deployment on App Server

## 📌 Objective

Build a complete Jenkins Pipeline that:

* Deploys code from Gitea repository
* Runs on Jenkins Agent (App Server 1)
* Verifies deployment using automated testing
* Ensures website accessibility

---

# Architecture

```plaintext
Developer Push
      ↓
Gitea Repository
      ↓
Jenkins Pipeline Trigger
      ↓
Deploy Stage
      ↓
Git Pull on App Server
      ↓
Test Stage
      ↓
Curl Validation
      ↓
Application Live
```

---

# Task Requirements

## Repository

```bash
sarah/web
```

---

## Jenkins Agent

| Parameter             | Value                     |
| --------------------- | ------------------------- |
| Node Name             | App Server 1              |
| Label                 | stapp01                   |
| Remote Root Directory | /home/sarah/jenkins_agent |
| Launch Method         | SSH                       |
| Username              | sarah                     |

---

## Pipeline Job

```bash
deploy-job
```

Stages:

* Deploy
* Test

---

# Step 1 — Update Website Content

SSH into App Server:

```bash
ssh sarah@stapp01
```

Go to repo:

```bash
cd /var/www/html
```

Edit:

```bash
vi index.html
```

Update content:

```html
Welcome to xFusionCorp Industries
```

Commit & Push:

```bash
git add .
git commit -m "Updated index.html"
git push origin master
```

---

# Step 2 — Verify Java 17

Check Java:

```bash
java -version
```

If missing:

```bash
sudo yum install -y java-17-openjdk
```

Verify:

```bash
java -version
```

Expected:

```plaintext
openjdk version "17"
```

---

# Step 3 — Configure Jenkins Agent

### Node Configuration

```plaintext
Manage Jenkins → Nodes → New Node
```

---

## Name

```plaintext
App Server 1
```

---

## Type

```plaintext
Permanent Agent
```

---

## Label

```plaintext
stapp01
```

---

## Remote Root Directory

```plaintext
/home/sarah/jenkins_agent
```

---

## Launch Method

```plaintext
Launch agents via SSH
```

---

## Host

```plaintext
stapp01
```

---

## Credentials

```plaintext
Username: sarah
Password: Sarah_pass123
```

---

## Host Verification

```plaintext
Non verifying Verification Strategy
```

---

# Step 4 — Create Pipeline Job

```plaintext
New Item → deploy-job → Pipeline
```

---

# Step 5 — Jenkinsfile

## Final Working Pipeline

```groovy
pipeline {
    agent { label 'stapp01' }

    stages {

        stage('Deploy') {
            steps {
                sh '''
                    cd /var/www/html
                    git reset --hard origin/master
                    git pull origin master
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    STATUS=$(curl -o /dev/null -s -w "%{http_code}" http://stlb01:8091)

                    if [ "$STATUS" -ne 200 ]; then
                        echo "Application test failed"
                        exit 1
                    else
                        echo "Application deployed successfully"
                    fi
                '''
            }
        }
    }
}
```

---

# Build Execution Flow

```plaintext
Pipeline Starts
      ↓
Deploy Stage
      ↓
Git Reset
      ↓
Git Pull
      ↓
Test Stage
      ↓
Curl Check
      ↓
Success
```

---

# Issues Faced & Solutions

---

# Error 1 — SSH Launch Method Missing

## Issue

Could not find:

```plaintext
Launch agents via SSH
```

## Solution

Installed plugin:

```plaintext
SSH Build Agents Plugin
```

Restart Jenkins.

---

# Error 2 — Agent Offline

## Fix

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

---

# Error 3 — Java Version Issue

## Fix

Install Java 17:

```bash
sudo yum install -y java-17-openjdk
```

---

# Error 4 — Permission Denied

## Error

```bash
cannot open '.git/FETCH_HEAD'
```

## Fix

```bash
sudo chown -R sarah:sarah /var/www/html
```

---

# Error 5 — Local Changes Conflict

## Error

```bash
Your local changes would be overwritten
```

## Fix

```bash
git reset --hard origin/master
```

---

# Error 6 — Curl Test Failed

## Fix

Verify Apache:

```bash
sudo systemctl status httpd
```

Restart:

```bash
sudo systemctl restart httpd
```

---

# Validation

Build pipeline:

```plaintext
Build Now
```

Expected:

---

## Deploy Stage

```plaintext
Already up to date
```

---

## Test Stage

```plaintext
Application deployed successfully
```

---

# Final Verification

Open:

```plaintext
http://stlb01:8091
```

Expected:

```plaintext
Welcome to xFusionCorp Industries
```

---

# Key Learnings

✅ Jenkins Pipeline Basics
✅ Jenkins Agent Configuration
✅ Multi-stage Pipelines
✅ Automated Deployment
✅ Automated Testing
✅ Git Repository Synchronization
✅ Curl-based Validation

---

                             
