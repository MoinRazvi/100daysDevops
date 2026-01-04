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

### ✅ Check available shells
```bash
cat /etc/shells
```

---

### ✅ Test
```bash
# run a simple test command (placeholder)
test
```


### ✅ Test1
```bash
# run a simple test command (placeholder)1
test1
```

