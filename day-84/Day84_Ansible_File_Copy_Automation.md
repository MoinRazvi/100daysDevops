# Day 84 — Ansible File Copy Automation

## 📌 Objective

Automated file distribution from the Jump Host to all application servers using Ansible.

Copied:

```plaintext id="d2k7wc"
/usr/src/dba/index.html
```

to:

```plaintext id="r5j8mn"
/opt/dba/index.html
```

across:

* stapp01
* stapp02
* stapp03

---

# Architecture

```plaintext id="p8x4vd"
Jump Host
   │
   ▼
Inventory File
   │
   ▼
Ansible Playbook
   │
   ▼
Parallel SSH Connections
   │
   ├── stapp01
   ├── stapp02
   └── stapp03
         │
         ▼
   /opt/dba/index.html
```

---

# Task Requirements

### Inventory

```bash id="f6w9pz"
/home/thor/ansible/inventory
```

### Playbook

```bash id="k2m7rs"
/home/thor/ansible/playbook.yml
```

Validation:

```bash id="x4v8qa"
ansible-playbook -i inventory playbook.yml
```

---

# Step 1 — Navigate to Directory

```bash id="u3r5yc"
cd /home/thor/ansible
```

---

# Step 2 — Create Inventory

## inventory

```ini id="c9t6nj"
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

# Step 3 — Initial Playbook

```yaml id="a8w2df"
---
- hosts: app
  tasks:
    - name: Create destination directory
      file:
        path: /opt/dba
        state: directory

    - name: Copy index file
      copy:
        src: /usr/src/dba/index.html
        dest: /opt/dba/index.html
```

---

# Issue Faced

## Error

```log id="z7m1kp"
Destination /opt/dba not writable
```

---

# Root Cause

The playbook was running as:

* tony
* steve
* banner

These users do not have write permissions under:

```plaintext id="j4r8xn"
/opt
```

---

# Solution Applied

Added privilege escalation:

```yaml id="b5x9qw"
become: yes
```

---

# Final Working Playbook

```yaml id="n6k3tv"
---
- hosts: app
  become: yes
  tasks:
    - name: Create destination directory
      file:
        path: /opt/dba
        state: directory
        mode: '0755'

    - name: Copy index file
      copy:
        src: /usr/src/dba/index.html
        dest: /opt/dba/index.html
        mode: '0644'
```

---

# Step 4 — Execute Playbook

```bash id="r9p2ec"
ansible-playbook -i inventory playbook.yml
```

---

# Successful Output

```log id="m8x4zy"
TASK [Create destination directory]
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Copy index file]
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]
```

---

# Validation

Verify file copy:

```bash id="w2c6kr"
ansible -i inventory app -m shell -a "ls -l /opt/dba/index.html"
```

---

# Validation Output

```log id="q5v8yt"
stapp01 | CHANGED
stapp02 | CHANGED
stapp03 | CHANGED
```

---

# Execution Flow

```plaintext id="e7k3pz"
Read Inventory
      ↓
Connect to All App Servers
      ↓
Elevate Privileges (become)
      ↓
Create /opt/dba
      ↓
Copy index.html
      ↓
Validation Success
```

---

# Key Learnings

✅ Multi-node automation
✅ Inventory grouping
✅ Parallel file distribution
✅ Ansible copy module
✅ Privilege escalation with become
✅ File permission handling

---

# Commands Used

## Test Connectivity

```bash id="h4m7zx"
ansible -i inventory all -m ping
```

---

## Execute Playbook

```bash id="t6w2kn"
ansible-playbook -i inventory playbook.yml
```

---

# Final Validation Checklist

```plaintext id="c3r9vq"
✓ Inventory configured
✓ All app servers reachable
✓ Destination directory created
✓ File copied successfully
✓ Permission issue resolved
✓ Validation passed
```

---

# Final Status

```plaintext id="y8n5md"
SUCCESS
```
