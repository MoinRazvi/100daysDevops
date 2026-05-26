# 🚀 Day 92 — Ansible Roles & Jinja2 Template Deployment

## 📌 Objective

Automate **HTTPD role execution** using Ansible Roles and deploy a dynamic web page using **Jinja2 templating**.

This task focused on:

✅ Role-based automation
✅ Jinja2 template creation
✅ Dynamic hostname rendering
✅ Template deployment via role
✅ Ownership & permissions management
✅ Resolving role path validation issue

---

# 🖥️ Interactive Validation Logs

## 🔹 Initial Playbook Execution

```bash
thor@jump-host ~/ansible$ ansible-playbook -i inventory playbook.yml
```

### Error Output

```plaintext
ERROR! the role 'httpd' was not found in

/home/thor/ansible/roles
/home/thor/.ansible/roles
/usr/share/ansible/roles
/etc/ansible/roles
/home/thor/ansible
```

---

# ❌ Error Faced

## Root Cause

Role directory was created as:

```plaintext
/home/thor/ansible/role/httpd/
```

But Ansible expects:

```plaintext
/home/thor/ansible/roles/httpd/
```

**Missing `s` in roles**

---

# 🔍 Debugging Step

## Verify Current Path

```bash
pwd
```

Output:

```plaintext
/home/thor/ansible/role/httpd/tasks
```

This confirmed incorrect directory structure.

---

# 🛠 Resolution Applied

## Rename Directory

```bash
cd /home/thor/ansible
mv role roles
```

---

## New Issue Encountered

Lab validator failed with:

```plaintext
'/home/thor/ansible/role/httpd/templates/index.html.j2' file does not exist
```

---

# 🧠 Root Cause Analysis

This lab had a special validation requirement:

### Validator checks:

```plaintext
/home/thor/ansible/role/httpd/
```

### Ansible runtime expects:

```plaintext
/home/thor/ansible/roles/httpd/
```

---

# ⚡ Final Fix Applied

Maintained both directories.

---

## Create Validation Path

```bash
mkdir -p /home/thor/ansible/role/httpd/tasks
mkdir -p /home/thor/ansible/role/httpd/templates
```

---

## Copy Files Back

```bash
cp /home/thor/ansible/roles/httpd/tasks/main.yml \
/home/thor/ansible/role/httpd/tasks/

cp /home/thor/ansible/roles/httpd/templates/index.html.j2 \
/home/thor/ansible/role/httpd/templates/
```

---

# ⚙️ Playbook File

## **File:** `/home/thor/ansible/playbook.yml`

```yaml
---
- name: Run httpd role
  hosts: stapp01
  become: yes

  roles:
    - httpd
```

---

# 📝 Jinja2 Template

## **File:**

`/home/thor/ansible/role/httpd/templates/index.html.j2`

```html
This file was created using Ansible on {{ inventory_hostname }}
```

---

# 🔥 Jinja2 Dynamic Rendering

Template:

```html
{{ inventory_hostname }}
```

Runtime output on App Server 1:

```html
This file was created using Ansible on stapp01
```

No hardcoding required.

---

# ⚙️ Role Task File

## **File:**

`/home/thor/ansible/role/httpd/tasks/main.yml`

```yaml
---
- name: Install httpd
  yum:
    name: httpd
    state: present

- name: Start and enable httpd
  service:
    name: httpd
    state: started
    enabled: yes

- name: Deploy index template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    owner: tony
    group: tony
    mode: '0777'
```

---

# ▶️ Execute Playbook

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

# ✅ Successful Execution Output

```plaintext
PLAY [Run httpd role]

TASK [httpd : Install httpd] **************** ok
TASK [httpd : Start and enable httpd] ******* ok
TASK [httpd : Deploy index template] ******** changed

PLAY RECAP
stapp01 : ok=3 changed=1 failed=0
```

---

# 🔍 Verification Commands

## Check File Content

```bash
ansible stapp01 -i inventory -a "cat /var/www/html/index.html"
```

Output:

```plaintext
This file was created using Ansible on stapp01
```

---

## Verify Permissions

```bash
ansible stapp01 -i inventory -a "ls -l /var/www/html/index.html"
```

Output:

```plaintext
-rwxrwxrwx tony tony
```

---

# 📚 Key Learnings

### 1️⃣ Ansible Role Directory Convention

Ansible automatically searches only inside:

```plaintext
roles/
```

not

```plaintext
role/
```

---

### 2️⃣ Jinja2 Templates

Used for dynamic variable substitution.

Example:

```html
{{ inventory_hostname }}
```

---

### 3️⃣ Lab Validation Edge Cases

Sometimes:

* Runtime expects one structure
* Validator checks another

Practical workaround:

Maintain both paths.

---

# 🎯 Final Outcome

Successfully achieved:

✅ HTTPD role execution
✅ Dynamic Jinja2 deployment
✅ Hostname rendering
✅ Proper permissions
✅ Role path issue resolved
✅ Validation passed

---

# ⭐ Debugging Highlight

**Understanding difference between:**

```plaintext
role/
vs
roles/
```

A tiny typo caused the full role resolution failure.

---
