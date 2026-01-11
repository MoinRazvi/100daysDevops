# 🚀 Day 13 of 100 – Securing Apache with iptables (Production Firewall Hardening)

## 🎯 Objective

Secure an Apache application port by implementing **iptables-based firewall rules** that:

* Restrict access to a specific source (LBR host only)
* Block all other incoming traffic
* Ensure rules persist after reboot
* Maintain SSH access at all times

This task simulates **real-world production firewall hardening**.

---

## 🧩 Problem Statement

Apache was running on a **custom port (5000)** on all app servers.
There was **no firewall** installed, leaving the port **open to everyone**.

### Requirements:

1. Install `iptables` and dependencies on all app servers
2. Allow Apache port **5000** access **only from LBR host**
3. Block access from all other hosts
4. Ensure firewall rules **persist after reboot**
5. Do **not lock yourself out** (SSH must remain accessible)

---

## 🏗️ Environment Details

| Component     | Value                     |
| ------------- | ------------------------- |
| App Servers   | stapp01, stapp02, stapp03 |
| Load Balancer | LBR                       |
| LBR IP        | `172.16.238.14`           |
| Apache Port   | `5000`                    |
| SSH Port      | `22`                      |
| Firewall Tool | iptables                  |
| firewalld     | Not installed             |

---

## 🛠️ Tools & Commands Used

* `iptables`
* `iptables-services`
* `systemctl`
* `curl`
* `ssh`
* `iptables -L --line-numbers`

---

## 🔐 Firewall Design (Golden Rule)

> **Allow first → Restrict later → Reject last**

Correct firewall logic:

1. Allow established connections
2. Allow SSH (port 22)
3. Allow Apache only from LBR
4. Reject everything else

---

## 🧪 Step-by-Step Implementation (Safe Order)

### ✅ Step 1: Install iptables (on all app servers)

```bash
sudo yum install -y iptables-services
sudo systemctl enable iptables
sudo systemctl start iptables
```

---

### ✅ Step 2: Allow existing connections (critical)

```bash
sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
```

---

### ✅ Step 3: Allow SSH (DO NOT SKIP)

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

### ✅ Step 4: Set default policy to DROP

```bash
sudo iptables -P INPUT DROP
```

---

### ✅ Step 5: Allow Apache ONLY from LBR

```bash
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 5000 -j ACCEPT
```

---

### ✅ Step 6: Reject all other traffic (LAST rule)

```bash
sudo iptables -A INPUT -j REJECT --reject-with icmp-host-prohibited
```

---

## 🔍 Final Verification (MANDATORY)

```bash
sudo iptables -L INPUT -n --line-numbers
```

### ✅ Correct Final Table

```
Chain INPUT (policy DROP)
1 ACCEPT  state RELATED,ESTABLISHED
2 ACCEPT  tcp dpt:22
3 ACCEPT  tcp 172.16.238.14 dpt:5000
4 REJECT  all
```

⚠️ There must be **NO** rule like:

```
ACCEPT all -- 0.0.0.0/0
```

---

## 🧪 Testing

### From LBR host

```bash
curl http://stapp01:5000
```

✅ Works

### From jump host

```bash
curl http://stapp01:5000
```

❌ Blocked

### SSH access check

```bash
ssh tony@stapp01
```

✅ Works

---

## 💾 Persistence After Reboot

```bash
sudo service iptables save
```

Rules saved to:

```text
/etc/sysconfig/iptables
```

---

## 📌 Real-World Use Cases

* Securing internal application ports
* Allowing traffic only from load balancers
* Preventing lateral movement in data centers
* Meeting security & compliance standards
* Production incident response hardening

---

## 🧠 Key Learnings (Most Important)

* iptables is **first-match-wins**
* One `ACCEPT all` rule breaks all security
* `REJECT` must always be **last**
* SSH must be allowed **before** DROP/REJECT
* Firewall changes apply **immediately**
* Duplicate rules cause confusion
* Always verify with `--line-numbers`
* Never rush firewall changes in production

---

## ✅ Outcome

✔ Apache secured
✔ Access limited to LBR only
✔ SSH preserved
✔ Rules persistent
✔ No security compromised

---
<img width="470" height="274" alt="image" src="https://github.com/user-attachments/assets/5a43d0b4-238a-4051-b2e8-03e2b7b6d959" />

