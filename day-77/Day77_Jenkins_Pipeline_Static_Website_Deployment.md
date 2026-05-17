# 🚀 Day 77 – Jenkins Pipeline Deployment for Static Website Using Gitea

## 🎯 Objective

The xFusionCorp Industries development team required a Jenkins Pipeline to automate deployment of a static website hosted in a Gitea repository.

The deployment target was:

* App Server 1
* Apache document root `/var/www/html`
* Accessible through the Load Balancer URL

---

# 🧠 Why This Task Matters

This task demonstrates a real-world CI/CD deployment workflow using:

* Jenkins Pipeline
* Jenkins SSH agents
* Git-based deployments
* Automated website updates
* Gitea integration

---

# 🧱 CI/CD Deployment Workflow

```text id="d77repo1"
Developer (Sarah)
        ↓
Gitea Repository (web_app)
        ↓
Jenkins Pipeline
        ↓
Jenkins Agent (stapp01)
        ↓
git pull origin master
        ↓
Apache Web Server
        ↓
Accessible via Load Balancer
```

---

# 🛠️ Environment Details

| Component               | Details                  |
| ----------------------- | ------------------------ |
| Jenkins User            | admin                    |
| Jenkins Password        | Adm!n321                 |
| Gitea User              | sarah                    |
| Gitea Password          | Sarah_pass123            |
| Repository              | web_app                  |
| App Server              | stapp01                  |
| App Server User         | tony                     |
| Jenkins Agent Directory | /home/tony/jenkins_agent |
| Deployment Directory    | /var/www/html            |
| Apache Port             | 8080                     |

---

# 🛠️ Step 1 – Configure Jenkins Agent

Navigate:

```text id="d77repo2"
Manage Jenkins
   ↓
Nodes
   ↓
New Node
```

---

# 🔹 Jenkins Agent Configuration

| Field                 | Value                    |
| --------------------- | ------------------------ |
| Node Name             | App Server 1             |
| Labels                | stapp01                  |
| Remote Root Directory | /home/tony/jenkins_agent |
| Launch Method         | Launch agents via SSH    |
| Host                  | stapp01                  |
| Username              | tony                     |

---

# 🔹 Create Agent Directory

Run on App Server 1:

```bash id="d77repo3"
mkdir -p /home/tony/jenkins_agent
```

---

# 🛠️ Step 2 – Create Pipeline Job

Navigate:

```text id="d77repo4"
New Item
```

---

# 🔹 Job Configuration

| Field    | Value               |
| -------- | ------------------- |
| Job Name | nautilus-webapp-job |
| Job Type | Pipeline            |

⚠️ Important:

* Must NOT be Multibranch Pipeline

---

# 🛠️ Step 3 – Configure Pipeline Script

Navigate:

```text id="d77repo5"
Pipeline
   ↓
Pipeline Script
```

---

# 🔹 Final Working Jenkins Pipeline

```groovy id="d77repo6"
pipeline {
    agent { label 'stapp01' }

    stages {
        stage('Deploy') {
            steps {
                sh '''
                    cd /var/www/html/
                    git pull origin master
                '''
            }
        }
    }
}
```

---

# 🔍 Why This Pipeline Is Better

Instead of manually copying files:

```bash id="d77repo7"
cp -r
```

this deployment:

* pulls latest code directly from repository ✅
* keeps repository synchronized ✅
* avoids duplicate file copies ✅
* resembles real-world CI/CD deployments ✅

---

# 🔍 How Deployment Works

```text id="d77repo8"
Pipeline executes on stapp01
        ↓
Moves into deployed repository
        ↓
Runs git pull origin master
        ↓
Latest website changes deployed instantly
```

---

# 🛠️ Step 4 – Build Pipeline

Click:

```text id="d77repo9"
Build Now
```

Expected:

```text id="d77repo10"
Finished: SUCCESS
```

---

# 🧪 Verification

Open:

```text id="d77repo11"
App Button
```

Expected:

* Website loads successfully
* Latest code changes visible
* No `/web_app` subdirectory in URL

Correct:

```text id="d77repo12"
https://<LB-URL>
```

Wrong:

```text id="d77repo13"
https://<LB-URL>/web_app
```

---

# ❌ Issues Faced & Troubleshooting

---

# 🔴 Issue 1 — Wrong Jenkins Agent Directory

### Initial Mistake

Used:

```text id="d77repo14"
/home/sarah/jenkins_agent
```

### Root Cause

Jenkins SSH agent connects using:

```text id="d77repo15"
tony@stapp01
```

### Fix

Updated remote root directory:

```text id="d77repo16"
/home/tony/jenkins_agent
```

---

# 🔴 Issue 2 — Wrong Job Type Selected

### Problem

Selected:

```text id="d77repo17"
Multibranch Pipeline
```

### Fix

Created:

```text id="d77repo18"
Pipeline
```

job instead.

---

# 🔴 Issue 3 — Deployment Into Subdirectory

### Problem

Website accessible as:

```text id="d77repo19"
/web_app
```

### Root Cause

Incorrect deployment method.

### Fix

Used:

```bash id="d77repo20"
git pull origin master
```

inside deployed repository.

---

# 🔴 Issue 4 — Agent Offline

### Root Cause

Missing remote root directory.

### Fix

Created:

```bash id="d77repo21"
mkdir -p /home/tony/jenkins_agent
```

---

# 🔴 Issue 5 — Pipeline Syntax Errors

### Root Cause

Incorrect declarative syntax.

### Fix

Used proper declarative Jenkins pipeline structure.

---

# 🛠️ Useful Commands

---

## Create Agent Directory

```bash id="d77repo22"
mkdir -p /home/tony/jenkins_agent
```

---

## Verify Apache Status

```bash id="d77repo23"
systemctl status httpd
```

---

## Verify Deployment Directory

```bash id="d77repo24"
ls -l /var/www/html
```

---

## Pull Latest Changes Manually

```bash id="d77repo25"
cd /var/www/html
git pull origin master
```

---

# 📌 Key Concepts Covered

* Jenkins Pipeline
* Jenkins SSH agents
* Declarative pipelines
* CI/CD automation
* Gitea integration
* Git-based deployments
* Apache deployment

---

# 🧠 Key Learnings

* Git pull deployments are cleaner than file copying
* Jenkins agents isolate execution environments
* Correct deployment paths matter
* Declarative pipelines simplify automation
* CI/CD workflows become easier with Git integration

---

# ✅ Final Status

✔ Jenkins agent configured successfully
✔ Pipeline job created
✔ Deploy stage configured
✔ Git-based deployment working
✔ Website accessible from root URL
✔ Latest code changes deployed automatically
✔ CI/CD pipeline completed successfully

---
<img width="1147" height="860" alt="image" src="https://github.com/user-attachments/assets/44e6ce8e-293d-4cd2-b615-c0efe33c3a81" />

<img width="1529" height="860" alt="image" src="https://github.com/user-attachments/assets/3183a49b-3b06-407f-bad7-af49cd0919ba" />

