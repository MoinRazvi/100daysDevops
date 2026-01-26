## 🚀 Day 23 – Fork Git Repository Using Gitea UI

## 🎯 Objective

Enable a newly onboarded developer (**Jon**) to start contributing by **forking an existing Git repository** using the **Gitea web interface** — a common workflow in enterprise and open-source environments.

---

## 🧠 Why Forking Is Important

Forking allows developers to:

* Work independently without affecting the original repository
* Experiment with features safely
* Submit changes via pull requests
* Follow standard Git collaboration practices

This workflow is widely used on platforms like **GitHub, GitLab, and Gitea**.

---

## 🌐 Platform Details

* **Git Server**: Gitea
* **Access Method**: Web UI
* **Original Repository**: `sarah/story-blog`
* **Forked Under User**: `jon`

---

## 🧪 Hands-on Implementation (UI-Based Workflow)

> ⚠️ **Note**: This task is completed entirely using the **web UI**.
> Screenshots or screen recording are required for verification.

---

### ✅ Step 1: Access Gitea UI

* Click on the **Gitea UI** button available on the **top bar** of the lab environment.
* This opens the Gitea login page.

---

### ✅ Step 2: Login as Jon

Login with the following credentials:

* **Username**: `jon`
* **Password**: `Jon_pass123`

Click **Sign In**.

---

### ✅ Step 3: Locate the Repository

* Use the search bar or browse repositories
* Open the repository:

  ```
  sarah/story-blog
  ```

---

### ✅ Step 4: Fork the Repository

* Click the **Fork** button at the top-right corner
* Select **jon** as the target user/namespace when prompted

---

### ✅ Step 5: Verify the Fork

* After forking, confirm you are redirected to:

  ```
  jon/story-blog
  ```
* Verify:

  * Repository owner is **jon**
  * Repository name remains **story-blog**

---

## 📌 Real-World Use Cases

* Developer onboarding
* Open-source contribution workflows
* Feature development via forks
* Secure collaboration without direct write access

---

## 🧠 Key Learnings

* Forking creates an independent copy under a new user
* UI-based Git operations are common in enterprise tools
* Verification of ownership is critical after forking
* Screenshots provide audit-ready proof of work

---

## ✅ Final Status

✔ Logged in as user `jon`
✔ Repository `sarah/story-blog` located
✔ Successfully forked under `jon`
✔ Fork verified
