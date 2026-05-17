# 🚀 Day 76 – Jenkins Project-Based Authorization for Developers

## 🎯 Objective

The xFusionCorp DevOps team configured project-based access control in Jenkins for newly onboarded developers.

The task involved:

* Configuring job-level permissions
* Assigning specific access rights
* Using Project-based Matrix Authorization
* Managing secure developer access

---

# 🧠 Why This Task Matters

Access management is a critical DevOps responsibility.

This task introduces:

* RBAC concepts in Jenkins
* Job-level authorization
* Principle of least privilege
* Secure CI/CD administration

---

# 🧱 Authorization Workflow

```text id="d76flow1"
Jenkins Admin
      ↓
Project-Based Authorization
      ↓
Assign User Permissions
      ↓
Controlled Job Access
```

---

# 🛠️ Environment Details

| Component   | Details  |
| ----------- | -------- |
| Jenkins Job | Packages |
| User 1      | sam      |
| User 2      | rohan    |

---

# 🛠️ User Credentials

| User  | Password        |
| ----- | --------------- |
| sam   | sam@pass12345   |
| rohan | rohan@pass12345 |

---

# 🛠️ Required Plugin

Install:

```text id="d76plugin1"
Matrix Authorization Strategy
```

---

# 🛠️ Jenkins Configuration Steps

---

# 🔹 Step 1: Login to Jenkins

Access Jenkins UI.

Login:

| Field    | Value    |
| -------- | -------- |
| Username | admin    |
| Password | Adm!n321 |

---

# 🔹 Step 2: Verify Matrix Authorization Plugin

Navigate:

```text id="d76nav1"
Manage Jenkins
   ↓
Plugins
```

Ensure installed:

```text id="d76plugin2"
Matrix Authorization Strategy
```

Restart Jenkins if required.

---

# 🔹 Step 3: Open Existing Job

Navigate:

```text id="d76nav2"
Packages
   ↓
Configure
```

---

# 🔹 Step 4: Enable Project-Based Security

Under:

```text id="d76nav3"
General
```

Enable:

```text id="d76security1"
Enable project-based security
```

---

# 🔹 Step 5: Configure Inheritance Strategy

Select:

```text id="d76security2"
Inherit permissions from parent ACL
```

---

# 🔹 Step 6: Add User sam

Add user:

```text id="d76user1"
sam
```

Grant permissions:

| Permission |
| ---------- |
| Read       |
| Build      |
| Configure  |

---

# 🔹 Step 7: Add User rohan

Add user:

```text id="d76user2"
rohan
```

Grant permissions:

| Permission |
| ---------- |
| Read       |
| Build      |
| Cancel     |
| Configure  |
| Update     |
| Tag        |

---

# 🔹 Step 8: Save Configuration

Click:

```text id="d76save1"
Save
```

---

# 🧪 Verification

---

# 🔹 Verify User Access

Login using:

```text id="d76verify1"
sam / sam@pass12345
```

Expected:

* Can view Packages job
* Can build
* Can configure

---

# 🔹 Verify rohan Access

Login using:

```text id="d76verify2"
rohan / rohan@pass12345
```

Expected:

* Additional cancel/update/tag permissions available

---

# 🔍 How Project-Based Authorization Works

```text id="d76flow2"
Global Jenkins Security
        ↓
Job-Level Matrix Permissions
        ↓
Specific User Access
```

---

# 📌 Key Concepts Covered

* Jenkins RBAC
* Project-based authorization
* Matrix authorization strategy
* Job-level security
* Least privilege access

---

# 🧠 Key Learnings

* Jenkins permissions can be controlled per job
* Matrix authorization provides granular access
* Different users can have different capabilities
* Project-level security improves CI/CD safety

---

# ❌ Common Issues Faced

---

# 🔴 Issue 1 — Plugin Not Found

### Problem

Searching:

```text id="d76err1"
Project-based Matrix Authorization Strategy
```

returned nothing.

### Fix

Installed:

```text id="d76fix1"
Matrix Authorization Strategy
```

plugin instead.

---

# 🔴 Issue 2 — Permissions Not Visible

### Root Cause

Project-based security not enabled.

### Fix

Enabled:

```text id="d76fix2"
Enable project-based security
```

---

# 🔴 Issue 3 — Users Unable to Access Job

### Root Cause

Missing:

```text id="d76fix3"
Read
```

permission.

### Fix

Granted:

* Read permission first
* then additional permissions

---

# 🔴 Issue 4 — Plugin Installation Failure

### Root Cause

Jenkins restart pending.

### Fix

Restarted Jenkins:

```bash id="d76fix4"
service jenkins restart
```

---

# 🛠️ Useful Navigation Paths

---

## Plugin Installation

```text id="d76nav4"
Manage Jenkins
   ↓
Plugins
```

---

## Job Security Configuration

```text id="d76nav5"
Packages
   ↓
Configure
   ↓
Enable project-based security
```

---

# 📌 Permission Matrix Summary

| User  | Permissions                                 |
| ----- | ------------------------------------------- |
| sam   | Read, Build, Configure                      |
| rohan | Read, Build, Cancel, Configure, Update, Tag |

---

# ✅ Final Status

✔ Matrix Authorization plugin installed
✔ Project-based security enabled
✔ Inheritance strategy configured
✔ sam permissions assigned correctly
✔ rohan permissions assigned correctly
✔ Jenkins job access secured successfully

---
