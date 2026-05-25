
# 🚀 Day 90 — Ansible ACL File Permissions Management

## 📌 Objective

Automate secure file creation and ACL-based permission management across Nautilus application servers using Ansible.

This task focused on:

* Creating files under `/opt/security`
* Enforcing root ownership
* Assigning ACL permissions to specific users/groups
* Debugging YAML formatting issues

---

## 🏗️ Architecture

```plaintext
Jump Host (Ansible Controller)
        │
        ├── stapp01 → blog.txt → group tony (r)
        ├── stapp02 → story.txt → user steve (rw)
        └── stapp03 → media.txt → group banner (rw)
```

---

## 📂 Project Structure

```plaintext
/home/thor/ansible/
├── inventory
└── playbook.yml
```

---

## 📝 Inventory Configuration

### Command

```bash
cat inventory
```

### Output

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

## ⚙️ Playbook File

**File:** `/home/thor/ansible/playbook.yml`

```yaml
---
- name: Configure ACL permissions on app servers
  hosts: app
  become: yes

  tasks:

    - name: Create /opt/security on all servers
      file:
        path: /opt/security
        state: directory
        mode: '0755'

    - name: Create blog.txt on stapp01
      file:
        path: /opt/security/blog.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp01"

    - name: Set ACL for group tony
      acl:
        path: /opt/security/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
      when: inventory_hostname == "stapp01"

    - name: Create story.txt on stapp02
      file:
        path: /opt/security/story.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp02"

    - name: Set ACL for user steve
      acl:
        path: /opt/security/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present
      when: inventory_hostname == "stapp02"

    - name: Create media.txt on stapp03
      file:
        path: /opt/security/media.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp03"

    - name: Set ACL for group banner
      acl:
        path: /opt/security/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
      when: inventory_hostname == "stapp03"
```

---

## ❌ Error Faced

### Command

```bash
ansible-playbook -i inventory playbook.yml
```

### Error Output

```plaintext
ERROR! We were unable to read either as JSON nor YAML

Syntax Error while loading YAML.
found character that cannot start any token
```

---

## 🔍 Debugging Step

### Check Hidden Characters

```bash
cat -A playbook.yml
```

### Output

```plaintext
^Ipath: /opt/security/blog.txt
^Ientity: tony
^Ipermissions: r
```

---

## 🧠 Root Cause

`^I` represents:

```plaintext
TAB characters
```

YAML strictly allows:

✅ Spaces
❌ Tabs

---

## 🛠 Resolution Applied

### Replace Tabs with Spaces

```bash
sed -i 's/\t/  /g' playbook.yml
```

---

## ✅ Syntax Validation

### Command

```bash
ansible-playbook -i inventory playbook.yml --syntax-check
```

### Output

```plaintext
playbook: playbook.yml
```

---

## ▶️ Playbook Execution

### Command

```bash
ansible-playbook -i inventory playbook.yml
```

### Output

```plaintext
PLAY [Configure ACL permissions on app servers]

TASK [Create /opt/security on all servers] ******** ok
TASK [Create blog.txt on stapp01] ***************** changed
TASK [Set ACL for group tony] ********************* changed
TASK [Create story.txt on stapp02] **************** changed
TASK [Set ACL for user steve] ********************* changed
TASK [Create media.txt on stapp03] **************** changed
TASK [Set ACL for group banner] ******************* changed
```

---

## 📊 Verification Logs

### Verify stapp01

```bash
ansible stapp01 -i inventory -a "getfacl /opt/security/blog.txt"
```

Output:

```plaintext
group:tony:r--
```

---

### Verify stapp02

```bash
ansible stapp02 -i inventory -a "getfacl /opt/security/story.txt"
```

Output:

```plaintext
user:steve:rw-
```

---

### Verify stapp03

```bash
ansible stapp03 -i inventory -a "getfacl /opt/security/media.txt"
```

Output:

```plaintext
group:banner:rw-
```

---

## 🔥 Useful Commands

### Execute Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

### Syntax Check

```bash
ansible-playbook -i inventory playbook.yml --syntax-check
```

### Detect Hidden Tabs

```bash
cat -A playbook.yml
```

### Replace Tabs

```bash
sed -i 's/\t/  /g' playbook.yml
```

---

## 🎯 Final Outcome

Successfully automated:

✅ ACL file creation
✅ Root ownership
✅ User/group ACL permissions
✅ YAML debugging resolution

---

## 📚 Key Learnings

Day-90 reinforced:

* Linux ACL automation
* YAML indentation discipline
* Hidden tab debugging
* Conditional host execution
* Ansible file permission management

---

## ⭐ Debugging Highlight

**Using `cat -A` to detect hidden tabs**

This was the exact breakthrough that resolved the failure.

---
