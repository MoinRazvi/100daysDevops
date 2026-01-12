# 🚀 Day 14 of 100 – Apache Port Conflict Troubleshooting (Production Incident)

## 🎯 Objective

Identify and resolve an Apache startup failure caused by a **port conflict**, ensure Apache is:

* Running successfully on **all app servers**
* Listening on the **required port (8088)**
* Not blocked by other services

This task represents a **real-world production incident scenario** commonly handled by DevOps and Production Support engineers.

---

## 🧩 Problem Statement

Apache service (`httpd`) failed to start on one of the app servers even though:

* Apache was configured to listen on port **8088**
* Firewall rules were already correct

Error observed:

```text
Job for httpd.service failed because the control process exited with error code
```

---

## 🏗️ Environment Details

| Component      | Value                     |
| -------------- | ------------------------- |
| App Servers    | stapp01, stapp02, stapp03 |
| Apache Service | httpd                     |
| Required Port  | 8088                      |
| OS             | Linux                     |
| Firewall       | iptables                  |
| Mail Service   | sendmail                  |

---

## 🛠️ Tools & Commands Used

* `systemctl`
* `ss`
* `netstat`
* `journalctl`
* `grep`
* `apachectl`
* `curl`

---

## 🔍 Step 1: Check Apache Service Status

```bash
sudo systemctl status httpd
```

If Apache fails, restart attempt shows:

```bash
sudo systemctl restart httpd
```

---

## 🔍 Step 2: Check Apache Error Logs

```bash
sudo journalctl -xe | tail -20
```

Typical error:

```text
Address already in use
no listening sockets available
```

---

## 🔍 Step 3: Identify Which Service Is Using Port 8088

```bash
sudo ss -tulnp | grep 8088
```

or

```bash
sudo netstat -tulnp | grep 8088
```

### Output observed:

```text
127.0.0.1:8088 → sendmail
```

➡️ This confirms **sendmail** was occupying port **8088**, preventing Apache from starting.

---

## 🔧 Step 4: Stop and Disable sendmail

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

Verify port is free:

```bash
sudo ss -tulnp | grep 8088
```

✅ No output = port released

---

## 🔧 Step 5: Validate Apache Configuration

Check for duplicate `Listen` directives:

```bash
sudo grep -R "Listen 8088" /etc/httpd/
```

Ensure only **one** line exists:

```text
Listen 8088
```

Validate config syntax:

```bash
sudo apachectl configtest
```

Expected:

```text
Syntax OK
```

---

## 🚀 Step 6: Start and Enable Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Verify:

```bash
sudo systemctl status httpd
```

---

## 🔍 Step 7: Confirm Apache Is Listening on Port 8088

```bash
sudo ss -tulnp | grep httpd
```

Expected:

```text
LISTEN 0 128 0.0.0.0:8088 ... httpd
```

---

## 🧪 Step 8: Test from Jump Host

```bash
curl http://stapp01:8088
curl http://stapp02:8088
curl http://stapp03:8088
```

✅ Apache reachable
⚠️ Empty response is acceptable (no code deployed yet)

---

## 📌 Real-World Use Cases

* Resolving service startup failures
* Diagnosing port conflicts
* Handling production incidents
* Apache troubleshooting
* Service ownership verification

---

## 🧠 Key Learnings (Very Important)

* “Service failed” often means **port conflict**
* Apache cannot bind to a port already in use
* `ss -tulnp` and `netstat -tulnp` are critical tools
* Always verify **who owns the port**
* Mail services (sendmail) often bind unexpected ports
* Config validation (`apachectl configtest`) prevents downtime
* Logs tell the truth — always check them

---

## ✅ Outcome

✔ Identified faulty app server
✔ Discovered sendmail port conflict
✔ Freed required port
✔ Apache started successfully
✔ Apache running on port 8088 on all app servers

---
<img width="482" height="277" alt="image" src="https://github.com/user-attachments/assets/b8cfe2f9-2011-46de-8034-fdc40037a047" />

