# 🚀 Day 72 – Jenkins Parameterized Automation for Remote Package Installation

## 🎯 Objective

The Nautilus DevOps team automated package installation on the Storage Server using Jenkins.

The task involved:

* Creating a Jenkins Freestyle Job
* Using build parameters
* Executing remote commands
* Installing Linux packages dynamically

---

# 🧠 Why This Task Matters

This task demonstrates real DevOps automation concepts:

* Jenkins automation
* Infrastructure management
* Remote command execution
* Parameterized CI/CD workflows

---

# 🧱 Automation Workflow

```text id="d72flow1"
Jenkins Job
    ↓
User Inputs PACKAGE Name
    ↓
SSH Connection to Storage Server
    ↓
yum Installs Package
    ↓
Package Verified
```

---

# 🛠️ Environment Details

| Component      | Value    |
| -------------- | -------- |
| Storage Server | ststor01 |
| Username       | natasha  |
| Password       | Bl@kW    |

---

# 🛠️ Implementation Steps

---

# 🔹 Step 1: Access Jenkins UI

Click the **Jenkins** button from the lab.

Login using:

| Field    | Value    |
| -------- | -------- |
| Username | admin    |
| Password | Adm!n321 |

---

# 🔹 Step 2: Create New Jenkins Job

Navigate:

```text id="d72nav1"
Dashboard
   ↓
New Item
```

Enter:

```text id="d72job1"
install-packages
```

Select:

```text id="d72job2"
Freestyle Project
```

Click:

```text id="d72job3"
OK
```

---

# 🔹 Step 3: Configure Build Parameter

Under:

```text id="d72nav2"
General
```

Enable:

```text id="d72param1"
This project is parameterized
```

Add:

```text id="d72param2"
String Parameter
```

Configure:

| Field         | Value        |
| ------------- | ------------ |
| Name          | PACKAGE      |
| Default Value | vim-enhanced |

---

# 🔹 Step 4: Install sshpass on Jenkins Server

SSH into Jenkins server:

```bash id="d72ssh1"
ssh root@jenkins
```

Install:

```bash id="d72ssh2"
apt install sshpass -y
```

---

# 🔹 Step 5: Configure Build Step

Navigate:

```text id="d72nav3"
Build Steps
   ↓
Execute Shell
```

Add:

```bash id="d72shellfinal"
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 "sudo yum install -y $PACKAGE"
```

---

# 🔹 Step 6: Save Job

Click:

```text id="d72save1"
Save
```

---

# 🔹 Step 7: Build Job

Click:

```text id="d72build1"
Build with Parameters
```

Use:

```text id="d72param3"
PACKAGE=vim-enhanced
```

Click:

```text id="d72build2"
Build
```

---

# 🔹 Step 8: Verify Build Status

Expected:

```text id="d72verify1"
SUCCESS
```

---

# 🔹 Step 9: Verify Package Installation

SSH into storage server:

```bash id="d72verify2"
ssh natasha@ststor01
```

Password:

```text id="d72verify3"
Bl@kW
```

Verify:

```bash id="d72verify4"
rpm -qa | grep vim-enhanced
```

Expected:

```text id="d72verify5"
vim-enhanced installed
```

---

# 🔍 How This Automation Works

```text id="d72flow2"
User Inputs Package Name
          ↓
Jenkins Receives Parameter
          ↓
Execute Shell Runs
          ↓
SSH to Storage Server
          ↓
yum Installs Package
```

---

# 📌 Key Concepts Covered

* Jenkins Freestyle Jobs
* Parameterized builds
* SSH automation
* Linux package management
* Infrastructure automation

---

# 🧠 Key Learnings

* Jenkins can automate infrastructure tasks
* Parameters make automation reusable
* SSH-based automation is widely used in DevOps
* Small Jenkins jobs can eliminate repetitive admin tasks

---

# ❌ Common Mistakes Avoided

* ❌ Missing build parameters
* ❌ Incorrect SSH credentials
* ❌ Missing sshpass package
* ❌ SSH host key verification failures

---

# 🛠️ Important Jenkins Concepts

## Parameterized Build

```text id="d72concept1"
PACKAGE
```

Allows dynamic package installation.

---

## Execute Shell Automation

```bash id="d72concept2"
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 "sudo yum install -y $PACKAGE"
```

Executes automation remotely.

---

# ✅ Final Status

✔ Jenkins job created
✔ Parameterized build configured
✔ Remote automation working
✔ Package installed successfully
✔ Infrastructure automation achieved

---

![Image](https://images.openai.com/static-rsc-4/G1ZVjqLbwP3ML8OKOu0afODO8FVtQlbHk99xcK3MbnAAoS26Ufpn9Xew1UhTTm-01XyjWoHr0raV5Pky8gwadU3PPxhsFEDzsTIjz2IYsH3V5LdXLhIA8lRIAUX0c7AKS69quwiFh77hXqi0K-KsALuxV5KLU8XXvxa9uNL12lAUb3qbQJvPFjcuHCfPEW0V?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/lJKLVbbCd2z2BKCLdnzsz-y3JrLUro9869uY5ZIdUvdSHgcKQkh_-bfW18Cz5k86Aa8bdqYj6jH81_qTsRQtW6ERmjQ1n9WfLOrGZqP1UsuOU2lZPESgo5JsAM6K3P8AkZ9MA82ophNKzVJrpfrwYGNxWuENyZsZfAM9zquF3O4Lbpumn80gtX79-zc2ty_V?purpose=fullsize)
