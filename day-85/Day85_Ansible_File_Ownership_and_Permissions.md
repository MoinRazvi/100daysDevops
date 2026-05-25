# Day 85 — Ansible File Ownership and Permissions

## 📌 Objective

Automated file creation across all application servers using Ansible while configuring:

* File creation
* File permissions
* Dynamic ownership assignment
* Group ownership configuration

Created:

```plaintext id="x3k7wd"
/usr/src/web.txt
```

on:

* App Server 1
* App Server 2
* App Server 3

---

# Architecture

```plaintext id="t9v4qe"
Jump Host
   │
   ▼
Inventory File
   │
   ▼
Ansible Playbook
   │
   ▼
Parallel Execution
   │
   ├── stapp01 → owner: tony
   ├── stapp02 → owner: steve
   └── stapp03 → owner: banner
         │
         ▼
   /usr/src/web.txt (0755)
```

---

# Task Requirements

### Inventory File

```bash id="k4m8yc"
~/playbook/inventory
```

### Playbook File

```bash id="p2x7rb"
~/playbook/playbook.yml
```

Validation:

```bash id="f8w3nd"
ansible-playbook -i inventory playbook.yml
```

---

# Step 1 — Navigate to Directory

```bash id="r5j9tv"
cd ~/playbook
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

Each host uses its own authentication credentials.

---

# Step 3 — Create Playbook

```bash id="y8k5rc"
vi playbook.yml
```

---

## Final Working Playbook

```yaml id="w4n7md"
---
- hosts: app
  become: yes
  tasks:
    - name: Create blank file with required ownership and permissions
      file:
        path: /usr/src/web.txt
        state: touch
        mode: '0755'
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
```

---

# Playbook Logic Breakdown

---

## `become: yes`

```plaintext id="c9x2jm"
Executes task with elevated privileges
```

Required because:

```plaintext id="q7r4zp"
/usr/src
```

requires root-level access.

---

## `state: touch`

Creates an empty file.

Equivalent to:

```bash id="m6v8ke"
touch /usr/src/web.txt
```

---

## `mode: '0755'`

Sets permissions:

```plaintext id="z5w1tn"
rwxr-xr-x
```

---

## Dynamic Ownership

```yaml id="h2p9yc"
owner: "{{ ansible_user }}"
group: "{{ ansible_user }}"
```

Automatically maps:

```plaintext id="n4x6rd"
stapp01 → tony
stapp02 → steve
stapp03 → banner
```

This avoids writing separate tasks.

---

# Step 4 — Execute Playbook

```bash id="j3m7wv"
ansible-playbook -i inventory playbook.yml
```

---

# Successful Output

```log id="f7q2zt"
PLAY [app]

TASK [Create blank file with required ownership and permissions]
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP
stapp01 : ok=1 changed=1 failed=0
stapp02 : ok=1 changed=1 failed=0
stapp03 : ok=1 changed=1 failed=0
```

---

# Validation

Verify:

```bash id="v9r4xn"
ansible -i inventory app -m shell -a "ls -l /usr/src/web.txt"
```

---

# Validation Output

```log id="k8w5mp"
-rwxr-xr-x 1 tony   tony   0
-rwxr-xr-x 1 steve  steve  0
-rwxr-xr-x 1 banner banner 0
```

---

# Execution Flow

```plaintext id="e5p2yc"
Read Inventory
      ↓
Authenticate to All Servers
      ↓
Elevate Privileges
      ↓
Create File
      ↓
Apply Permissions
      ↓
Assign Ownership
      ↓
Validation Success
```

---

# Key Learnings

✅ Ansible File Module
✅ Dynamic Variables
✅ Permission Management
✅ Ownership Assignment
✅ Parallel Execution
✅ Privilege Escalation

---

# Commands Used

## Connectivity Test

```bash id="m2t8zr"
ansible -i inventory all -m ping
```

---

## Execute Playbook

```bash id="c6x4vn"
ansible-playbook -i inventory playbook.yml
```

---

## Validation

```bash id="r3w9jk"
ansible -i inventory app -m shell -a "ls -l /usr/src/web.txt"
```

---

# Final Validation Checklist

```plaintext id="b7n5qd"
✓ Inventory created
✓ Playbook created
✓ File created
✓ Permissions set to 0755
✓ Ownership mapped correctly
✓ Validation successful
```

---

# Final Status

```plaintext id="x4v8pm"
SUCCESS
```
