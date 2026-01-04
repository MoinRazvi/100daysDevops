# ⏱️ Day 06 – Linux Cron Jobs

## 🎯 Objective

Learn how to **schedule recurring tasks using Linux cron**, install and manage the cron service, and create cron jobs for users — a **fundamental automation skill for DevOps and system administrators**.

---

## 🕒 Why Cron Jobs Matter?

Cron allows you to **automate repetitive tasks** without manual intervention.

Common use cases include:

* Log cleanup
* Backups
* Health checks
* Script automation
* Periodic maintenance jobs

Cron ensures tasks run **reliably and consistently** in production systems.

---

## 🛠️ Commands Used

* `yum / dnf`
* `systemctl`
* `crontab`
* `cat`
* `ls`

---

## 🧪 Hands-on Commands Used

### ✅ Install Cron Service (cronie)

```bash
sudo yum install -y cronie
```

(On newer systems)

```bash
sudo dnf install -y cronie
```

---

### ✅ Start and Enable Cron Service

Start the cron daemon:

```bash
sudo systemctl start crond
```

Enable cron at boot:

```bash
sudo systemctl enable crond
```

Verify cron service status:

```bash
sudo systemctl status crond
```

---

### ✅ Create a Cron Job for Root User

Edit root’s crontab:

```bash
sudo crontab -e
```

Add the following entry:

```text
*/5 * * * * echo hello > /tmp/cron_text
```

📌 This job writes `hello` to `/tmp/cron_text` every **5 minutes**.

---

### ✅ Verify Cron Job Configuration

List root’s cron jobs:

```bash
sudo crontab -l
```

---

### ✅ Verify Cron Job Execution

Check the output file:

```bash
cat /tmp/cron_text
```

Confirm the file exists:

```bash
ls -l /tmp/cron_text
```

---

## 📌 Real-World Use Cases

* Automating backups and cleanups
* Running monitoring scripts
* Scheduling reports
* Periodic application maintenance
* Infrastructure housekeeping tasks

---

## 🧠 Key Learnings

* Cron enables **time-based automation**
* `cronie` provides the cron service on RHEL-based systems
* `crond` must be running for jobs to execute
* Root cron jobs require `sudo crontab -e`
* Always verify cron jobs using `crontab -l`
* Output redirection helps validate cron execution
