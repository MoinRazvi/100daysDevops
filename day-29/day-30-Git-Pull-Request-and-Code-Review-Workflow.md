
# 🚀 Day 30 – Git Pull Request & Code Review Workflow (Gitea)

## 🎯 Objective

Implement a **controlled Git workflow** where direct pushes to the `master` branch are restricted. Instead, changes must go through:

* Feature branch
* Pull Request (PR)
* Code review
* Approval
* Merge

This ensures only **reviewed and approved code** reaches the `master` branch.

---

## 🧠 Why This Is Important

In production environments:

* ❌ Developers should NOT push directly to `master`
* ✅ Code must go through PR review
* ✅ Changes should be approved before merging
* ✅ Accountability and traceability must be maintained

This mirrors workflows used in:

* GitHub
* GitLab
* Bitbucket
* Enterprise DevOps pipelines

---

## 🧱 Environment Details

* **Git Server**: Gitea
* **User 1**: max
* **User 2 (Reviewer)**: tom
* **Repository**: Pre-cloned under max’s home directory
* **Feature Branch**: `story/fox-and-grapes`
* **Target Branch**: `master`
* **PR Title**: `Added fox-and-grapes story`

---

# 🧪 Implementation Steps

---

## ✅ Step 1: SSH into Storage Server as Max

```bash
ssh max@ststor01
```

Password:

```
Max_pass123
```

---

## ✅ Step 2: Navigate to Max’s Cloned Repository

```bash
cd ~
ls
cd <repo-directory>
```

---

## ✅ Step 3: Verify Existing Content & Commit History

Check branches:

```bash
git branch -a
```

Check commit history:

```bash
git log --oneline --decorate --graph
```

Verify:

* Sarah’s story exists
* Author information is correct
* Commit messages are visible
* Max’s branch `story/fox-and-grapes` exists

---

## ✅ Step 4: Confirm Max’s Branch Is Pushed

```bash
git branch -r
```

You should see:

```
origin/story/fox-and-grapes
```

This confirms Max already pushed his feature branch.

---

# 🌐 Pull Request Workflow (UI-Based)

---

## ✅ Step 5: Access Gitea Web UI

* Click **Gitea UI** button from the lab top bar
* Login as:

```
Username: max
Password: Max_pass123
```

📸 Take Screenshot: Max logged into Gitea

---

## ✅ Step 6: Create Pull Request

1. Go to repository
2. Click **Pull Requests**
3. Click **New Pull Request**
4. Configure:

| Field              | Value                        |
| ------------------ | ---------------------------- |
| Title              | `Added fox-and-grapes story` |
| Source Branch      | `story/fox-and-grapes`       |
| Destination Branch | `master`                     |

5. Click **Create Pull Request**

📸 Take Screenshot: PR created successfully

---

## ✅ Step 7: Add Reviewer (Tom)

1. Open the newly created PR
2. On the right side panel, click **Reviewers**
3. Add **tom**

📸 Take Screenshot: Tom added as reviewer

---

# 🔁 Review & Approval Process

---

## ✅ Step 8: Logout Max

Logout from Gitea UI.

---

## ✅ Step 9: Login as Tom

```
Username: tom
Password: Tom_pass123
```

📸 Screenshot: Tom logged in

---

## ✅ Step 10: Review and Approve PR

1. Go to Pull Requests
2. Open PR titled:

   ```
   Added fox-and-grapes story
   ```
3. Click **Review**
4. Approve changes
5. Click **Merge**

📸 Screenshot: PR approved and merged

---

# ✅ Verification

After merge:

* `story/fox-and-grapes` changes appear in `master`
* PR status shows **Merged**
* Commit history updated

Optional local verification:

```bash
git checkout master
git pull
git log --oneline
```

---

# 📌 Real-World DevOps Concepts Demonstrated

* Feature branch workflow
* Pull Request governance
* Code review enforcement
* Role-based access control
* Approval before merge
* Protected master branch practice

---

# 🧠 Key Learnings

* Master branch should never accept direct pushes
* PRs ensure accountability and quality control
* Reviewers add security and reliability
* DevOps workflows are about process control, not just commands
* UI workflows are common in enterprise environments

---

# ❌ What Was Avoided

* ❌ Direct push to master
* ❌ Force merge
* ❌ Skipping review
* ❌ Unreviewed code promotion

---

# ✅ Final Status

✔ Feature branch pushed
✔ Pull Request created
✔ Reviewer assigned
✔ PR approved by Tom
✔ Changes merged into master

