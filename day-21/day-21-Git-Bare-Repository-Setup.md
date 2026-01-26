## 🚀 Day 21 – Git Bare Repository Setup (Central Repository)

## 🎯 Objective

Set up a **central Git bare repository** on the **Storage Server** to support collaborative development for the Nautilus project — a **standard DevOps practice** for shared repositories and CI/CD pipelines.

---

## 🧠 Why a Bare Repository?

A **bare repository**:

* Does **not** contain a working directory
* Is used as a **central remote repository**
* Is ideal for:

  * Team collaboration
  * CI/CD pipelines
  * Controlled access to source code

This mirrors how platforms like **GitHub, GitLab, and Gitea** manage repositories internally.

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **Package Manager**: `yum`
* **Repository Path**: `/opt/cluster.git`
* **Repository Type**: Bare Git repository

---

## 🛠️ Commands Used

* `yum`
* `git`
* `mkdir`
* `cd`
* `ls`

---

## 🧪 Hands-on Implementation

### ✅ Step 1: Login to Storage Server

```bash
ssh natasha@ststor01
```

---

### ✅ Step 2: Install Git Using yum

```bash
sudo yum install -y git
```

Verify installation:

```bash
git --version
```

---

### ✅ Step 3: Create Bare Repository Directory

```bash
sudo mkdir -p /opt/cluster.git
cd /opt/cluster.git
```

---

### ✅ Step 4: Initialize Bare Git Repository

```bash
sudo git init --bare
```

---

### ✅ Step 5: Verify Repository Structure

```bash
ls -l /opt/cluster.git
```

Expected contents:

* `HEAD`
* `objects/`
* `refs/`
* `hooks/`
* `config`

✔ This confirms a **valid bare Git repository**.

---

## 📌 Real-World Use Cases

* Central Git repository for multiple developers
* Backend repository for CI/CD systems
* On-prem Git hosting
* Secure storage of source code

---

## 🧠 Key Learnings

* Bare repositories are **meant for sharing**, not development
* `git init --bare` is required for server-side repositories
* Central repositories typically reside in `/opt` or `/var`
* This setup mirrors production Git infrastructure

---

## ✅ Final Status

✔ Git installed successfully
✔ Bare repository created at `/opt/cluster.git`
✔ Repository structure verified
✔ Task completed successfully

