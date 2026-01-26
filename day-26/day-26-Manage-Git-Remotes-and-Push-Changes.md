## 🚀 Day 26 – Manage Git Remotes and Push Changes

## 🎯 Objective

Update Git remotes for an existing repository and push new changes to a **newly added remote**, ensuring:

* Correct remote configuration
* Changes committed only to the `master` branch
* No unnecessary repository or permission changes

This task reflects a **real-world DevOps responsibility** involving Git server updates and multi-remote repository management.

---

## 🧠 Why This Task Matters

In production environments:

* Repositories often have **multiple remotes** (origin, backup, staging)
* DevOps teams manage repository migration and replication
* Explicit remote management avoids accidental pushes

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Working Repository**: `/usr/src/kodekloudrepos/demo`
* **Existing Remote**: `/opt/demo.git`
* **New Remote Name**: `dev_demo`
* **New Remote Path**: `/opt/xfusioncorp_demo.git`
* **Branch Updated**: `master`
* **File Added**: `/tmp/index.html`

---

## 🛠️ Commands Used

* `git remote`
* `git remote add`
* `git status`
* `git add`
* `git commit`
* `git push`
* `cp`

---

## 🧪 Hands-on Implementation

### ✅ Step 1: Login to Storage Server

```bash
ssh natasha@ststor01
```

---

### ✅ Step 2: Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/demo
```

Verify repository:

```bash
git status
```

---

### ✅ Step 3: Add New Git Remote

```bash
git remote add dev_demo /opt/xfusioncorp_demo.git
```

Verify remotes:

```bash
git remote -v
```

Expected output includes:

```text
dev_demo  /opt/xfusioncorp_demo.git (fetch)
dev_demo  /opt/xfusioncorp_demo.git (push)
```

---

### ✅ Step 4: Copy File into Repository

```bash
cp /tmp/index.html .
```

Verify:

```bash
ls -l index.html
```

---

### ✅ Step 5: Add and Commit File on `master`

```bash
git add index.html
git commit -m "Add index.html to demo repository"
```

Verify:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

### ✅ Step 6: Push `master` Branch to New Remote

The task specifies pushing to the **new remote**, so explicitly reference it:

```bash
git push dev_demo master
```

✔ This pushes the `master` branch to `/opt/xfusioncorp_demo.git`.

---

## 📌 Verification

Confirm branch pushed to new remote:

```bash
git log --oneline --decorate -1
```

(Optional) Verify refs on remote if required:

```bash
git ls-remote dev_demo
```

---

## 📌 Real-World Use Cases

* Migrating repositories to new Git servers
* Maintaining backup remotes
* Distributing code to different environments
* Controlled Git operations in DevOps workflows

---

## 🧠 Key Learnings

* Git repositories can have **multiple remotes**
* Remote names are user-defined (`origin`, `dev_demo`, etc.)
* Always verify remotes before pushing
* Explicit push commands prevent mistakes
* DevOps teams often manage Git infrastructure changes

---

## ✅ Final Status

✔ New remote `dev_demo` added
✔ File committed on `master`
✔ `master` branch pushed to new remote
✔ Task completed successfully
