# 🚀 Day 01 – Linux User Setup with Non-Interactive Shell

## 🎯 Objective
Learn how to create Linux users with **non-interactive shells** to improve system security by preventing direct login access.  
This is a **common real-world DevOps and system administration practice** for service and application accounts.

---

## 👤 What is a Non-Interactive Shell?
A non-interactive shell is a shell that **does not allow users to log in via SSH or terminal**.

It is commonly used for:
- Application users
- Service accounts
- Background processes
- Automation tools

This enforces the **Principle of Least Privilege**.

---

## 🔐 Common Non-Interactive Shells
| Shell Path | Purpose |
|-----------|--------|
| `/sbin/nologin` | Displays a message and denies login |
| `/bin/false` | Immediately terminates the session |

---

## 🛠️ Hands-on Commands Used

### ✅ Check Available Shells
```bash
cat /etc/shells


### **✅ Create a User with /sbin/nologin**
```bash
sudo useradd -m -s /sbin/nologin appuser


-m → Creates home directory

-s → Assigns login shell

✅ Create a User with /bin/false
sudo useradd -m -s /bin/false serviceuser

✅ Verify User Shell
grep appuser /etc/passwd


Example output:

appuser:x:1002:1002::/home/appuser:/sbin/nologin

✅ Attempt SSH Login (Expected to Fail)
ssh appuser@localhost


Expected result:

This account is currently not available.

✅ Change Existing User Shell to Non-Interactive
sudo usermod -s /sbin/nologin existinguser

✅ Lock User Account (Optional Hardening)
sudo passwd -l appuser

✅ Check User Login Status
sudo passwd -S appuser

✅ Switch User Test (Will Fail)
su - appuser

📌 Real-World Use Cases

Database service accounts (e.g., mysql, postgres)

Application runtime users

CI/CD agents

Monitoring and logging services

Security-hardened production systems

🧠 Key Learnings

Not all users need shell access

/sbin/nologin is preferred for clarity

Reduces attack surface

Essential Linux hardening practice

Frequently used in enterprise environments
