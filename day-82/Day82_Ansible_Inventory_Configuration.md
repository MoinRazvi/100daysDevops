# Day 82 — Ansible Inventory Configuration

## 📌 Objective

Configured an **INI-based Ansible inventory file** to enable playbook execution against App Server 1.

This marks the beginning of the **Ansible Automation phase** in my 100 Days of DevOps journey.

---

# Architecture

```plaintext
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
App Server 1 (stapp01)
```

---

# Task Requirements

Create:

```bash
/home/thor/playbook/inventory
```

Validation command:

```bash
ansible-playbook -i inventory playbook.yml
```

---

# Step 1 — Navigate to Playbook Directory

## Command

```bash
cd /home/thor/playbook
```

### Output

```log
thor@jump_host ~/playbook $
```

---

# Step 2 — Create Inventory File

## Command

```bash
vi inventory
```

---

# Inventory Configuration

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_connection=ssh
```

---

## Inventory Structure Visualization

```plaintext
[app]                    → Host Group
stapp01                  → Target Server
ansible_user=tony        → SSH Username
ansible_ssh_pass=Ir0nM@n → SSH Password
ansible_connection=ssh   → Connection Type
```

---

# Step 3 — Verify Inventory

## Command

```bash
cat inventory
```

### Output

```log
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_connection=ssh
```

---

# Step 4 — Test Connectivity

## Command

```bash
ansible -i inventory all -m ping
```

### Output

```log
stapp01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# Step 5 — Execute Playbook

## Command

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Execution Flow

```plaintext
Read Inventory
     ↓
Resolve Host Group [app]
     ↓
Connect to stapp01
     ↓
Authenticate using tony
     ↓
Load playbook.yml
     ↓
Execute Tasks
     ↓
Return Status
```

---

# How the Validation Command Works

## Command

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Breakdown

### `ansible-playbook`

Executes automation tasks.

---

### `-i inventory`

Loads server list from:

```bash
inventory
```

---

### `playbook.yml`

Contains automation instructions.

Example:

```yaml
---
- hosts: app
  tasks:
    - name: Ping target
      ping:
```

---

# Practical Workflow

```plaintext
Inventory File → Target Server Mapping
Playbook File → Task Definitions
Ansible Engine → Task Execution
Result → Success / Failure
```

---

# Validation Success Output

```log
PLAY [app] ******************************

TASK [Ping target] **********************
ok: [stapp01]

PLAY RECAP ******************************
stapp01 : ok=1 changed=0 failed=0
```

---

# Key Concepts Learned

✅ Ansible Inventory
✅ Host Group Mapping
✅ SSH Authentication Variables
✅ Playbook Execution Flow
✅ Connectivity Validation
✅ Infrastructure Automation Basics

---

# Commands Used

## Create Inventory

```bash
cat > inventory << EOF
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_connection=ssh
EOF
```

---

## Test Connection

```bash
ansible -i inventory all -m ping
```

---

## Execute Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

# Final Validation Checklist

```plaintext
✓ Inventory file created
✓ Correct INI syntax
✓ App Server 1 added
✓ Authentication configured
✓ Connectivity verified
✓ Playbook execution successful
```

