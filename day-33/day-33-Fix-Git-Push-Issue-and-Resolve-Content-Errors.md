# 🚀 Day 33 – Fix Git Push Issue and Correct Repository Content

## 🎯 Objective

Sarah and Max were working on a repository containing multiple stories. Max recently added new changes but is **unable to push them to the remote repository**.

The task is to:

* Fix the Git push issue
* Ensure `story-index.txt` contains **titles for all 4 stories**
* Correct a typo:

  ```
  Mooose → Mouse
  ```
* Successfully push the changes to the remote repository

---

# 🧠 Why This Task Matters

This simulates real DevOps scenarios where:

* Push fails due to remote updates
* Content validation is required before deployment
* Developers must **fix conflicts + correct data**
* Code must be clean before pushing

---

# 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `max`
* **Repository Path**:

  ```
  /home/max/story-blog
  ```
* **Remote Repository**: Gitea
* **Files Involved**:

  * `story-index.txt`

---

# 🛠️ Commands Used

* `git status`
* `git pull`
* `git add`
* `git commit`
* `git push`
* `vi` / `nano`

---

# 🧪 Step-by-Step Implementation

---

## ✅ Step 1 – SSH into Storage Server

```bash
ssh max@ststor01
```

Password:

```
Max_pass123
```

---

## ✅ Step 2 – Navigate to Repository

```bash
cd /home/max/story-blog
```

Check status:

```bash
git status
```

---

## ❌ Step 3 – Attempt Push (Expected Failure)

```bash
git push origin master
```

You will likely see:

```text
rejected (fetch first)
```

---

## 🔍 Root Cause

* Remote repository has new changes
* Local branch is **behind origin**
* Git blocks push to prevent overwriting history

---

## ✅ Step 4 – Pull Latest Changes

```bash
git pull origin master
```

✔ This syncs local repo with remote

---

## ⚠️ If Conflict Occurs

Resolve manually:

```bash
git status
```

Edit conflicting files → then:

```bash
git add .
git commit -m "resolve merge conflict"
```

---

## ✅ Step 5 – Fix Content Issues

### 🔹 Edit File

```bash
vi story-index.txt
```

---

### 🔹 Fix Typo

Change:

```
The Lion and the Mooose
```

To:

```
The Lion and the Mouse
```

---

### 🔹 Ensure 4 Story Titles Exist

Example structure:

```
The Fox and the Grapes
The Lion and the Mouse
The Cat and the Dog
The Monkey and the Crocodile
```

✔ Must include **all 4 stories**

---

## ✅ Step 6 – Stage Changes

```bash
git add story-index.txt
```

---

## ✅ Step 7 – Commit Changes

```bash
git commit -m "fix typo and update story index"
```

---

## ✅ Step 8 – Push to Remote

```bash
git push origin master
```

✔ Push should now succeed

---

# 🌐 Optional UI Verification (Gitea)

Login:

* Username: `max` or `sarah`
* Password: respective credentials

Verify:

* Updated file present
* Correct story titles
* Typo fixed

📸 Take screenshots for lab validation

---

# 📌 Verification

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

# 🧠 Key Learnings

* Push can fail when local repo is outdated
* Always pull before pushing
* Resolve conflicts carefully
* Validate content before committing
* DevOps includes **data correctness + Git operations**

---

# ❌ What Was Avoided

* ❌ Force push
* ❌ Ignoring conflicts
* ❌ Pushing incorrect data
* ❌ Skipping validation

---

# ✅ Final Status

✔ Push issue resolved
✔ Repository updated
✔ Typo corrected (Mooose → Mouse)
✔ All 4 story titles added
✔ Changes successfully pushed
