# 🚀 Day 91 — Ansible HTTPD Web Server Deployment with `lineinfile`

## 📌 Objective

Automate **HTTPD installation**, **service management**, and **web page deployment** across all Nautilus application servers using **Ansible**.

This task focused on:

✅ Installing Apache HTTPD
✅ Starting & enabling service
✅ Creating web content
✅ Adding content at top using `lineinfile`
✅ Setting file ownership & permissions
✅ Infrastructure automation using Ansible

---

# 🖥️ Interactive Validation Logs

## 🔹 Playbook Execution

```bash
thor@jump-host ~/ansible$ ansible-playbook -i inventory playbook.yml
```

### Output

```plaintext
PLAY [Install and configure httpd] *******************************************

TASK [Gathering Facts] *******************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Install httpd] *********************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Start and enable httpd] ************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Create index.html with base content] ***********************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Add welcome line at top] ***********************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Set final ownership] ***************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

PLAY RECAP *******************************************************************
stapp01 : ok=6 changed=4 unreachable=0 failed=0
stapp02 : ok=6 changed=4 unreachable=0 failed=0
stapp03 : ok=6 changed=4 unreachable=0 failed=0
```

---

# 📂 Project Structure

```plaintext
/home/thor/ansible/
├── inventory
└── playbook.yml
```

---

# 📄 Inventory File

## **File:** `inventory`

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=<password> ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=<password> ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=<password> ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

# ⚙️ Playbook File

## **File:** `playbook.yml`

```yaml
---
- name: Install and configure httpd
  hosts: app
  become: yes

  tasks:

    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html with base content
      copy:
        dest: /var/www/html/index.html
        content: |
          This is a Nautilus sample file, created using Ansible!
        owner: apache
        group: apache
        mode: '0777'

    - name: Add welcome line at top
      lineinfile:
        path: /var/www/html/index.html
        line: Welcome to Nautilus Group!
        insertbefore: BOF

    - name: Set final ownership
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: '0777'
```

---

# ▶️ Execute Playbook

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

# 🔍 Verification Commands

## Verify Web Page Content

```bash
ansible app -i inventory -a "cat /var/www/html/index.html"
```

### Output

```plaintext
Welcome to Nautilus Group!
This is a Nautilus sample file, created using Ansible!
```

---

## Verify File Permissions

```bash
ansible app -i inventory -a "ls -l /var/www/html/index.html"
```

### Output

```plaintext
-rwxrwxrwx apache apache
```

---

## Verify HTTPD Service

```bash
ansible app -i inventory -a "systemctl status httpd"
```

---

# 🧠 Key Learning

🔹 Using `yum` module for package installation
🔹 Managing services with Ansible
🔹 File deployment using `copy`
🔹 Adding top-line content using `lineinfile`
🔹 Using `insertbefore: BOF`
🔹 Ownership & permission management

---

# ⚡ Important Concept

### Why `insertbefore: BOF`?

`BOF` = **Beginning Of File**

It ensures:

```plaintext
Welcome to Nautilus Group!
```

gets inserted at the **top** of the file.

---

# 🏆 Final Outcome

✅ HTTPD installed on all servers
✅ Service running successfully
✅ Web page deployed
✅ Welcome line inserted at top
✅ Correct ownership applied
✅ Permissions set to `0777`
✅ Validation passed successfully

---
