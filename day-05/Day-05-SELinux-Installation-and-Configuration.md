# 🔐 Day 05 – SELinux Installation & Configuration

## 🎯 Objective

Understand how to **install, enable, and configure SELinux**, switch between its modes, and verify enforcement — a **critical Linux security concept** used in production environments.

---

## 🛡️ Why SELinux Matters?

SELinux (Security-Enhanced Linux) provides **Mandatory Access Control (MAC)**, which goes beyond traditional Linux permissions.

Without SELinux:

* Processes can access more resources than required
* A compromised service can impact the entire system

With SELinux:

* Access is **policy-driven**
* Even root processes are restricted
* Security breaches are contained

---

## 🛠️ Commands Used

* `getenforce`
* `sestatus`
* `setenforce`
* `vi / nano`
* `rpm`
* `yum / dnf`
* `reboot`

---

## 🧪 Hands-on Commands Used

### ✅ Check SELinux Status

```bash
getenforce
```

Alternative detailed status:

```bash
sestatus
```

---

### ✅ Install SELinux Utilities (If Not Installed)

```bash
sudo yum install -y policycoreutils policycoreutils-python-utils
```

(For newer systems)

```bash
sudo dnf install -y policycoreutils policycoreutils-python-utils
```

---

### ✅ Switch SELinux Modes Temporarily

Set SELinux to **Permissive** mode:

```bash
sudo setenforce 0
```

Set SELinux back to **Enforcing** mode:

```bash
sudo setenforce 1
```

⚠️ Note: `setenforce` changes are **temporary** and reset after reboot.

---

### ✅ Verify Current Mode

```bash
getenforce
```

---

### ✅ Configure SELinux Permanently

Edit the SELinux configuration file:

```bash
sudo vi /etc/selinux/config
```

Set one of the following:

```text
SELINUX=enforcing
SELINUX=permissive
SELINUX=disabled
```

---

### ✅ Apply Permanent Changes

```bash
sudo reboot
```

---

## 📌 Real-World Use Cases

* Securing production Linux servers
* Preventing unauthorized file and process access
* Containing compromised services
* Meeting compliance and security standards
* Debugging access-denied issues using permissive mode

---

## 🧠 Key Learnings

* SELinux enforces **mandatory access control**
* Enforcing mode blocks unauthorized actions
* Permissive mode logs violations without blocking
* Disabled mode removes SELinux protection (not recommended)
* Always use **permissive mode for troubleshooting**, not disable
* Permanent changes require editing `/etc/selinux/config`
