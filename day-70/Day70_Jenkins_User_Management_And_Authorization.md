# 🚀 Day 70 – Jenkins User Management & Authorization Strategy

## 🎯 Objective

The Nautilus DevOps team integrated Jenkins user access control into their CI/CD platform.

The task involved:

* Creating a new Jenkins user
* Configuring authorization strategy
* Restricting anonymous access
* Applying job-level permissions

---

# 🧠 Why This Task Matters

CI/CD security is extremely important.

This task introduces:

* User management
* Role-based permissions
* Jenkins authorization strategies
* Principle of least privilege

---

# 🧱 Authorization Workflow

```text id="d70flow1"
Admin User
    ↓
Create Jenkins User
    ↓
Configure Matrix Authorization
    ↓
Restrict Anonymous Access
    ↓
Grant Job Read Permissions
```

---

# 🛠️ Implementation Steps

---

# 🔹 Step 1: Access Jenkins UI

Click the **Jenkins** button in the lab.

Login using:

| Field    | Value    |
| -------- | -------- |
| Username | admin    |
| Password | Adm!n321 |

---

# 🔹 Step 2: Install Required Plugin

Navigate:

```text id="d70nav1"
Manage Jenkins
   ↓
Plugins
```

Search and install:

```text id="d70plugin1"
Matrix Authorization Strategy Plugin
```

If prompted:

✔ Select:

```text id="d70plugin2"
Restart Jenkins when installation is complete and no jobs are running
```

Wait until Jenkins login page appears again.

---

# 🔹 Step 3: Create Jenkins User

Navigate:

```text id="d70nav2"
Manage Jenkins
   ↓
Security
   ↓
Users
   ↓
Create User
```

Create:

| Field            | Value      |
| ---------------- | ---------- |
| Username         | yousuf     |
| Password         | 8FmzjvFU6S |
| Confirm Password | 8FmzjvFU6S |
| Full Name        | Yousuf     |

Save user.

---

# 🔹 Step 4: Configure Authorization Strategy

Navigate:

```text id="d70nav3"
Manage Jenkins
   ↓
Security
```

Under Authorization:

Select:

```text id="d70auth1"
Project-based Matrix Authorization Strategy
```

---

# 🔹 Step 5: Configure Global Permissions

Add user:

```text id="d70user1"
yousuf
```

Grant ONLY:

✔ Overall → Read

---

# 🔹 Step 6: Remove Anonymous Permissions

Locate:

```text id="d70anon1"
Anonymous
```

Remove ALL permissions.

---

# 🔹 Step 7: Verify Admin Permissions

Ensure:

```text id="d70admin1"
admin
```

retains:

✔ Overall → Administer

---

# 🔹 Step 8: Configure Job-Level Permissions

Open existing Jenkins job.

Navigate:

```text id="d70nav4"
Job
  ↓
Configure
```

Enable:

```text id="d70job1"
Enable project-based security
```

Add user:

```text id="d70user2"
yousuf
```

Grant ONLY:

✔ Job → Read

Do NOT assign:

* Build
* Configure
* Delete
* SCM
* Agent
* Workspace

Save configuration.

---

# 🧪 Verification

---

## Login as yousuf

Use:

| Field    | Value      |
| -------- | ---------- |
| Username | yousuf     |
| Password | 8FmzjvFU6S |

Expected:

✔ Can view Jenkins
✔ Can view job
✔ Cannot modify jobs
✔ Cannot administer Jenkins

---

# 🔍 Security Flow

```text id="d70flow2"
Admin User
     ↓
Matrix Authorization
     ↓
User Permissions Restricted
     ↓
Least Privilege Applied
```

---

# 📌 Key Concepts Covered

* Jenkins security
* User management
* Matrix authorization
* Role-based access control
* Job-level permissions

---

# 🧠 Key Learnings

* CI/CD security is critical
* Least privilege principle improves security
* Jenkins supports granular authorization controls
* Anonymous access should be restricted

---

# ❌ Common Mistakes Avoided

* ❌ Giving admin permissions to all users
* ❌ Leaving anonymous access enabled
* ❌ Forgetting admin retain permissions
* ❌ Granting unnecessary job permissions

---

# 🛠️ Important Jenkins Concepts

## Global Permission

```text id="d70concept1"
Overall → Read
```

Allows viewing Jenkins only.

---

## Job Permission

```text id="d70concept2"
Job → Read
```

Allows viewing job only.

---

## Admin Permission

```text id="d70concept3"
Overall → Administer
```

Full Jenkins control.

---

# ✅ Final Status

✔ Matrix Authorization Plugin installed
✔ Jenkins user created
✔ Anonymous access removed
✔ Global permissions configured
✔ Job-level permissions assigned securely

---

# 🎨 Day 70 Infographic

![Image](https://images.openai.com/static-rsc-4/t8lpqD-IavK7DCHdamx3xBqscOsj4nM6e-GWaP6ugwYC6ppk4fG0fNq-Qot4gmm0M5LDMx1ungXLaFszw5dkPuaVuWOitntP_b6IpcaaH6Of7PMjjVeSE1-YuQ4D9RrUDxb9F-r1BAqNbHYEnM2-n5VEyWCWyWyNqgjuT3UtP8R8ESPar2VhLpbzjpiquSU5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/n-aNTsEl6A8FcHfx_DFeil7qULEPeOmhVqNwtDCcdjp64e4yfRyn0HBIYSgYGvAyo6q5cid81uK--dtnBFCXcYQzCl_CjWg6kGY3IJLlSHvlK6m6Q3aef_fVumCVgd2OeQbJykmuHpb1QfaXbwMmGwX0fcHOJSeBhm3ytFTdKvxLc4tpQMrdUnAB6V5YHXF5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/VkevGqkdpdaejSMuQA0shLgp6CV-B_zweu1YUTfW5rz4Dt_6VpN5UfhLTC-272wFWLeoXumebm8BuCuryhOInPBsmieOYfSxZv5-yADxBEkgnNLYQjizbTOaMzybnS27fK5kfOMjXm6Mid-yWzobJlv8NcSD_43F6PgAo29kU_pfCMB09fqwCMiQCwnj2dX7?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/E1BUWGSFVe5BbsxOZwPnCqhh8wCbR017-FLzd0wZ-x2AzLycjVdIbRbSVuRFtGe02_M57U7SXwVCuuRemM9TPAhrcMueiQLMwy5seWbBg9iUKyWzb2gV2Sa6LTr57vojZHzbHIhCIkc-3QYwfCBsLhCZF2CoxezTcoEAxsR5qt8UAHY_0I-hfQZGmxTztOVG?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/18nZ2fTA2gMJZao9-chnl8iHKcjfVtXLM6IYXO2xPuTSKgZulRZSlopnZjhfHgspo92F5VHf3TJ-FLFoQwpwwUgQMXINSHsfdjJgP9t8oxn6Y7nNEMAI29r2Ew559e3CwO_-9yV6PRxwykHgC_2cifgkF6Uo5DfuMAzVTl2dfyGwaxjCjUWv4HG0STje6Kem?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/njqa7uNnawIFJEeq9iIkj2uUvY66mwufS5wj9KJY9WY4QFMUi4OJ0luAxVXRcoAcW26Hka6VPtevxfOm9uQR5TzwR0ZVLJu7HW1y8i49P3Nv91LOC44eyTHBKMQdZhzTtFfixvYn5vPwX6KhWJHUvBC9aAecM2Gbev5ZJyGQ7lR98uVYR_-AAhF7bP8hAYaa?purpose=fullsize)
