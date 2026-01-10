# 🚀 Day 12 of 100 | Apache Troubleshooting & Network Debugging

## 🎯 Objective

Troubleshoot and resolve an issue where **Apache was not reachable from a remote jump host**, despite working locally.
The goal was to restore connectivity **without compromising security** and validate access using CLI tools.

---

## 🧩 Problem Statement

Monitoring reported that **Apache was not reachable** on a specific port from the **jump host**.
Possible causes included:

* Apache service failure
* Port misconfiguration
* Firewall or iptables blocking
* SELinux restrictions
* Port conflicts with other services

The application worked on some servers but failed on one (`stapp01`).

---

## 🏗️ Environment Details

| Component   | Value                           |
| ----------- | ------------------------------- |
| Jump Host   | `jumphost`                      |
| App Servers | `stapp01`, `stapp02`, `stapp03` |
| Service     | Apache (`httpd`)                |
| Port Used   | `3001`                          |
| OS Firewall | `iptables`                      |
| SELinux     | Disabled                        |

---

## 🛠️ Tools & Commands Used

* `curl`
* `telnet`
* `ss`
* `netstat`
* `systemctl`
* `journalctl`
* `iptables`
* `grep`
* `vi`

---

## 🧪 Troubleshooting Process (Step-by-Step)

### ✅ Step 1: Verify Issue from Jump Host

```bash
curl http://stapp01:3001
```

❌ Result:

```text
No route to host
```

---

### ✅ Step 2: Verify Apache Locally on App Server

```bash
curl http://localhost:3001
```

✔ Worked → Apache was running and serving content.

---

### ✅ Step 3: Check Apache Service Status

```bash
sudo systemctl status httpd
```

Initially failed with:

```text
Address already in use: could not bind to 0.0.0.0:3001
```

---

### ✅ Step 4: Identify Port Conflict

```bash
sudo ss -tulnp | grep 3001
```

Found:

```text
127.0.0.1:3001 LISTEN sendmail
```

📌 **Root cause:** `sendmail` was already using port `3001`.

---

### ✅ Step 5: Resolve Port Conflict

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

Restart Apache:

```bash
sudo systemctl start httpd
```

Verify:

```bash
sudo ss -tulnp | grep httpd
```

---

### ✅ Step 6: Confirm Apache Binding

```bash
tcp LISTEN 0.0.0.0:3001 httpd
```

✔ Apache bound correctly to all interfaces.

---

### ✅ Step 7: Check SELinux Status

```bash
getenforce
```

Output:

```text
Disabled
```

✔ SELinux ruled out.

---

### ✅ Step 8: Identify Firewall Layer

```bash
firewall-cmd --state
```

Output:

```text
command not found
```

✔ `firewalld` not installed.

---

### ✅ Step 9: Inspect iptables (Root Cause)

```bash
sudo iptables -L -n
```

Found **REJECT rules**, causing:

```text
No route to host
```

---

### ✅ Step 10: Allow Apache Port via iptables (Safe Fix)

```bash
sudo iptables -I INPUT -p tcp --dport 3001 -j ACCEPT
```

✔ Did not flush rules
✔ Did not weaken security

---

### ✅ Step 11: Final Verification from Jump Host

```bash
curl http://stapp01:3001
```

🎉 **Success — Apache reachable remotely**

---

## 📌 Real-World Use Cases

* Production incident response
* Network-level debugging
* Apache service recovery
* Multi-layer Linux troubleshooting
* Security-aware fixes (no blanket disabling)

---

## 🧠 Key Learnings

* Local service access ≠ remote accessibility
* `No route to host` usually indicates **iptables REJECT**
* Port conflicts can silently break services
* Always check **who owns the port**
* Apache can fail even if config is correct
* firewalld and iptables are **independent layers**
* Never disable security controls blindly
* CLI tools are essential for real DevOps work
