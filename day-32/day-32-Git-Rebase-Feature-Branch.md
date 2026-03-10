# 🚀 Day 32 – Rebase Feature Branch with Master

## 🎯 Objective

The Nautilus development team is working with the repository:

```
/usr/src/kodekloudrepos/media
```

A developer is currently working on the **feature branch**, but new commits were added to the **master branch**.

The requirement is to:

* Update the **feature branch with latest master changes**
* **Avoid creating a merge commit**
* **Preserve all existing feature branch work**
* **Use Git rebase instead of merge**
* Push the updated branch to remote.

---

# 🧠 Concept: Git Rebase vs Merge

### Merge

```
git merge master
```

Creates a **merge commit**.

Example history:

```
master ----A----B----C
              \ 
feature        D----E----F
                      \
                       Merge Commit
```

---

### Rebase

```
git rebase master
```

Replays feature commits **on top of master**.

Result:

```
master ----A----B----C
                      \
feature                D'---E'---F'
```

✔ Cleaner history
✔ No merge commit
✔ Linear commit structure

---

# 🧱 Environment

Server

```
Storage Server
```

User

```
natasha
```

Repository

```
/usr/src/kodekloudrepos/media
```

Branches involved

```
master
feature
```

---

# 🧪 Step-by-Step Implementation

---

# ✅ Step 1 – SSH to Storage Server

```
ssh natasha@ststor01
```

---

# ✅ Step 2 – Navigate to Repository

```
cd /usr/src/kodekloudrepos/media
```

Verify repository:

```
git status
```

---

# ✅ Step 3 – Check Existing Branches

```
git branch
```

Example output

```
* master
  feature
```

---

# ✅ Step 4 – Switch to Feature Branch

```
git checkout feature
```

Verify:

```
git branch
```

Output

```
* feature
  master
```

---

# ✅ Step 5 – Rebase Feature Branch with Master

```
git rebase master
```

What happens here:

Git takes commits from **feature branch** and replays them **on top of master branch**.

This keeps history clean and avoids merge commits.

---

# ⚠️ If Rebase Conflict Occurs

Check conflicts:

```
git status
```

Resolve conflict in files then run:

```
git add .
git rebase --continue
```

Repeat until rebase completes.

---

# ✅ Step 6 – Verify Commit History

```
git log --oneline --graph
```

Expected result:

```
master commit
previous master commit
feature commit 1
feature commit 2
```

✔ No merge commit
✔ Linear history

---

# ✅ Step 7 – Push Changes to Remote

Since rebase rewrites history, use force push:

```
git push origin feature --force
```

This updates the remote feature branch.

---

# 📌 Verification

Check branch status:

```
git status
```

Check commit graph:

```
git log --oneline --graph
```

Confirm feature branch sits **on top of master commits**.

---

# 🧠 Key Learnings

* `git rebase` creates **clean linear history**
* Used to **avoid unnecessary merge commits**
* Feature branch commits are **replayed on top of master**
* Rebase is common in **professional DevOps workflows**
* Force push may be required after rebasing.

---

# ❌ What Was Avoided

* ❌ Merge commits
* ❌ Messy commit history
* ❌ Losing feature branch work
* ❌ Overwriting master branch

---

# ✅ Final Status

✔ Feature branch successfully rebased
✔ No merge commits created
✔ All feature work preserved
✔ Changes pushed to remote repository
