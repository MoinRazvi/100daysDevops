# 🚀 Day 31 – Reset Git History to Specific Commit

## 🎯 Objective

The Nautilus development team wants to clean up the repository:

```
/usr/src/kodekloudrepos/ecommerce
```

The goal is to:

* Remove unwanted test commits
* Reset branch to commit:

  ```
  add data.txt file
  ```
* Ensure only two commits remain:

  * initial commit
  * add data.txt file

---

## 🧠 Important Concept

This is NOT a `git revert`.

This is:

```
git reset --hard
```

Difference:

| Command          | Effect                                       |
| ---------------- | -------------------------------------------- |
| git revert       | Creates a new commit undoing changes         |
| git reset --hard | Moves branch pointer back (rewrites history) |

We need history rewrite here.

---

## 🧱 Environment

* Server: Storage Server
* User: natasha
* Repo Path:

  ```
  /usr/src/kodekloudrepos/ecommerce
  ```

---

# 🧪 Step-by-Step Implementation

---

## ✅ Step 1: SSH to Storage Server

```bash
ssh natasha@ststor01
```

---

## ✅ Step 2: Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/ecommerce
```

Check branch:

```bash
git branch
```

Ensure you're on master:

```bash
git checkout master
```

---

## ✅ Step 3: Identify the Target Commit

```bash
git log --oneline
```

Find commit:

```
<hash> add data.txt file
```

Example:

```
abc1234 add data.txt file
```

---

## ✅ Step 4: Reset History

```bash
git reset --hard abc1234
```

✔ HEAD now points to that commit
✔ All later commits removed locally

---

## ✅ Step 5: Verify History

```bash
git log --oneline
```

Expected output:

```
abc1234 add data.txt file
def5678 initial commit
```

Only **two commits** should exist.

---

## ⚠ If Remote Exists (Optional)

If remote repository exists and you must update it:

```bash
git push origin master --force
```

⚠ Force push required because history changed.

(Only do this if lab requires remote update.)

---

# 📌 Verification

```bash
git status
```

Expected:

```
nothing to commit, working tree clean
```

---

# 🧠 Key Learnings

* `git reset --hard` rewrites commit history
* Use reset only when history cleanup is required
* Reset moves branch pointer
* Revert creates new commit — not suitable here
* Force push is required after history rewrite

---

# ❌ What Was Avoided

* ❌ Using git revert
* ❌ Creating extra cleanup commits
* ❌ Partial history edits
* ❌ Deleting repo manually

---

# ✅ Final Status

✔ Repository cleaned
✔ HEAD moved to "add data.txt file"
✔ Only two commits remain
✔ Working tree clean

🚀 **Day 31 completed successfully**

---
