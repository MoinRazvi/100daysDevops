## 🚀 Day 22 – Clone Existing Git Repository on Storage Server

## 🎯 Objective

Clone an **existing Git repository** on the **Storage Server** for the Nautilus development team, ensuring:

* Correct source and destination paths
* Correct user execution
* No changes to repository content or permissions

This reflects a **real-world DevOps task** where repositories are replicated for development, CI/CD, or auditing purposes.

---

## 🧠 Why This Task Matters

Cloning repositories on shared infrastructure is common for:

* Developer onboarding
* CI/CD pipelines
* Backup and mirroring
* Code review environments

Doing this **without modifying permissions or repository state** is critical in controlled production systems.

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Source Repository**: `/opt/ecommerce.git`
* **Destination Directory**: `/usr/src/kodekloudrepos`
* **Repository Type**: Bare repository (source)

---

## 🛠️ Commands Used

* `git clone`
* `ls`
* `cd`
* `git status`

---

## 🧪 Hands-on Implementation

> ⚠️ **Important Constraints**
>
> * Perform all actions as user `natasha`
> * Do **not** change permissions
> * Do **not** modify repository content
> * Do **not** reinitialize the repository

---

### ✅ Step 1: Login to Storage Server

```bash
ssh natasha@ststor01
```

---

### ✅ Step 2: Verify Source Repository Exists

```bash
ls -ld /opt/ecommerce.git
```

Expected:

* Directory exists
* Ends with `.git`, indicating a bare repository

---

### ✅ Step 3: Verify Destination Directory

```bash
ls -ld /usr/src/kodekloudrepos
```

> ⚠️ If the directory already exists, **do not modify it**.

---

### ✅ Step 4: Clone the Repository

```bash
cd /usr/src/kodekloudrepos
git clone /opt/ecommerce.git
```

This creates:

```text
/usr/src/kodekloudrepos/ecommerce
```

---

### ✅ Step 5: Verify the Clone

```bash
cd ecommerce
git status
```

Expected output:

```text
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

✔ This confirms the repository is **initialized and clean**.

---

## ℹ️ Important Note: “Empty Repository” Message

If Git shows:

```text
You appear to have cloned an empty repository.
```

This is **expected and correct**:

* The source repository is bare and has no commits yet
* The repository is already initialized
* **Do NOT run `git init`**

---

## ❌ What NOT to Do

* ❌ Do not run `git init`
* ❌ Do not change ownership or permissions
* ❌ Do not add or modify files
* ❌ Do not commit anything

---

## 📌 Real-World Use Cases

* Creating local working copies of central Git repos
* Preparing repositories for CI/CD jobs
* Auditing and reviewing source code
* Supporting distributed development teams

---

## 🧠 Key Learnings

* Bare repositories can be cloned like normal Git repos
* “Empty repository” does **not** mean uninitialized
* Respecting permissions is critical on shared servers
* DevOps work often requires **doing less, not more**

---

## ✅ Final Status

✔ Repository cloned successfully
✔ No permissions modified
✔ No repository content altered
✔ Task completed as `natasha`

