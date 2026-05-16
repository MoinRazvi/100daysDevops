# 🚀 Day 72 – Jenkins Parameterized Build Job

## 🎯 Objective

The Nautilus DevOps team tested Jenkins parameterized builds to help a new DevOps Engineer understand dynamic job execution.

The task involved:

* Creating a parameterized Jenkins job
* Adding string & choice parameters
* Executing shell commands using parameters
* Running builds with custom values

---

# 🧠 Why This Task Matters

Parameterized builds are foundational in CI/CD automation.

They allow:

* Dynamic inputs
* Reusable jobs
* Environment-based deployments
* Flexible automation workflows

---

# 🧱 Workflow Overview

```text id="d72pflow1"
User Inputs Parameters
        ↓
Jenkins Job Receives Values
        ↓
Shell Command Executes
        ↓
Build Output Generated
```

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

```text id="d72pnav1"
Dashboard
   ↓
New Item
```

Enter:

```text id="d72pjob1"
parameterized-job
```

Select:

```text id="d72pjob2"
Freestyle Project
```

Click:

```text id="d72pjob3"
OK
```

---

# 🔹 Step 3: Configure Build Parameters

Under:

```text id="d72pnav2"
General
```

Enable:

```text id="d72pparam1"
This project is parameterized
```

---

# 🔹 Step 4: Add String Parameter

Add:

```text id="d72pparam2"
String Parameter
```

Configure:

| Field         | Value |
| ------------- | ----- |
| Name          | Stage |
| Default Value | Build |

---

# 🔹 Step 5: Add Choice Parameter

Add:

```text id="d72pparam3"
Choice Parameter
```

Configure:

| Field | Value |
| ----- | ----- |
| Name  | env   |

Choices:

```text id="d72pparam4"
Development
Staging
Production
```

---

# 🔹 Step 6: Configure Build Step

Navigate:

```text id="d72pnav3"
Build Steps
   ↓
Execute Shell
```

Add:

```bash id="d72pshell1"
echo "Stage: $Stage"
echo "Environment: $env"
```

---

# 🔹 Step 7: Save Job

Click:

```text id="d72psave1"
Save
```

---

# 🔹 Step 8: Build Job

Click:

```text id="d72pbuild1"
Build with Parameters
```

Use:

| Parameter | Value   |
| --------- | ------- |
| Stage     | Build   |
| env       | Staging |

Click:

```text id="d72pbuild2"
Build
```

---

# 🔹 Step 9: Verify Console Output

Open:

```text id="d72pverify1"
Console Output
```

Expected:

```text id="d72pverify2"
Stage: Build
Environment: Staging
```

---

# 🔍 How Parameterized Builds Work

```text id="d72pflow2"
User Selects Parameters
          ↓
Jenkins Stores Variables
          ↓
Shell Script Uses Variables
          ↓
Dynamic Build Execution
```

---

# 📌 Key Concepts Covered

* Jenkins parameterized builds
* String parameters
* Choice parameters
* Shell scripting
* Dynamic CI/CD workflows

---

# 🧠 Key Learnings

* Parameters make Jenkins jobs reusable
* Choice parameters reduce human errors
* Dynamic builds improve CI/CD flexibility
* Variables can control deployments and workflows

---

# ❌ Common Mistakes Avoided

* ❌ Forgetting parameterized option
* ❌ Wrong parameter names
* ❌ Incorrect shell variable usage
* ❌ Missing build execution verification

---

# 🛠️ Important Jenkins Concepts

## String Parameter

```text id="d72pconcept1"
Stage
```

Dynamic text input.

---

## Choice Parameter

```text id="d72pconcept2"
env
```

Predefined environment selection.

---

## Shell Variables

```bash id="d72pconcept3"
$Stage
$env
```

Used during build execution.

---

# ✅ Final Status

✔ Parameterized Jenkins job created
✔ String parameter configured
✔ Choice parameter configured
✔ Shell script executed successfully
✔ Build verified with Staging environment

---
