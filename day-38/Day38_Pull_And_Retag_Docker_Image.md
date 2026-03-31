# 🚀 Day 38 – Pull and Re-tag Docker Image

## 🎯 Objective

The Nautilus DevOps team needs to prepare a container image for testing by:

* Pulling the `busybox:musl` image
* Re-tagging it as `busybox:news`

---

## 🧠 Why This Task Matters

Image tagging is important because:

* Helps manage different versions of images
* Allows custom naming for internal usage
* Supports CI/CD pipelines and environment-specific tagging
* Avoids re-downloading same image multiple times

---

## 🧱 Environment Details

* **Server**: App Server 3 (`stapp03`)
* **User**: `tony`
* **Original Image**: `busybox:musl`
* **New Tag**: `busybox:news`
* **Image**: BusyBox

---

## 🛠️ Commands Used

* `docker pull`
* `docker tag`
* `docker images`

---

## 🛠️ Implementation Steps

### 🔹 Step 1: Login to App Server 3

```bash
ssh tony@stapp03
```

---

### 🔹 Step 2: Pull the Image

```bash
docker pull busybox:musl
```

---

### 🔹 Step 3: Re-tag the Image

```bash
docker tag busybox:musl busybox:news
```

---

## 🧪 Verification

### 🔹 Check Available Images

```bash
docker images
```

Expected output should include both:

* `busybox:musl`
* `busybox:news`

(They will have the **same IMAGE ID**, confirming successful tagging)

---

## 📌 Key Notes

* `docker tag` does **not duplicate image**, only creates a new reference
* Both tags point to the same underlying image
* Useful for versioning and environment labeling

---

## 📌 Real-World Use Cases

* Creating environment-specific tags (dev, qa, prod)
* Version control for container images
* Preparing images for private registries
* CI/CD pipeline image management

---

## 🧠 Key Learnings

* Difference between pulling and tagging images
* Efficient image reuse without duplication
* Importance of naming conventions in DevOps
* Verifying images using `docker images`

---

## ❌ What Was Avoided

* ❌ Pulling duplicate images unnecessarily
* ❌ Using incorrect tag format
* ❌ Forgetting to verify image list
* ❌ Confusing tag with new image creation

---

## ✅ Final Status

✔ Image `busybox:musl` pulled successfully
✔ Image re-tagged as `busybox:news`
✔ Both tags available locally
✔ No duplicate image created

---
