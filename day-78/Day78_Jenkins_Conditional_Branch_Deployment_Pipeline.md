# 🚀 Day 78 – Jenkins Conditional Branch Deployment Pipeline Using Gitea

## 🎯 Objective

The xFusionCorp Industries development team needed a Jenkins Pipeline capable of deploying different Git branches dynamically based on a parameter.

The deployment should:

* deploy the `master` branch when `BRANCH=master`
* deploy the `feature` branch when `BRANCH=feature`

Deployment target:

* App Server 1
* Apache document root `/var/www/html`

---

# 🧠 Why This Task Matters

This task demonstrates real-world CI/CD deployment workflows:

* Parameterized Jenkins Pipelines
* Dynamic branch deployments
* Git-based CI/CD automation
* Temporary workspace deployments
* Automated website deployment

---

# 🧱 CI/CD Conditional Deployment Workflow

```text id="d78repo1"
Developer Pushes Code
        ↓
Gitea Repository (web_app)
        ↓
Parameterized Jenkins Pipeline
        ↓
BRANCH Selected
(master / feature)
        ↓
Fresh Clone in Temporary Workspace
        ↓
Deploy to Apache Document Root
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
| Jenkins Agent Label     | stapp01                  |
| Jenkins Agent Directory | /home/tony/jenkins_agent |
| Deployment Directory    | /var/www/html            |
| Apache Port             | 8080                     |

---

# 🛠️ Step 1 – Configure Jenkins Agent

Navigate:

```text id="d78repo2"
Manage Jenkins
   ↓
Nodes
   ↓
New Node
```

---

# 🔹 Node Configuration

| Field                 | Value                    |
| --------------------- | ------------------------ |
| Node Name             | App Server 1             |
| Label                 | stapp01                  |
| Remote Root Directory | /home/tony/jenkins_agent |
| Launch Method         | Launch agents via SSH    |
| Host                  | stapp01                  |
| Username              | tony                     |

---

# 🔹 Create Agent Directory

```bash id="d78repo3"
mkdir -p /home/tony/jenkins_agent
```

---

# 🛠️ Step 2 – Create Jenkins Pipeline Job

Navigate:

```text id="d78repo4"
New Item
```

---

# 🔹 Job Configuration

| Field    | Value                 |
| -------- | --------------------- |
| Job Name | datacenter-webapp-job |
| Job Type | Pipeline              |

⚠️ Important:

* Must NOT be Multibranch Pipeline

---

# 🛠️ Step 3 – Enable Parameterized Build

Under:

```text id="d78repo5"
General
```

Enable:

```text id="d78repo6"
This project is parameterized
```

---

# 🔹 Add String Parameter

| Field         | Value  |
| ------------- | ------ |
| Name          | BRANCH |
| Default Value | master |

---

# 🛠️ Step 4 – Configure Pipeline Script

Navigate:

```text id="d78repo7"
Pipeline
   ↓
Pipeline Script
```

---

# 🔹 Final Working Jenkins Pipeline

```groovy id="d78repo8"
pipeline {
    agent { label 'stapp01' }

    parameters {
        string(name: 'BRANCH', defaultValue: 'master')
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    sh """
                        rm -rf /tmp/web_app

                        git clone -b ${BRANCH} \
                        https://3000-port-zhqzvijl3omll5ri.labs.kodekloud.com/sarah/web_app.git \
                        /tmp/web_app

                        ls -la /tmp/web_app

                        echo 'Ir0nM@n' | sudo -S cp -r /tmp/web_app/* /var/www/html/

                        rm -rf /tmp/web_app
                    """
                }
            }
        }
    }
}
```

---

# 🔍 Why This Pipeline Is Better

Instead of using direct repository updates:

```bash id="d78repo9"
git pull
```

this approach:

* performs fresh clone every deployment ✅
* avoids stale repository state ✅
* supports clean branch switching ✅
* avoids checkout conflicts ✅
* cleans temporary deployment files ✅

---

# 🔍 How Deployment Works

```text id="d78repo10"
Pipeline starts
      ↓
