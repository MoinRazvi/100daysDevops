
# Day 83 — Ansible Playbook File Creation

## 📌 Objective

Updated Ansible inventory configuration and created a playbook to generate an empty file on **App Server 3**.

---

# Architecture

```plaintext id="u8j3cd"
Jump Host
   │
   ▼
Inventory File
   │
   ▼
Ansible Playbook
   │
   ▼
SSH Authentication
   │
   ▼
App Server 3 (stapp03)
   │
   ▼
/tmp/file.txt
```

---

# Task Requirements

### Update inventory

```bash id="k5x9dp"
/home/thor/ansible/inventory
```

### Create playbook

```bash id="a3w7rb"
/home/thor/ansible/playbook.yml
```

Validation:

```bash id="z1q5hn"
ansible-playbook -i inventory playbook.yml
```

---

# Step 1 — Navigate to Directory

```bash id="p7n4yf"
cd /home/thor/ansible
```

---

# Step 2 — Update Inventory

## Final Working inventory

```ini id="b2v8ks"
[app]
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

## Inventory Breakdown

```plaintext id="e6r4xm"
[app]                            → Host Group
stapp03                          → App Server 3
ansible_user=banner              → SSH User
ansible_ssh_pass=BigGr33n        → Password
StrictHostKeyChecking=no         → Skip host verification
```

---

# Step 3 — Create Playbook

```bash id="m8z6qa"
vi playbook.yml
```

Add:

```yaml id="x4t7lc"
---
- hosts: app
  tasks:
    - name: Create empty file
      file:
        path: /tmp/file.txt
        state: touch
```

---

# Step 4 — Test Connectivity

## Command

```bash id="h5d9ur"
ansible -i inventory all -m ping
```

---

## Initial Issue Faced

```log id="n3y7kv"
UNREACHABLE!
Invalid/incorrect password
Permission denied
```

---

## Root Cause

Used:

```ini id="w2q8mf"
ansible_ssh_pass=$pwd
```

The variable was resolving incorrectly.

---

## Fix Applied

Replaced variable with direct password:

```ini id="c9x6bj"
ansible_ssh_pass=BigGr33n
```

---

## Successful Output

```log id="s4m1zt"
stapp03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# Step 5 — Execute Playbook

```bash id="j8w5nr"
ansible-playbook -i inventory playbook.yml
```

---

## Successful Execution

```log id="t6r3yx"
PLAY [app]

TASK [Gathering Facts] ************
ok: [stapp03]

TASK [Create empty file] **********
changed: [stapp03]

PLAY RECAP
stapp03 : ok=2 changed=1 failed=0
```

---

# Validation Check

Verify file creation:

```bash id="q7v2de"
ansible -i inventory app -m shell -a "ls -l /tmp/file.txt"
```

---

## Output

```log id="l4x9mc"
-rw-r--r-- 1 banner banner 0 /tmp/file.txt
```

---

# Execution Flow

```plaintext id="d5k8sa"
Read Inventory
      ↓
Authenticate to stapp03
      ↓
Execute Playbook
      ↓
Run file module
      ↓
Create /tmp/file.txt
      ↓
Validation Success
```

---

# Key Learnings

✅ Updating Inventory
✅ Ansible Authentication
✅ File Module
✅ Host Connectivity Validation
✅ Playbook Execution

---

# Commands Used

## Test Inventory

```bash id="v1j6re"
ansible -i inventory all -m ping
```

---

## Execute Playbook

```bash id="u3n8zp"
ansible-playbook -i inventory playbook.yml
```

---

# Final Validation Checklist

```plaintext id="y9t4qb"
✓ Inventory updated
✓ Password corrected
✓ Connectivity verified
✓ Playbook created
✓ File created successfully
✓ Validation passed
```
