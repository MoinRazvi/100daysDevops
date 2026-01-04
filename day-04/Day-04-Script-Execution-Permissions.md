
# 🚀 Day 04 – Script Execution Permissions

## 🎯 Objective

Learn how to **manage script execution permissions** in Linux using `chmod`, understand permission bits, and ensure scripts are executable by intended users — a **core Linux & DevOps skill**.

---

## 📜 Why Script Permissions Matter?

In Linux, a script **will not execute unless execute permission is explicitly granted**.
Incorrect permissions can lead to:

* Script execution failures
* Automation breakdowns
* Security risks from over-permissive access

Proper permissions ensure:

* ✅ Controlled execution
* 🔐 Security and least privilege
* ⚙️ Reliable automation in production

---

## 🛠️ Commands Used

* `ls -l`
* `chmod`
* `stat`
* `id`

---

## 🧪 Hands-on Commands Used

### ✅ Check Current Script Permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

Example output observed:

```text
---x--x--x 1 root root 40 Dec 29 04:52 xfusioncorp.sh
```

---

### ✅ Understand Permission Bits

* First triplet → **Owner**
* Second triplet → **Group**
* Third triplet → **Others**

`x` → execute
`r` → read
`w` → write

---

### ✅ Grant Execute Permission to All Users

```bash
chmod a+x /tmp/xfusioncorp.sh
```

Equivalent numeric mode:

```bash
chmod 111 /tmp/xfusioncorp.sh
```

---

### ✅ Verify Updated Permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected output:

```text
-r-x--x--x 1 root root 40 Dec 29 04:52 xfusioncorp.sh
```

---

### ✅ Make Script Readable and Executable by All (Recommended)

```bash
chmod 755 /tmp/xfusioncorp.sh
```

This results in:

* Owner → read, write, execute
* Group → read, execute
* Others → read, execute

---

### ✅ Execute the Script

```bash
/tmp/xfusioncorp.sh
```

Or:

```bash
sh /tmp/xfusioncorp.sh
```

---

## 📌 Real-World Use Cases

* Running deployment scripts
* Cron job execution
* Automation and backup scripts
* CI/CD pipeline scripts
* Troubleshooting permission denied errors

---

## 🧠 Key Learnings

* Scripts require **execute (`x`) permission** to run
* `chmod` controls who can run a script
* Numeric and symbolic modes both matter
* Over-permissive scripts can be a security risk
* Always verify permissions with `ls -l`

