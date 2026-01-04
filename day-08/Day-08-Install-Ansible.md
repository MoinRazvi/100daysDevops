# 🔑 Day 08 – Install Ansible

## 🎯 Objective

Learn how to **install Ansible on a Linux system**, ensure it is **available globally for all users**, and verify the installation — a **foundational step for automation and configuration management** in DevOps.

---

## 🤖 Why Ansible?

Ansible is an **agentless automation tool** widely used for:

* Configuration management
* Application deployment
* Infrastructure automation
* Orchestration across multiple servers

It works over **SSH** and requires **no agents** on managed nodes, making it simple and powerful.

---

## 🛠️ Commands Used

* `python3`
* `pip3`
* `pip`
* `ansible`
* `ansible-playbook`
* `which`
* `echo`
* `export`

---

## 🧪 Hands-on Commands Used

### ✅ Verify Python and pip3 Availability

Check Python version:

```bash
python3 --version
```

Check pip3 version:

```bash
pip3 --version
```

---

### ✅ Install Ansible Using pip3 (Specific Version)

Install **Ansible 4.9.0** using pip3:

```bash
pip3 install ansible==4.9.0
```

📌 This installs Ansible under the Python site-packages directory.

---

### ✅ Verify Ansible Installation

Check Ansible version:

```bash
ansible --version
```

Expected output includes:

* Ansible version: `4.9.0`
* Python version
* Executable path

---

### ✅ Ensure Ansible Is Available Globally

Check where Ansible binary is located:

```bash
which ansible
```

If required, add pip binary path to system-wide PATH:

```bash
echo 'export PATH=$PATH:/usr/local/bin' | sudo tee -a /etc/profile
```

Reload profile:

```bash
source /etc/profile
```

---

### ✅ Verify Global Availability (All Users)

Switch to another user:

```bash
su - someuser
```

Verify Ansible is accessible:

```bash
ansible --version
```

---

### ✅ Verify Ansible Playbook Command

```bash
ansible-playbook --version
```

---

## 📌 Real-World Use Cases

* Using a jump host as an Ansible controller
* Automating configuration across app servers
* Managing infrastructure without agents
* Running ad-hoc commands and playbooks
* CI/CD and infrastructure automation

---

## 🧠 Key Learnings

* Ansible requires **Python and SSH**, nothing else
* Installing via `pip3` allows version control
* Ensuring PATH correctness makes Ansible globally usable
* Version verification is critical before automation
* Ansible installation is the **first step toward automation**
