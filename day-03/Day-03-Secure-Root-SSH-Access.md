# 🚀 Day 03 – Secure Root SSH Access

## 🎯 Objective

Understand how to **secure root SSH access** by disabling insecure login methods, enforcing best practices, and validating SSH configuration — a **critical production-grade DevOps skill**.

---

## 🔐 Why Secure Root SSH Access?

Direct root SSH access is a **high-risk attack vector**.
In production environments, unsecured root login can lead to:

* Brute-force attacks
* Privilege escalation
* Complete system compromise

Securing SSH helps enforce:

* 🔒 Least privilege
* 🛡️ Strong authentication
* 📜 Compliance & audit requirements

---

## 🛠️ Commands Used

* `ssh`
* `sshd`
* `vi / nano`
* `systemctl`
* `grep`
* `chmod`
* `chown`

---

## 🧪 Hands-on Commands Used

### ✅ Check Current Root SSH Configuration

```bash
sudo grep -i PermitRootLogin /etc/ssh/sshd_config
```

---

### ✅ Disable Root Login via SSH

Edit the SSH configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

Set or update:

```text
PermitRootLogin no
```

---

### ✅ Restart SSH Service to Apply Changes

```bash
sudo systemctl restart sshd
```

Verify service status:

```bash
sudo systemctl status sshd
```

---

### ✅ Test Root SSH Access (Should Fail)

```bash
ssh root@server_ip
```

**Expected result:** Access denied

---

### ✅ Enable Key-Based Access for Non-Root User

Generate SSH key (from client / jump host):

```bash
ssh-keygen -t rsa -b 4096
```

Copy key to server user:

```bash
ssh-copy-id user@server_ip
```

---

### ✅ Verify Password-less SSH Login

```bash
ssh user@server_ip
```

---

### ✅ Disable Password Authentication (Optional Hardening)

Edit SSH configuration:

```bash
sudo vi /etc/ssh/sshd_config
```

Update:

```text
PasswordAuthentication no
```

Restart SSH service:

```bash
sudo systemctl restart sshd
```

---

### ✅ Validate SSH Configuration Syntax

```bash
sudo sshd -t
```

*(No output indicates the configuration is valid)*

---

### 📌 Real-World Use Cases

* Hardening production Linux servers
* Preventing brute-force SSH attacks
* Meeting security compliance standards
* Enforcing secure access via bastion/jump hosts
* Protecting cloud and on-prem infrastructure

---

### 🧠 Key Learnings

* Root SSH login should be **disabled in production**
* Key-based authentication is **more secure than passwords**
* SSH changes must always be **validated before reload**
* Restarting `sshd` applies security policies immediately
* Secure SSH is a **foundational DevOps responsibility**  
