# Day 88 — Deploying Apache Web Server with Ansible

## Task Overview

The Nautilus DevOps team needed to automate Apache HTTP server deployment across all application servers in Stratos DC using **Ansible**.

The task involved:

* Installing Apache (`httpd`)
* Starting and enabling service
* Deploying sample web content using **blockinfile**
* Setting file ownership and permissions
* Managing everything centrally from the Ansible control node

---

# Objective

Automate web server setup on all app servers:

* **stapp01**
* **stapp02**
* **stapp03**

Using:

* Ansible Inventory
* Playbook
* blockinfile module
* service module
* file module

---

# Inventory File

**File:**

```bash
/home/thor/ansible/inventory
```

## Inventory Configuration

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

# Understanding `[app]`

`[app]` is an **Ansible host group**

Purpose:

* Groups multiple hosts logically
* Allows executing tasks on all grouped servers

Example:

```yaml
hosts: app
```

This means:

Execute playbook on:

* stapp01
* stapp02
* stapp03

---

# Important Ansible Concept

## Is `app` Reserved?

**No**

You can use custom names like:

```ini
[webservers]
[xfusion]
[production]
```

But the playbook must match:

```yaml
hosts: webservers
```

---

# Playbook File

**File:**

```bash
/home/thor/ansible/playbook.yml
```

---

# Playbook Configuration

```yaml
---
- name: Configure Apache Web Server
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

    - name: Add content to index.html
      blockinfile:
        path: /var/www/html/index.html
        create: yes
        block: |
          Welcome to XfusionCorp!

          This is Nautilus sample file, created using Ansible!

          Please do not modify this file manually!

    - name: Set ownership
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache

    - name: Set permissions
      file:
        path: /var/www/html/index.html
        mode: '0644'
```

---

# Why Use `blockinfile`?

Task requirement explicitly specified:

* Use **blockinfile**
* Do not use custom markers
* Do not use empty markers

Default Ansible markers were used automatically.

---

# Commands Executed

## Navigate to directory

```bash
cd /home/thor/ansible
```

---

## Verify inventory

```bash
cat inventory
```

---

## Test connectivity

```bash
ansible -i inventory app -m ping
```

---

## Execute playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Verify deployed file

```bash
ansible -i inventory app -a "ls -l /var/www/html/index.html"
```

---

## Verify file content

```bash
ansible -i inventory app -a "cat /var/www/html/index.html"
```

---

# How Ansible Connection Works

Ansible uses:

## Inventory File

Defines:

* Hostnames
* Users
* Passwords
* SSH connection parameters

---

## Playbook

Defines:

* What task to execute
* On which host group

---

## Execution Flow

```plaintext
Thor (Control Node)
       ↓
Reads Inventory
       ↓
SSH Connection
       ↓
Runs Tasks Remotely
       ↓
Returns Results
```

---

# Key Learning

### Inventory answers:

**Where and how to connect**

---

### Playbook answers:

**What to do after connecting**

---

# Modules Used

## yum

Installs packages

```yaml
yum:
  name: httpd
  state: present
```

---

## service

Manages services

```yaml
service:
  name: httpd
  state: started
```

---

## blockinfile

Adds structured content

---

## file

Controls:

* Ownership
* Permissions

---

# Validation Command

The platform validates using:

```bash
ansible-playbook -i inventory playbook.yml
```

No extra arguments required.

---

# Outcome

Successfully automated:

* Apache installation
* Service startup
* Sample webpage deployment
* Ownership management
* Permission management

Across all application servers using Ansible.

---

# Key Takeaways

Day 88 reinforced:

* Inventory grouping
* Host targeting
* blockinfile usage
* Remote service management
* End-to-end web server automation

---

# Tech Stack

* Ansible
* YAML
* SSH
* Apache HTTPD
* Linux Automation

---
