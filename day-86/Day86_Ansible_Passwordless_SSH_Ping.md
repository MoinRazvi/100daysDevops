# Day 86 — Ansible Passwordless SSH Setup

## 📌 Objective

Configured **passwordless SSH authentication** between:

```plaintext id="x7k3wd"
Jump Host (Ansible Controller)
        ↓
App Server 2 (Managed Node)
```

and validated connectivity using **Ansible ping**.

---

# Architecture

```plaintext id="t8v4qe"
thor@jump-host
    │
    ▼
SSH Key Pair
    │
    ├── Private Key → stays on Jump Host
    └── Public Key → copied to App Server 2
                │
                ▼
Authorized Keys Trust
                │
                ▼
Passwordless SSH Authentication
                │
                ▼
Ansible Ping Success
```

---

# Task Requirement

Using inventory file:

```bash id="k5m8yc"
/home/thor/ansible/inventory
```

Ensure Ansible ping works for:

```plaintext id="p3x7rb"
stapp02
```

---

# Why Passwordless SSH?

The task specifically required:

```plaintext id="f7w3nd"
Password-less SSH connection
```

This means Ansible should authenticate using **SSH keys**, not passwords.

---

# Important Clarification

## StrictHostKeyChecking vs Passwordless SSH

### StrictHostKeyChecking=no

```ini id="u6m2zk"
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

This only skips:

```plaintext id="d3p8vq"
Are you sure you want to continue connecting (yes/no)?
```

It does **NOT** remove password prompts.

---

## Passwordless SSH

Requires:

* SSH key pair generation
* Public key installation
* Authorized trust relationship

This removes:

```plaintext id="a7x4wp"
steve@stapp02's password:
```

---

# Step 1 — Navigate to Ansible Directory

```bash id="r5j9tv"
cd /home/thor/ansible
```

---

# Step 2 — Verify Inventory

Initial inventory:

```ini id="w4n7md"
[app]
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

This uses password authentication.

---

# Step 3 — Generate SSH Key

Command:

```bash id="c9x2jm"
ssh-keygen -t rsa
```

---

## Key Insight

This command works from **any directory**.

Example:

```bash id="q7r4zp"
cd /tmp
ssh-keygen -t rsa
```

It still prompts:

```plaintext id="m6v8ke"
Enter file in which to save the key (/home/thor/.ssh/id_rsa):
```

Press:

```plaintext id="z5w1tn"
Enter
```

---

# Default Key Paths

## Private Key

```bash id="h2p9yc"
/home/thor/.ssh/id_rsa
```

---

## Public Key

```bash id="n4x6rd"
/home/thor/.ssh/id_rsa.pub
```

---

# Step 4 — Copy Public Key to App Server 2

Command:

```bash id="j3m7wv"
ssh-copy-id steve@stapp02
```

Password:

```plaintext id="f7q2zt"
Am3ric@
```

---

# Where Does It Copy?

Source:

```bash id="v9r4xn"
/home/thor/.ssh/id_rsa.pub
```

Destination:

```bash id="k8w5mp"
/home/steve/.ssh/authorized_keys
```

Not:

```plaintext id="e5p2yc"
id_rsa.pub
```

It appends to:

```plaintext id="m2t8zr"
authorized_keys
```

---

# Step 5 — Verify Passwordless SSH

Test:

```bash id="c6x4vn"
ssh steve@stapp02
```

Expected:

```plaintext id="r3w9jk"
Login without password prompt
```

Exit:

```bash id="b7n5qd"
exit
```

---

# Step 6 — Update Inventory

Remove password authentication.

Final inventory:

```ini id="x4v8pm"
[app]
stapp02 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

# Step 7 — Test Ansible Ping

## Ping only stapp02

```bash id="p4m8zw"
ansible -i inventory stapp02 -m ping
```

---

## Why Target Specific Host?

This command:

```bash id="k7r3vc"
stapp02
```

targets only App Server 2.

---

## Other Examples

### Ping all app servers

```bash id="n2x7yf"
ansible -i inventory app -m ping
```

---

### Ping stapp01

```bash id="v8m5qd"
ansible -i inventory stapp01 -m ping
```

---

### Ping stapp03

```bash id="f6p4zt"
ansible -i inventory stapp03 -m ping
```

---

# Successful Output

```json id="r9p2ec"
stapp02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# Execution Flow

```plaintext id="m8x4zy"
Generate SSH Key
      ↓
Create Public/Private Pair
      ↓
Copy Public Key
      ↓
Remote authorized_keys Updated
      ↓
Passwordless Trust Established
      ↓
Ansible Ping Validation
```

---

# Key Learnings

✅ SSH Key Generation
✅ Default SSH Key Paths
✅ ssh-copy-id
✅ authorized_keys
✅ Passwordless Authentication
✅ Host-specific Ansible ping

---

# Commands Used

## Generate Key

```bash id="w2c6kr"
ssh-keygen -t rsa
```

---

## Copy Key

```bash id="q5v8yt"
ssh-copy-id steve@stapp02
```

---

## Verify SSH

```bash id="e7k3pz"
ssh steve@stapp02
```

---

## Ping stapp02

```bash id="h4m7zx"
ansible -i inventory stapp02 -m ping
```

---

# Final Validation Checklist

```plaintext id="t6w2kn"
✓ SSH key generated
✓ Public key copied
✓ Passwordless login verified
✓ Inventory updated
✓ Ansible ping successful
```

---

# Final Status

```plaintext id="c3r9vq"
SUCCESS
```
