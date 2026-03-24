# 🚀 Day 35 – Git Hook for Automatic Release Tagging

## 🎯 Objective

The Nautilus application development team was working on a project repository `/opt/blog.git` which is cloned under `/usr/src/kodekloudrepos/blog` on the Storage server in Stratos DC.

The requirement was to:

* Merge the `feature` branch into `master`
* Create a **post-update Git hook**
* Automatically generate a **release tag** whenever changes are pushed to the master branch

Tag format:

```
release-YYYY-MM-DD
```

---

# 🧠 Why This Task Matters

Git hooks are used to:

* Automate release processes
* Enforce workflows
* Reduce manual effort
* Enable CI/CD-like automation

---

# 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Local Repo Path**:

  ```
  /usr/src/kodekloudrepos/blog
  ```
* **Remote Repo Path**:

  ```
  /opt/blog.git
  ```

---

# 🛠️ Commands Used

* `git checkout`
* `git merge`
* `git push`
* `git tag`
* `date`
* `vi`
* `chmod`

---

# 🧪 Initial Attempt (What Went Wrong)

### ❌ Issue Observed

After completing the setup, running:

```bash
git tag
```

Returned:

```text
(no output)
```

---

### 🔍 Root Cause

The hook was mistakenly created in:

```
/usr/src/kodekloudrepos/blog/.git/hooks/
```

This is **incorrect** because:

* This is a **local repository**
* `post-update` hook only works on the **remote (bare repository)**

---

# ✅ Correct Fix (Very Important)

### 🔹 Step 1: Navigate to Remote Repo Hooks

```bash
cd /opt/blog.git/hooks
```

---

### 🔹 Step 2: Create post-update Hook

```bash
vi post-update
```

---

### 🔹 Step 3: Add Script

```bash
#!/bin/bash

tag="release-$(date +%F)"
git tag $tag
```

---

### 🔹 Step 4: Make Hook Executable

```bash
chmod +x post-update
```

---

# 🧪 Step 5 – Merge Feature into Master

```bash
cd /usr/src/kodekloudrepos/blog
git checkout master
git merge feature
```

---

# 🧪 Step 6 – Trigger Hook

Create a commit:

```bash
echo "trigger" >> test.txt
git add test.txt
git commit -m "trigger hook"
```

---

## ✅ Push Changes

```bash
git push origin master
```

---

# 🧪 Step 7 – Verify Tag Creation

First check in remote repo:

```bash
cd /opt/blog.git
git tag
```

Expected:

```
release-2026-03-24
```

---

## 🔹 Fetch Tags Locally

```bash
cd /usr/src/kodekloudrepos/blog
git fetch --tags
git tag
```

---

# ⚠️ Important Learning (Critical DevOps Concept)

| Hook        | Runs Where  |
| ----------- | ----------- |
| pre-commit  | Local repo  |
| pre-push    | Local repo  |
| post-update | Remote repo |

👉 Hooks related to push events **must be placed in remote (bare repo)**

---

# 📌 Real-World Use Cases

* Automated release tagging
* CI/CD pipelines
* Deployment triggers
* Version control automation

---

# 🧠 Key Learnings

* Hooks must be placed in the **correct repository**
* `post-update` runs on **remote repo after push**
* Tags can be generated dynamically using `date`
* Always test hooks after creation
* DevOps automation depends on correct placement

---

# ❌ What Was Avoided

* ❌ Manual tagging
* ❌ Wrong hook location
* ❌ Hardcoded values
* ❌ Skipping validation

---

# ✅ Final Status

✔ Feature branch merged
✔ post-update hook created correctly
✔ Automatic tag generated
✔ Changes pushed successfully
