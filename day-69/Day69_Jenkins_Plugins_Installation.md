# 🚀 Day 69 – Installing Git & GitLab Plugins in Jenkins

## 🎯 Objective

The Nautilus DevOps team prepared Jenkins for CI/CD workloads by installing essential plugins:

* Git Plugin
* GitLab Plugin

These plugins enable Jenkins to integrate with:

* Git repositories
* GitLab repositories
* CI/CD workflows

---

# 🧠 Why This Task Matters

Plugins are what make Jenkins powerful.

Without plugins:

* Jenkins cannot integrate with SCM tools
* Pipelines become limited
* Automation workflows are incomplete

---

# 🧱 Workflow Overview

```text id="d69flow1"
Developer Pushes Code
        ↓
Git / GitLab Repository
        ↓
Jenkins Plugin Integration
        ↓
Build Pipeline Triggered
        ↓
CI/CD Workflow
```

---

# 🛠️ Implementation Steps

---

# 🔹 Step 1: Access Jenkins UI

Click the **Jenkins** button from the lab environment.

OR:

```text id="d69ui1"
http://<server-ip>:8080
```

---

# 🔹 Step 2: Login to Jenkins

Use:

| Field    | Value    |
| -------- | -------- |
| Username | admin    |
| Password | Adm!n321 |

---

# 🔹 Step 3: Open Plugin Manager

Navigate:

```text id="d69nav1"
Dashboard
   ↓
Manage Jenkins
   ↓
Plugins
```

---

# 🔹 Step 4: Install Plugins

Go to:

```text id="d69nav2"
Available Plugins
```

Search and select:

✔ Git
✔ GitLab

---

# 🔹 Step 5: Install Plugins

Click:

```text id="d69btn1"
Install
```

OR

```text id="d69btn2"
Install after restart
```

---

# 🔹 Step 6: Restart Jenkins (If Required)

If prompted:

✔ Select:

```text id="d69btn3"
Restart Jenkins when installation is complete and no jobs are running
```

---

# 🔹 Step 7: Wait for Jenkins to Restart

After restart:

* Wait until login page appears again
* Login back into Jenkins

---

# 🔹 Step 8: Verify Plugins

Navigate:

```text id="d69nav3"
Manage Jenkins
   ↓
Plugins
   ↓
Installed Plugins
```

Verify:

✔ Git Plugin installed
✔ GitLab Plugin installed

---

# 🧪 Verification

Expected plugins:

```text id="d69verify1"
Git
GitLab
```

---

# 🔍 How Plugins Help Jenkins

```text id="d69flow2"
Git Repository
      ↓
Webhook Trigger
      ↓
Jenkins Job
      ↓
Automated Build/Test/Deploy
```

---

# 📌 Key Concepts Covered

* Jenkins plugin management
* Git integration
* GitLab integration
* Jenkins restart handling
* CI/CD preparation

---

# 🧠 Key Learnings

* Jenkins functionality depends heavily on plugins
* Plugins extend Jenkins capabilities
* Git/GitLab plugins are foundational for CI/CD pipelines

---

# ❌ Common Mistakes Avoided

* ❌ Forgetting Jenkins restart
* ❌ Installing wrong plugins
* ❌ Closing browser during restart
* ❌ Not verifying installed plugins

---

# ✅ Final Status

✔ Jenkins accessible
✔ Git plugin installed
✔ GitLab plugin installed
✔ Jenkins restarted successfully
✔ CI/CD integrations ready

---

# 🎨 Day 69 Infographic

![Image](https://images.openai.com/static-rsc-4/jELQL26t3Xm7K1-J6xwG4B-UTKwZivo1I_aUeqIBHu4xGexGqorAFs2achcMwOzaS4bjOq8CpfUhHdFXP_ZOn7mHApLu0jijc9gcHjky_ToMliot4A6IW095svQYIo2zbaH9KE2LJMTnTuvOwceCmcEfvi_mkyVCbiz6tjm1xmmhME8_1_42BJ70Ki5HOap5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/uMScVKe4MQRlbf8PBXrf0z22QrvgeIQqjLZ7pL8u6tPZgeYjTwdwXhTMI2ay3F-o7YiLjwFATpK5-drhkPrlpI8BQNI5eByvInp68tREGP25-JvrqJ0gwXGvVP8v45WKnS-KoxbY28h9CJbEmJsofvz1wo_0IpTDixsKVOWRP0hz1DqkS5I71JKtQ5PJU-yu?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/pAOQMwvPMQNLcEWmeozorSYHePLDDuf3BlBynGPlsI_uG08h72ZhZznhh9y1-Gges-v3-Roi_Znm9VopRoUIbjxIpigiD8KVMbo84_f9Qe02sEaPms2pJi4lThfZIPg-YJ0Zak8ihrRpgWrbg-TIdqd1Tge7CBjPppBJPgIp4R0prWBSQc61LhLuCe0T-V_x?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/3d3NUSVjR85FyAN23Ih0TRrSgsvcPm_3ItAXd0OjQY_BQdUSMQ7b4YP54HaIe09VLUw2LxFF0Prhvk_jNeNKpgm53Sadsq4LfcgR633ramWb3qFmj_IyuRk9Zi1L3Y2svN0rfXmSB8Mq0gJ9hcoWPbH0XZlsX9IwZZ0KOGFrkTe_pPcpJXyY0Qz349bPpJ89?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/weBjoCw0KACQkIiRo4EOi2ZZGCx0yuQeaHYNPpFbnJGtccfZexdVa00TRnvF9_GuS7lQNwguug9FeiZeApU20j4ebjvOsAmOCMnuMaQvnGF0cgwml2E8GLcB8vJmtYCRz2TFx5Sm-4QOP9yOWNKeBKQdJLvm_0smBRI_Sdge-VH0gpYJ-D61mDwt87zVh5ye?purpose=fullsize)
