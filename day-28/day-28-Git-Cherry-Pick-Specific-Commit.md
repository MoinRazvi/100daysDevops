
## 🚀 Day 29 – Merge a Specific Commit from Feature Branch to Master

## 🎯 Objective

The Nautilus application development team is working on a Git repository `/opt/games.git`, cloned at `/usr/src/kodekloudrepos/games` on the Storage Server. One developer is actively working on the `feature` branch, but they want to **merge only a specific commit** into the `master` branch while keeping other feature work in progress.

The commit to be merged is identified by its message:

```
Update info.txt
```

---

## 🧠 Why This Task Matters

In real-world DevOps and Git workflows:

* Developers may want to **promote only selected changes**
* Full branch merges are not always safe
* **Cherry-picking** allows precise control over what gets merged

This is commonly used for:

* Hotfixes
* Selective feature promotion
* Release stabilization

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Repository Path**: `/usr/src/kodekloudrepos/games`
* **Remote Repository**: `/opt/games.git`
* **Source Branch**: `feature`
* **Target Branch**: `master`
* **Commit Message to Merge**: `Update info.txt`

---

## 🛠️ Commands Used

* `git branch`
* `git log`
* `git checkout`
* `git cherry-pick`
* `git push`
* `git status`

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

### ✅ Step 3: Identify the Commit on Feature Branch

Switch to feature branch:

```bash
git checkout feature
```

List commits:

```bash
git log --oneline
```

Locate the commit with message:

```text
Update info.txt
```

Copy its **commit hash** (example):

```text
abc1234 Update info.txt
```

---

### ✅ Step 4: Switch Back to Master Branch

```bash
git checkout master
```

Confirm:

```bash
git branch
```

Expected:

```text
* master
  feature
```

---

### ✅ Step 5: Cherry-Pick the Required Commit

```bash
git cherry-pick abc1234
```

✔ This merges **only that specific commit** into `master`.

If no conflicts occur, the cherry-pick completes automatically.

---

### ✅ Step 6: Verify the Commit on Master

```bash
git log --oneline -3
```

Expected output includes:

```text
xyz9999 Update info.txt
```

---

### ✅ Step 7: Push Changes to Remote Repository

```bash
git push origin master
```

---

## 📌 Verification

Ensure:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

## ❌ What Was Avoided

* ❌ Full branch merge
* ❌ Rebasing
* ❌ Force push
* ❌ Interrupting in-progress feature work

---

## 📌 Real-World Use Cases

* Hotfix deployment
* Selective production releases
* Partial feature promotion
* Release candidate stabilization

---

## 🧠 Key Learnings

* `git cherry-pick` allows precise commit-level merging
* Commit messages help identify required changes
* Always verify branch context before cherry-picking
* Push changes only after verification
* DevOps teams often manage selective merges

---

## ✅ Final Status

✔ Correct commit identified
✔ Commit cherry-picked into `master`
✔ Feature branch untouched
✔ Changes pushed successfully