Temporary workspace cleaned
      ↓
Selected branch cloned
      ↓
Files copied to Apache document root
      ↓
Temporary files removed
      ↓
Website updated automatically
```

---

# 🛠️ Step 5 – Build Pipeline

Click:

```text id="d78repo11"
Build with Parameters
```

---

# 🔹 Deploy Master Branch

```text id="d78repo12"
BRANCH = master
```

---

# 🔹 Deploy Feature Branch

```text id="d78repo13"
BRANCH = feature
```

---

# 🧪 Verification

Open:

```text id="d78repo14"
App Button
```

Expected:

* Website loads successfully
* Latest selected branch visible
* No `/web_app` subdirectory in URL

Correct:

```text id="d78repo15"
https://<LB-URL>
```

Wrong:

```text id="d78repo16"
https://<LB-URL>/web_app
```

---

# ❌ Issues Faced & Troubleshooting

---

# 🔴 Issue 1 — Wrong Jenkins Agent Directory

### Initial Mistake

Used:

```text id="d78repo17"
/home/sarah/jenkins_agent
```

### Root Cause

Jenkins agent connects using:

```text id="d78repo18"
tony@stapp01
```

### Fix

Updated directory:

```text id="d78repo19"
/home/tony/jenkins_agent
```

---

# 🔴 Issue 2 — Wrong Job Type Selected

### Problem

Selected:

```text id="d78repo20"
Multibranch Pipeline
```

### Fix

Created:

```text id="d78repo21"
Pipeline
```

job instead.

---

# 🔴 Issue 3 — Parameter Not Visible

### Root Cause

Parameterized build option not enabled.

### Fix

Enabled:

```text id="d78repo22"
This project is parameterized
```

---

# 🔴 Issue 4 — Branch Deployment Issues

### Root Cause

Existing repository state caused branch conflicts.

### Fix

Used fresh clone deployment:

```bash id="d78repo23"
git clone -b ${BRANCH}
```

instead of:

* git checkout
* git pull on existing repo

---

# 🔴 Issue 5 — Permission Denied During Deployment

### Root Cause

Apache document root required elevated permissions.

### Fix

Used:

```bash id="d78repo24"
echo 'Ir0nM@n' | sudo -S
```

for controlled sudo execution.

---

# 🔴 Issue 6 — Old Deployment Files Remaining

### Root Cause

Temporary deployment folder persisted between runs.

### Fix

Added cleanup:

```bash id="d78repo25"
rm -rf /tmp/web_app
```

before and after deployment.

---

# 🛠️ Useful Commands

---

## Create Agent Directory

```bash id="d78repo26"
mkdir -p /home/tony/jenkins_agent
```

---

## Clone Specific Branch

```bash id="d78repo27"
git clone -b feature <repo-url>
```

---

## Verify Apache Status

```bash id="d78repo28"
systemctl status httpd
```

---

## Verify Deployment

```bash id="d78repo29"
ls -l /var/www/html
```

---

# 📌 Key Concepts Covered

* Jenkins parameterized pipelines
* Conditional deployments
* Git branch deployments
* Temporary workspace deployments
* Jenkins SSH agents
* Declarative pipelines
* CI/CD automation

---

# 🧠 Key Learnings

* Fresh clone deployments are safer than reusing repositories
* Parameterized builds improve deployment flexibility
* Temporary deployment directories reduce conflicts
* Branch-based deployments are common in CI/CD
* Jenkins agents isolate execution environments

---

# ✅ Final Status

✔ Jenkins agent configured successfully
✔ Parameterized pipeline created
✔ BRANCH parameter working
✔ master branch deployment verified
✔ feature branch deployment verified
✔ Fresh clone deployment workflow implemented
✔ Website accessible successfully
✔ CI/CD branch deployment automated

---
