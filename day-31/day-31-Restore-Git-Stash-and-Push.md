# 🚀 Day 31 – Restore Stashed Changes and Push to Remote

## 🎯 Objective

The Nautilus development team was working on the repository:

```
/usr/src/kodekloudrepos/blog
```

One of the developers had previously stashed some in-progress changes. Now the team wants to:

* Locate the stashed changes
* Restore the stash identified as `stash@{1}`
* Commit the restored changes
* Push them to the remote repository

---

## 🧠 Why Git Stash Is Used

`git stash` is used to:

* Temporarily save work-in-progress changes
* Switch branches without committing
* Keep the working directory clean

Stashes are stored locally and must be manually restored.

---

## 🧱 Environment Details

* **Server**: Storage Server (`ststor01`)
* **User**: `natasha`
* **Repository Path**:

  ```
  /usr/src/kodekloudrepos/blog
  ```
* **Stash Identifier to Restore**:

  ```
  stash@{1}
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
cd /usr/src/kodekloudrepos/blog
```

Verify clean state:

```bash
git status
```

---

## ✅ Step 3: List Available Stashes

```bash
git stash list
```

Example output:

```text
stash@{0}: WIP on master: abc1234 recent changes
stash@{1}: WIP on master: def5678 some previous changes
```

Confirm that `stash@{1}` exists.

---

## ✅ Step 4: Restore the Required Stash

Apply the stash:

```bash
git stash apply stash@{1}
```

✔ This restores changes but keeps stash entry.

If you want to remove it after restore, you could use:

```bash
git stash drop stash@{1}
```

(Not mandatory unless lab requires cleanup.)

---

## ✅ Step 5: Verify Restored Changes

```bash
git status
```

You should now see modified or new files ready to commit.

---

## ✅ Step 6: Stage the Changes

```bash
git add .
```

---

## ✅ Step 7: Commit the Changes

```bash
git commit -m "restored stashed changes"
```

(If lab requires specific commit message, use exact wording.)

---

## ✅ Step 8: Push to Remote Repository

```bash
git push origin master
```

---

# 📌 Verification

```bash
git log --oneline -3
```

You should see your new commit at the top.

Also verify:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

# 🧠 Key Learnings

* `git stash list` shows saved stashes
* `git stash apply` restores stash without deleting it
* `git stash pop` restores and deletes stash
* Always commit after applying stash
* Stash is local — must push changes manually

---

# ❌ What Was Avoided

* ❌ Losing stashed changes
* ❌ Applying wrong stash
* ❌ Forgetting to commit
* ❌ Leaving untracked modifications

---

# ✅ Final Status

✔ Stash `stash@{1}` restored
✔ Changes committed
✔ Changes pushed to remote
✔ Working tree clean

---
