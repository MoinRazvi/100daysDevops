## 🚀 Day 24 – Git Branch Creation with Permission Handling

## 🎯 Objective

Create a **new Git branch** named `xfusioncorp_ecommerce` from the `master` branch in an existing repository **without making any code changes**, while handling **Git ownership and permission constraints** on a shared server.

This task reflects a **real-world DevOps responsibility** where repository access is controlled and changes must be minimal and intentional.

---

## 🧠 Why This Task Matters

Branching allows teams to:

* Isolate feature development
* Prepare releases safely
* Enable parallel workflows without touching production code

On shared servers, DevOps engineers must also understand:

* Git security restrictions
* Ownership and permission boundaries
* Safe ways to perform Git operations

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Repository Path**: `/usr/src/kodekloudrepos/ecommerce`
* **Base Branch**: `master`
* **New Branch**: `xfusioncorp_ecommerce`

---

## 🛠️ Commands Used

* `git branch`
* `git config`
* `git status`
* `sudo`

---

## 🧪 Hands-on Implementation

### ✅ Step 1: Login to Storage Server

```bash
ssh natasha@ststor01
```

---

### ✅ Step 2: Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/ecommerce
```

---

### ⚠️ Issue Encountered: Dubious Ownership Warning

When attempting to run Git commands:

```bash
git branch
```

Error received:

```text
fatal: detected dubious ownership in repository
```

---

### ✅ Step 3: Mark Repository as Safe (Git Security Feature)

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/ecommerce
```

> ✔ This does **not** modify code or permissions
> ✔ It only informs Git that the directory is trusted

---

### ⚠️ Issue Encountered: Permission Denied on Branch Creation

Attempting to create the branch as `natasha`:

```bash
git branch xfusioncorp_ecommerce master
```

Error:

```text
cannot lock ref 'refs/heads/xfusioncorp_ecommerce'
Permission denied
```

---

### ✅ Step 4: Create Branch Using sudo (Correct & Lab-Safe)

```bash
sudo git branch xfusioncorp_ecommerce master
```

> ✔ No files were modified
> ✔ No commits were created
> ✔ Only Git metadata was updated
> ✔ Fully compliant with lab rules

---

### ✅ Step 5: Verify Branch Creation

```bash
git branch
```

Expected output:

```text
* master
  xfusioncorp_ecommerce
```

---

### ✅ Step 6: Confirm No Code Changes

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

## ❌ What Was Avoided (Important)

* ❌ No file edits
* ❌ No commits
* ❌ No permission or ownership changes
* ❌ No repository reinitialization

---

## 📌 Real-World Use Cases

* Managing Git repositories on shared servers
* Creating release or feature branches safely
* Handling Git security checks in enterprise environments
* Performing DevOps tasks without touching application code

---

## 🧠 Key Learnings

* Git enforces ownership checks for security reasons
* `safe.directory` resolves trust warnings without risk
* Branch creation does **not** require checkout
* `sudo git branch` is acceptable when repo ownership differs
* DevOps work often involves **working within constraints**

---

## ✅ Final Status

✔ Branch `xfusioncorp_ecommerce` created successfully
✔ Based on `master` branch
✔ No code changes made
✔ Lab constraints fully respected
