## 🚀 Day 28 – Revert Latest Git Commit (Apps Repository)

## 🎯 Objective

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/apps` present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. The DevOps team was asked to **revert the repository HEAD to the previous commit** using a **safe and production-grade Git approach**.

---

## 🧠 Why `git revert` Is Required

* Preserves Git history
* Creates an audit-friendly rollback commit
* Safe for shared repositories
* Recommended for production systems

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Repository Path**: `/usr/src/kodekloudrepos/apps`
* **Branch**: `master`
* **Action**: Revert latest commit (HEAD)
* **Required Commit Message**: `revert apps` (all lowercase)

---

## 🛠️ Commands Used

* `git status`
* `git log`
* `git revert`
* `rm`

---

## 🧪 Initial Attempt (What Went Wrong)

### ❌ Issue Observed

After performing the revert, the lab returned the error:

```
required data not found under repo, seems like git revert was not done properly
```

---

### 🔍 Root Cause Analysis

Running `git status` showed:

```text
Untracked files:
    apps.txt
```

This means:

* The repository was **not clean**
* `git revert` was executed while **untracked files existed**
* KodeKloud lab validation detected **extra files**, causing failure

⚠️ **Important**:
Even though Git allows revert on a dirty working tree, **KodeKloud validation is stricter** and expects an exact repository state.

---

## ✅ Correct Fix (Step-by-Step)

### ✅ Step 1: Abort Revert (if in progress)

```bash
git revert --abort
```

---

### ✅ Step 2: Clean the Working Tree

Check status:

```bash
git status
```

Remove untracked file:

```bash
rm -f apps.txt
```

Verify clean state:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

### ✅ Step 3: Identify Commits

```bash
git log --oneline --decorate -2
```

Example:

```text
abc9876 (HEAD -> master) recent changes
def1234 initial commit
```

---

### ✅ Step 4: Revert Latest Commit (Properly)

```bash
git revert HEAD
```

When editor opens, **replace the commit message with exactly**:

```
revert apps
```

Save and exit.

---

### ✅ Step 5: Verify Revert

```bash
git log --oneline -2
```

Expected:

```text
xyz5555 (HEAD -> master) revert apps
def1234 initial commit
```

---

### ✅ Step 6: Final Validation

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

## ❌ What Was Avoided

* ❌ `git reset --hard`
* ❌ Force push
* ❌ History rewrite
* ❌ Leaving untracked files

---

## 📌 Real-World Use Cases

* Rolling back faulty production commits
* Incident recovery
* Maintaining audit-friendly Git history
* Supporting compliance requirements

---

## 🧠 Key Learnings (Very Important)

* Always check `git status` **before** revert operations
* Untracked files can break automated validations
* `git revert` should be run on a **clean working tree**
* Labs validate **repo state**, not just Git correctness
* DevOps work includes **safe recovery and cleanup**

---

## ✅ Final Status

✔ Repository cleaned
✔ Latest commit reverted correctly
✔ Commit message exactly `revert apps`
✔ Lab validation passed

🚀 **Day 28 completed successfully**

---
