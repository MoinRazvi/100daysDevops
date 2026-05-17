# 🚀 Day 75 – Jenkins SSH Build Agents & Matrix Authorization Strategy

## 🎯 Objective

The xFusionCorp DevOps team configured all application servers as Jenkins SSH build agents (slave nodes) to support distributed CI/CD execution.

Additionally, Jenkins authorization strategy plugins were configured for future role-based access management.

---

# 🧠 Why This Task Matters

This task introduced core Jenkins infrastructure concepts:

* Distributed Jenkins architecture
* SSH-based build agents
* Node labels
* Remote execution
* Matrix-based authorization

---

# 🧱 Jenkins Distributed Architecture

```text id="d75repo1"
                    Jenkins Master
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   App_server_1      App_server_2      App_server_3
      stapp01           stapp02           stapp03
```

---

# 🛠️ Environment Details

| Node Name    | Server  | User   | Remote Root Directory | Label   |
| ------------ | ------- | ------ | --------------------- | ------- |
| App_server_1 | stapp01 | tony   | /home/tony/jenkins    | stapp01 |
| App_server_2 | stapp02 | steve  | /home/steve/jenkins   | stapp02 |
| App_server_3 | stapp03 | banner | /home/banner/jenkins  | stapp03 |

---

# 🛠️ Plugins Installed

| Plugin                        | Purpose                         |
| ----------------------------- | ------------------------------- |
| SSH Build Agents              | Jenkins SSH slave/agent support |
| SSH Credentials               | SSH authentication              |
| Credentials                   | Secure credential management    |
| Matrix Authorization Strategy | Project-based authorization     |

---

# 🛠️ Java Installation & Upgrade

The Jenkins agents initially had older Java versions.

---

## 🔹 Initial Java Version

```bash id="d75repo2"
java -version
```

Output:

```text id="d75repo3"
openjdk version "11"
```

---

## 🔹 Installed Java 21

```bash id="d75repo4"
yum install java-21-openjdk -y
```

---

## 🔹 Updated Active Java Version

```bash id="d75repo5"
alternatives --config java
```

Selected Java 21.

---

## 🔹 Verified Java Version

```bash id="d75repo6"
java -version
```

Expected:

```text id="d75repo7"
openjdk version "21"
```

---

# 🛠️ Jenkins Credentials Configuration

---

## 🔹 Credential Type

```text id="d75repo8"
Username with password
```

---

## 🔹 Scope

```text id="d75repo9"
Global (Jenkins, nodes, items, all child items, etc)
```

---

## 🔹 Example Credential

| Field    | Value               |
| -------- | ------------------- |
| ID       | stapp01-creds       |
| Username | tony                |
| Password | App server password |

Repeated similarly for:

* stapp02
* stapp03

---

# 🛠️ Jenkins Agent Configuration

---

# 🔹 App_server_1

| Field                 | Value              |
| --------------------- | ------------------ |
| Node Name             | App_server_1       |
| Remote Root Directory | /home/tony/jenkins |
| Label                 | stapp01            |
| Host                  | stapp01            |

---

# 🔹 App_server_2

| Field                 | Value               |
| --------------------- | ------------------- |
| Node Name             | App_server_2        |
| Remote Root Directory | /home/steve/jenkins |
| Label                 | stapp02             |
| Host                  | stapp02             |

---

# 🔹 App_server_3

| Field                 | Value                |
| --------------------- | -------------------- |
| Node Name             | App_server_3         |
| Remote Root Directory | /home/banner/jenkins |
| Label                 | stapp03              |
| Host                  | stapp03              |

---

# 🛠️ Required Directories Created

```bash id="d75repo10"
mkdir -p /home/tony/jenkins
mkdir -p /home/steve/jenkins
mkdir -p /home/banner/jenkins
```

---

# 🧪 Agent Verification

Navigate:

```text id="d75repo11"
Manage Jenkins
   ↓
Nodes
```

Expected:

```text id="d75repo12"
App_server_1 → Online
App_server_2 → Online
App_server_3 → Online
```

---

# 🛠️ Testing Node-Specific Builds

Jobs were restricted using labels.

---

## Example

Under job configuration:

```text id="d75repo13"
Restrict where this project can be run
```

Used labels:

```text id="d75repo14"
stapp01
stapp02
stapp03
```

---

# 🔍 How Jenkins Distributed Builds Work

```text id="d75repo15"
Jenkins Master
      ↓
SSH Connection
      ↓
Agent.jar launched remotely
      ↓
Build executed on target node
```

---

# ❌ Issues Faced & Troubleshooting

---

# 🔴 Issue 1 — SSH Build Agents Plugin Failure

### Problem

Plugin dependencies showed:

```text id="d75repo16"
Failure - Details
```

### Root Cause

Jenkins restart pending.

### Fix

Restarted Jenkins:

```bash id="d75repo17"
service jenkins restart
```

---

# 🔴 Issue 2 — Wrong Credential Type Selected

### Problem

Initially selected:

```text id="d75repo18"
SSH Username with private key
```

### Fix

Corrected to:

```text id="d75repo19"
Username with password
```

---

# 🔴 Issue 3 — Java Version Mismatch

### Problem

Agents still showed:

```text id="d75repo20"
Java 11
```

after installing Java 21.

### Fix

Updated alternatives:

```bash id="d75repo21"
alternatives --config java
```

---

# 🔴 Issue 4 — Unable to Build Nodes Individually

### Problem

User expected direct build buttons on node list.

### Learning

Nodes only provide executors.

### Fix

Restricted jobs using labels:

```text id="d75repo22"
Restrict where this project can be run
```

---

# 🔴 Issue 5 — Matrix Authorization Plugin Not Found

### Problem

Searching:

```text id="d75repo23"
Project-based Matrix Authorization Strategy
```

returned nothing.

### Fix

Installed:

```text id="d75repo24"
Matrix Authorization Strategy
```

plugin instead.

---

# 🛠️ Useful Commands

---

## Restart Jenkins

```bash id="d75repo25"
service jenkins restart
```

---

## Verify Jenkins Status

```bash id="d75repo26"
service jenkins status
```

---

## Verify Java Version

```bash id="d75repo27"
java -version
```

---

## Switch Java Version

```bash id="d75repo28"
alternatives --config java
```

---

## Create Jenkins Directories

```bash id="d75repo29"
mkdir -p /home/tony/jenkins
mkdir -p /home/steve/jenkins
mkdir -p /home/banner/jenkins
```

---

# 📌 Key Concepts Covered

* Jenkins SSH agents
* Distributed builds
* SSH authentication
* Jenkins credentials
* Java management
* Node labels
* Matrix authorization
* Plugin troubleshooting

---

# 🧠 Key Learnings

* Jenkins agents require compatible Java versions
* SSH-based agents need proper credentials
* Labels control distributed builds
* Plugin failures often require Jenkins restart
* Matrix authorization plugin naming can be confusing

---

# ✅ Final Status

✔ All Jenkins agents configured
✔ SSH authentication working
✔ Java upgraded successfully
✔ All nodes online
✔ Node-specific builds working
✔ Matrix Authorization plugin installed
✔ Distributed Jenkins setup completed successfully

---
