## 🚀 Day 25 – Git Branch, Commit, Merge & Push Workflow

## 🎯 Objective

Execute a complete **Git feature workflow** by:

* Creating a new branch from `master`
* Adding a new file
* Committing changes
* Merging the branch back into `master`
* Pushing updates to the remote repository

This task reflects a **real-world DevOps + development collaboration process**.

---

## 🧠 Why This Task Matters

This workflow is used daily in production environments to:

* Isolate feature development
* Maintain clean main branches
* Enable safe collaboration
* Support CI/CD pipelines

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Working Repository**: `/usr/src/kodekloudrepos/games`
* **Remote Repository**: `/opt/games.git`
* **Base Branch**: `master`
* **Feature Branch**: `devops`
* **File Added**: `/tmp/index.html`

---

## 🛠️ Commands Used

* `git checkout`
* `git branch`
* `git add`
* `git commit`
* `git merge`
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
cd /usr/src/kodekloudrepos/games
```

Verify repository status:

```bash
git status
```

---

### ✅ Step 3: Ensure You Are on `master`

```bash
git checkout master
```

---

### ✅ Step 4: Create and Switch to Feature Branch

```bash
git checkout -b devops
```

Verify:

```bash
git branch
```

Expected:

```text
* devops
  master
```

---

### ✅ Step 5: Copy File into Repository

```bash
cp /tmp/index.html .
```

Verify:

```bash
ls -l index.html
```

---

### ✅ Step 6: Add and Commit the File

```bash
git add index.html
git commit -m "Add index.html file for devops branch"
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

### ✅ Step 7: Switch Back to `master`

```bash
git checkout master
```

---

### ✅ Step 8: Merge Feature Branch into `master`

```bash
git merge devops
```

Expected:

* Merge completes successfully
* No conflicts

---

### ✅ Step 9: Push Changes to Remote Repository

Push `devops` branch:

```bash
git push origin devops
```

Push `master` branch:

```bash
git push origin master
```

---

## 📌 Verification

Check file exists in both branches:

```bash
git show devops:index.html
git show master:index.html
```

---

## 📌 Real-World Use Cases

* Feature development lifecycle
* Safe code integration
* Release management
* CI/CD pipeline triggering

---

## 🧠 Key Learnings

* Always create feature branches from `master`
* Commit changes only in feature branches
* Merge validated changes back into `master`
* Push both feature and main branches when required
* Verification is critical after merge operations

---

## ✅ Final Status

✔ Feature branch created
✔ File added and committed
✔ Branch merged successfully
✔ Changes pushed to remote
✔ Task completed successfully
