# Day 87 — Install SQLite Using Ansible Yum Module

## 📌 Objective

Automated package installation on all application servers using **Ansible Yum module**.

Installed package:

```plaintext id="x3k7wd"
sqlite
```

Target servers:

* App Server 1 (`stapp01`)
* App Server 2 (`stapp02`)
* App Server 3 (`stapp03`)

---

# Architecture

```plaintext id="t9v4qe"
Jump Host (Ansible Controller)
        │
        ▼
Inventory File
        │
        ▼
Ansible Playbook
        │
        ▼
YUM Package Installation
   ├── stapp01
   ├── stapp02
   └── stapp03
        │
        ▼
SQLite Installed Successfully
```

---

# Task Requirements

### Inventory File

```bash id="k4m8yc"
/home/thor/playbook/inventory
```

---

### Playbook File

```bash id="p2x7rb"
/home/thor/playbook/playbook.yml
```

---

### Validation Command

```bash id="f8w3nd"
ansible-playbook -i inventory playbook.yml
```

---

# Step 1 — Create Playbook Directory

```bash id="r5j9tv"
mkdir -p /home/thor/playbook
cd /home/thor/playbook
```

---

# Step 2 — Create Inventory File

```bash id="a7x4wp"
vi inventory
```

Add:

```ini id="u6m2zk"
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

# Inventory Breakdown

```plaintext id="d3p8vq"
stapp01 → tony
stapp02 → steve
stapp03 → banner
```

---

# Step 3 — Create Playbook

```bash id="y8k5rc"
vi playbook.yml
```

---

## Final Working Playbook

```yaml id="w4n7md"
---
- name: Install sqlite package
  hosts: app
  become: yes

  tasks:
    - name: Install sqlite using yum
      yum:
        name: sqlite
        state: present
```

---

# Playbook Logic Breakdown

## `hosts: app`

Targets all servers inside:

```plaintext id="c9x2jm"
[app]
```

group from inventory.

---

## `become: yes`

Executes package installation with elevated privileges.

Equivalent to:

```bash id="q7r4zp"
sudo yum install
```

---

## Yum Module

Uses Ansible's native package manager module.

Equivalent command:

```bash id="m6v8ke"
yum install -y sqlite
```

---

## `state: present`

Ensures package exists.

If already installed:

```plaintext id="z5w1tn"
No changes
```

If missing:

```plaintext id="h2p9yc"
Installs automatically
```

---

# Step 4 — Ensure Thor Ownership

```bash id="n4x6rd"
chown -R thor:thor /home/thor/playbook
```

This ensures validation runs successfully.

---

# Step 5 — Test Connectivity

```bash id="j3m7wv"
ansible -i inventory app -m ping
```

Expected output:

```plaintext id="f7q2zt"
stapp01 | SUCCESS => {"ping": "pong"}
stapp02 | SUCCESS => {"ping": "pong"}
stapp03 | SUCCESS => {"ping": "pong"}
```

---

# Step 6 — Execute Playbook

```bash id="v9r4xn"
ansible-playbook -i inventory playbook.yml
```

---

# Successful Output

```log id="k8w5mp"
PLAY [Install sqlite package]

TASK [Gathering Facts]
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Install sqlite using yum]
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]
```

---

# Verification

## Incorrect Command (Error Faced)

```bash id="e5p2yc"
ansible -i inventory app -a "sqlite3 --verion"
```

Error:

```plaintext id="m2t8zr"
sqlite3: Error: unknown option: -verion
```

---

## Root Cause

Typo in version flag.

```plaintext id="c6x4vn"
--verion ❌
--version ✅
```

---

## Correct Verification Command

```bash id="r3w9jk"
ansible -i inventory app -a "sqlite3 --version"
```

---

# Successful Verification Output

```log id="b7n5qd"
stapp01 | CHANGED | rc=0
stapp02 | CHANGED | rc=0
stapp03 | CHANGED | rc=0
```

---

# Execution Flow

```plaintext id="x4v8pm"
Read Inventory
      ↓
Connect to All App Servers
      ↓
Escalate Privileges
      ↓
Invoke Yum Module
      ↓
Install SQLite
      ↓
Verify Installation
```

---

# Key Learnings

✅ Ansible Yum Module
✅ Package Automation
✅ Privilege Escalation
✅ Multi-node Execution
✅ Validation Troubleshooting
✅ Command Verification

---

# Commands Used

## Connectivity Check

```bash id="p4m8zw"
ansible -i inventory app -m ping
```

---

## Execute Playbook

```bash id="k7r3vc"
ansible-playbook -i inventory playbook.yml
```

---

## Verify Installation

```bash id="n2x7yf"
ansible -i inventory app -a "sqlite3 --version"
```

---

# Final Validation Checklist

```plaintext id="v8m5qd"
✓ Inventory created
✓ Playbook created
✓ SQLite installed
✓ Thor ownership configured
✓ Validation successful
✓ Verification corrected
```

---

# Final Status

```plaintext id="f6p4zt"
SUCCESS
```
