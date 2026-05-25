
# 🚀 Day 89 — Ansible HTTPD Installation & Service Management

## 📌 Objective

Automate **Apache HTTPD installation and service management** across all Nautilus application servers using Ansible.

This task focused on:

* Installing Apache (`httpd`)
* Starting the service
* Enabling service on boot
* Executing automation across multiple servers from Jump Host

---

# 🏗️ Architecture

```plaintext
Jump Host (Ansible Controller)
        │
        ├── stapp01
        ├── stapp02
        └── stapp03
```

Ansible pushes tasks from the **Jump Host** to all managed nodes.

---

# 📂 Project Structure

```plaintext
/home/thor/ansible/
├── inventory
└── playbook.yml
```

---

# 📝 Inventory Configuration

**File:** `inventory`

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### Explanation

### `[app]`

Defines a host group.

This allows:

```bash
ansible app
```

to target all listed servers.

---

### `ansible_user`

SSH username for target host.

---

### `ansible_ssh_pass`

Password used for SSH authentication.

---

### `StrictHostKeyChecking=no`

Prevents host key verification prompts.

---

# ⚙️ Playbook Configuration

**File:** `playbook.yml`

```yaml
---
- name: Install and Configure HTTPD
  hosts: app
  become: yes

  tasks:

    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start and Enable httpd
      service:
        name: httpd
        state: started
        enabled: yes
```

---

# 🔍 Playbook Breakdown

## hosts: app

Targets all servers inside inventory group:

```ini
[app]
```

---

## become: yes

Runs tasks with sudo/root privileges.

Equivalent to:

```bash
sudo
```

---

## yum Module

```yaml
yum:
  name: httpd
  state: present
```

Installs Apache package.

Equivalent shell command:

```bash
yum install -y httpd
```

---

## service Module

```yaml
service:
  name: httpd
  state: started
  enabled: yes
```

Performs:

```bash
systemctl start httpd
systemctl enable httpd
```

---

# ▶️ Execution Steps

## 1. Navigate to directory

```bash
cd /home/thor/ansible
```

---

## 2. Test inventory connectivity

```bash
ansible -i inventory app -m ping
```

Expected:

```plaintext
stapp01 | SUCCESS
stapp02 | SUCCESS
stapp03 | SUCCESS
```

---

## 3. Execute playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

# 📊 Expected Output

```plaintext
PLAY RECAP
stapp01 : ok=2 changed=2 failed=0
stapp02 : ok=2 changed=2 failed=0
stapp03 : ok=2 changed=2 failed=0
```

---

# ✅ Verification Commands

## Check Apache status

```bash
ansible -i inventory app -a "systemctl status httpd"
```

---

## Confirm service enabled

```bash
ansible -i inventory app -a "systemctl is-enabled httpd"
```

Expected:

```plaintext
enabled
```

---

# 🧠 Key Concepts Learned

## Ansible Controller

Machine executing playbooks.

Here:

```plaintext
Jump Host
```

---

## Managed Nodes

Remote servers receiving tasks.

Here:

```plaintext
stapp01
stapp02
stapp03
```

---

## Idempotency

Running playbook repeatedly produces same state.

Example:

* If Apache already installed → no changes
* If running → no restart unless required

---

## Infrastructure as Code

Instead of manually installing on each server:

```bash
yum install httpd
systemctl start httpd
```

One playbook automates all.

---

# 🔥 Commands Reference

### Execute Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

### Ping All Hosts

```bash
ansible -i inventory app -m ping
```

### Check Service

```bash
ansible -i inventory app -a "systemctl status httpd"
```

---

# 🎯 Outcome

Successfully automated:

✅ HTTPD package installation
✅ Service startup
✅ Boot persistence
✅ Multi-server orchestration

---

# 📚 Day 89 Learning

Today reinforced the power of Ansible for:

* Package management
* Service orchestration
* Multi-node automation
* Declarative infrastructure management

---
