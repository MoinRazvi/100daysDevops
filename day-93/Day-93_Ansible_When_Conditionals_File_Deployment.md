# 🚀 Day 93 — Ansible Conditional File Deployment using `when`

## 📌 Objective

Automate conditional file deployment across all Nautilus application servers using **Ansible `when` conditionals**.

This task focused on:

✅ Using `hosts: all`
✅ Conditional task execution
✅ Using `ansible_nodename`
✅ File deployment automation
✅ Ownership assignment
✅ Permission management

---

# 🖥️ Interactive Validation Logs

## 🔹 Playbook Execution

```bash
thor@jump-host ~/ansible$ ansible-playbook -i inventory playbook.yml
```

### Output

```plaintext
PLAY [Conditional file deployment using Ansible]

TASK [Gathering Facts] ******************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Create /opt/sysops directory] ****************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Copy blog.txt to App Server 1] ***************************************
changed: [stapp01]
skipping: [stapp02]
skipping: [stapp03]

TASK [Copy story.txt to App Server 2] **************************************
skipping: [stapp01]
changed: [stapp02]
skipping: [stapp03]

TASK [Copy media.txt to App Server 3] **************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************
stapp01 : ok=3 changed=2 skipped=2 failed=0
stapp02 : ok=3 changed=2 skipped=2 failed=0
stapp03 : ok=3 changed=2 skipped=2 failed=0
```

---

# 📂 Project Structure

```plaintext
/ home/thor/ansible/
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
- name: Conditional file deployment using Ansible
  hosts: all
  become: yes

  tasks:

    - name: Create /opt/sysops directory
      file:
        path: /opt/sysops
        state: directory
        mode: '0755'

    - name: Copy blog.txt to App Server 1
      copy:
        src: /usr/src/sysops/blog.txt
        dest: /opt/sysops/blog.txt
        owner: tony
        group: tony
        mode: '0777'
      when: ansible_nodename == "stapp01"

    - name: Copy story.txt to App Server 2
      copy:
        src: /usr/src/sysops/story.txt
        dest: /opt/sysops/story.txt
        owner: steve
        group: steve
        mode: '0777'
      when: ansible_nodename == "stapp02"

    - name: Copy media.txt to App Server 3
      copy:
        src: /usr/src/sysops/media.txt
        dest: /opt/sysops/media.txt
        owner: banner
        group: banner
        mode: '0777'
      when: ansible_nodename == "stapp03"
```

---

# ▶️ Execute Playbook

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

# 🔍 Verification Logs

## Verify App Server 1

```bash
ansible stapp01 -i inventory -a "ls -l /opt/sysops/blog.txt"
```

Output:

```plaintext
-rwxrwxrwx tony tony
```

---

## Verify App Server 2

```bash
ansible stapp02 -i inventory -a "ls -l /opt/sysops/story.txt"
```

Output:

```plaintext
-rwxrwxrwx steve steve
```

---

## Verify App Server 3

```bash
ansible stapp03 -i inventory -a "ls -l /opt/sysops/media.txt"
```

Output:

```plaintext
-rwxrwxrwx banner banner
```

---

# 🧠 Key Learning

## Why `hosts: all`?

The task explicitly required:

```yaml
hosts: all
```

This means all servers are targeted.

---

## How selective execution works

Selective execution is controlled by:

```yaml
when:
```

Example:

```yaml
when: ansible_nodename == "stapp01"
```

This tells Ansible:

👉 Execute only if current node = **stapp01**

---

# 🔍 Understanding `ansible_nodename`

This is a gathered fact.

It dynamically identifies current host.

To inspect:

```bash
ansible all -i inventory -m setup -a "filter=ansible_nodename"
```

Output example:

```plaintext
stapp01 => ansible_nodename: stapp01
stapp02 => ansible_nodename: stapp02
stapp03 => ansible_nodename: stapp03
```

---

# ⚡ Why Use Conditionals?

Without conditionals:

All tasks run on all servers ❌

With conditionals:

Each server gets only its intended file ✅

---

# 🏆 Final Outcome

Successfully achieved:

✅ Conditional file deployment
✅ Correct file placement
✅ Correct ownership assignment
✅ Correct permissions (`0777`)
✅ Dynamic host-based task execution
✅ Validation passed successfully

---

# ⭐ Debugging Highlight

Understanding the difference between:

```yaml
hosts: all
```

and

```yaml
when:
```

was the key concept.

`hosts: all` targets all servers
`when` filters task execution per server

---
