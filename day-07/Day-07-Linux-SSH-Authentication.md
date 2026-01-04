# 🔐 Day 07 – Linux SSH Authentication

## 🎯 Objective

Learn how to configure **secure SSH authentication**, set up **password-less (key-based) access**, and verify SSH connectivity — a **core DevOps requirement** for automation and server management.

---

## 🔑 Why SSH Authentication Matters?

SSH authentication controls **who can access servers and how**.

Password-based SSH:

* Vulnerable to brute-force attacks
* Not suitable for automation

Key-based SSH:

* More secure
* Required for automation (Ansible, scripts, CI/CD)
* Industry standard for production systems

---

## 🛠️ Commands Used

* `ssh`
* `ssh-keygen`
* `ssh-copy-id`
* `chmod`
* `chown`
* `ls -l`
* `cat`

---

## 🧪 Hands-on Commands Used

### ✅ Generate SSH Key Pair

Generate an RSA key pair on the client or jump host:

```bash
ssh-keygen -t rsa -b 4096
```

📌 **Why `-b 4096`?**

* `-b` specifies key size
* `4096` bits provides stronger security than 2048
* Recommended for production environments

Keys are generated in:

```text
~/.ssh/id_rsa        (private key)
~/.ssh/id_rsa.pub    (public key)
```

---

### ✅ Copy Public Key to Remote Server

Copy the public key to the target user on the server:

```bash
ssh-copy-id user@server_ip
```

Example (jump host to app server):

```bash
ssh-copy-id tony@app01
```

---

### ✅ Verify Password-less SSH Login

```bash
ssh user@server_ip
```

If configured correctly:

* Login happens **without asking for a password**

---

### ✅ Manual Method (If `ssh-copy-id` Is Not Available)

Create `.ssh` directory on remote server:

```bash
mkdir -p ~/.ssh
```

Append public key to `authorized_keys`:

```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

---

### ✅ Set Correct SSH Permissions

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Ensure correct ownership:

```bash
chown -R user:user ~/.ssh
```

Verify permissions:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

---

### ✅ Test SSH Access to Multiple Servers (Automation Readiness)

```bash
ssh app01
ssh app02
ssh app03
```

---

## 📌 Real-World Use Cases

* Ansible controller to managed nodes
* Password-less automation scripts
* CI/CD pipelines
* Secure access via jump/bastion hosts
* Infrastructure management at scale

---

## 🧠 Key Learnings

* SSH key-based authentication is **more secure than passwords**
* `ssh-keygen -b 4096` creates strong encryption keys
* Correct permissions on `.ssh` and `authorized_keys` are mandatory
* Password-less SSH is essential for automation tools
* Always test SSH access before using it in scripts or tools
