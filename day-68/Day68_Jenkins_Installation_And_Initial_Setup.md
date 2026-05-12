# 🚀 Day 68 – Installing & Configuring Jenkins for CI/CD

## 🎯 Objective

The DevOps team at xFusionCorp Industries initiated the setup of a CI/CD environment using Jenkins.

The task involved:

* Installing Jenkins using `apt`
* Starting Jenkins service
* Troubleshooting service startup issues
* Creating Jenkins admin user

---

# 🧠 Why This Task Matters

Jenkins is one of the most widely used CI/CD tools in DevOps.

This task introduces:

* CI/CD server setup
* Service management
* Troubleshooting startup failures
* Initial Jenkins configuration

---

# 🧱 Architecture Overview

```text id="oeb1k7"
Jump Host
    ↓ SSH
Jenkins Server
    ↓
Install Jenkins
    ↓
Start Jenkins Service
    ↓
Access Jenkins UI
    ↓
Create Admin User
```

---

# 🛠️ Implementation

---

# 🔹 Step 1: Connect to Jenkins Server

From jump host:

```bash id="jlwm68"
ssh root@jenkins
```

Password:

```text id="4t6dnm"
S3curePass
```

---

# 🔹 Step 2: Update Packages

```bash id="d5u4j7"
apt update
```

---

# 🔹 Step 3: Install Java (Required)

```bash id="0a7xoz"
apt install openjdk-17-jdk -y
```

Verify:

```bash id="z8z7vu"
java -version
```

---

# 🔹 Step 4: Add Jenkins Repository Key

```bash id="f7ovfh"
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

---

# 🔹 Step 5: Add Jenkins Repository

```bash id="vjlwm8"
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

---

# 🔹 Step 6: Update Repository

```bash id="48x9mt"
apt update
```

---

# 🔹 Step 7: Install Jenkins

```bash id="gmjlwm"
apt install jenkins -y
```

---

# 🔹 Step 8: Start Jenkins Service

```bash id="8i6hfa"
service jenkins start
```

---

# 🔹 Step 9: Verify Jenkins Status

```bash id="u5k7yy"
service jenkins status
```

Expected:

```text id="f0dzz0"
active (running)
```

---

# 🧪 Troubleshooting (If Service Fails)

---

## Check Service Status

```bash id="u2jlwm"
service jenkins status
```

---

## Check Jenkins Logs

```bash id="q3ytg7"
cat /var/log/jenkins/jenkins.log
```

---

# 🔍 Common Issue Found

Usually:

```text id="pj2ibn"
Java not installed
```

or

```text id="x4j1gu"
Port already in use
```

---

# 🌐 Access Jenkins UI

Use the **Jenkins button** in the lab environment.

OR:

```text id="3qg71l"
http://<server-ip>:8080
```

---

# 🔹 Step 10: Unlock Jenkins

Get initial admin password:

```bash id="8yjlwm"
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy password and paste into Jenkins UI.

---

# 🔹 Step 11: Install Suggested Plugins

Click:

```text id="p9h8o5"
Install Suggested Plugins
```

Wait for installation to complete.

---

# 🔹 Step 12: Create Admin User

Use:

| Field     | Value                                                                             |
| --------- | --------------------------------------------------------------------------------- |
| Username  | theadmin                                                                          |
| Password  | Adm!n321                                                                          |
| Full Name | Jim                                                                               |
| Email     | [jim@jenkins.stratos.xfusioncorp.com](mailto:jim@jenkins.stratos.xfusioncorp.com) |

---

# 🔹 Step 13: Save & Finish Setup

Click:

```text id="7pj5xb"
Save and Finish
```

Then:

```text id="mjlwm9"
Start using Jenkins
```

---

# 🛠️ Commands Used

```bash id="grfjlwm"
ssh root@jenkins

apt update

apt install openjdk-17-jdk -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

apt update

apt install jenkins -y

service jenkins start

service jenkins status

cat /var/log/jenkins/jenkins.log

cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# 🔍 How Jenkins Works

```text id="6ru6i1"
Developer Pushes Code
        ↓
Jenkins Pipeline Triggered
        ↓
Build + Test + Deploy
        ↓
Continuous Delivery
```

---

# 📌 Key Concepts Covered

* Jenkins installation
* CI/CD setup
* Service management
* Linux package management
* Troubleshooting logs
* Admin user configuration

---

# 🧠 Key Learnings

* Jenkins requires Java
* Services should always be verified after installation
* Logs are critical for troubleshooting
* Jenkins UI setup is part of deployment

---

# ❌ Common Mistakes Avoided

* ❌ Forgetting Java installation
* ❌ Not checking service logs
* ❌ Wrong repository configuration
* ❌ Ignoring service status

---

# ✅ Final Status

✔ Jenkins installed successfully
✔ Jenkins service running
✔ Admin user created
✔ Jenkins UI accessible
✔ CI/CD server ready

---

# 🚀 Why This Day Was Important

This was the beginning of real CI/CD infrastructure setup.

You covered:

* Server setup
* Package installation
* Service management
* CI/CD tooling
* Troubleshooting

---

# 🎨 Day 68 Infographic

![Image](https://images.openai.com/static-rsc-4/neiwjsbiNofOeTPCLCcTnWnR7K0pcuo0Hu9xVcIhGp5r2ewElK4RQ2gp5Wfye189G1y0erI1POI8W_tLU8Su-XEz0sXJV3u71L2_10z4ZnPsSFacwY_dah_tZkQUrZfk--vJznKMYKmSdlCYGc4dqbthcE6qXKrq7xiTLWwQMWxyBrla0gi7aXUrGQ8T9MC5?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/FAgAf122Nbtcfr1OwaDAz_MOeE_g0fG4XX99qUlQdhcldRE0Z1ozdSVhgjGdvWhm9sLqGJELKWC5D27guHG4slFswOV2p5hmelNZCds23Zm9I5R6Bqh7QYCsJC3lbTikWby_SepWh-Yw9ymdW8XBZayrQwUqMDCzYjDZGCUe_qWsQGkvr_KSY_ClgW5ugOVq?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/qEzULVkxoppkpvRvZNcX0gpgJFrKlp2SGSH8jVuSjNEqKJfuli6UWX_H4pEKaIN1tYMyOzhwEmF4WXCrJRgTs0hkXo1MDdrfMnQvFA5E2ZEf8-Jz_uqb4eUDrtRzhKTJg3ndZDW-RDCi7_KH-ZPdWQtJ-a7Pr5ddanQAFFjH1SnnvyKebeBF95NPkpsMq7Xu?purpose=fullsize)

