# 🚀 Day 17 of 100 – PostgreSQL Database & User Setup

## 🎯 Objective

Prepare the **PostgreSQL database layer** for a newly developed application by creating a **dedicated database user and database**, and granting appropriate permissions — without restarting the database service.

This is a **real-world DevOps pre-deployment task**, commonly required before application rollout.

---

## 🧩 Problem Statement

The Nautilus application development team plans to deploy a new application that uses **PostgreSQL**.

PostgreSQL is already installed and running on the Nautilus **Database Server**.
The task was to perform database-level configuration only.

### Requirements:

1. Create a database user `kodekloud_pop`
2. Set password to `B4zNgHA7Ya`
3. Create a database `kodekloud_db3`
4. Grant full permissions on the database to the user
5. ❌ **Do not restart PostgreSQL service**

---

## 🏗️ Environment Details

| Component       | Value              |
| --------------- | ------------------ |
| Server          | Nautilus DB Server |
| Database Engine | PostgreSQL         |
| Database User   | `kodekloud_pop`    |
| Database        | `kodekloud_db3`    |
| Password        | `B4zNgHA7Ya`       |
| Service Restart | Not allowed        |

---

## 🛠️ Tools & Commands Used

* `psql`
* `sudo su - postgres`
* PostgreSQL SQL commands

---

## 🧪 Step-by-Step Implementation

### ✅ Step 1: Switch to PostgreSQL OS User

```bash
sudo su - postgres
```

---

### ✅ Step 2: Enter PostgreSQL Shell

```bash
psql
```

You should see:

```text
postgres=#
```

> This prompt indicates you are **inside PostgreSQL**, ready to run SQL commands.

---

### ✅ Step 3: Create Database User

Run **inside `postgres=#`**:

```sql
CREATE USER kodekloud_pop WITH PASSWORD 'B4zNgHA7Ya';
```

Verify user creation:

```sql
\du
```

---

### ✅ Step 4: Create Database

```sql
CREATE DATABASE kodekloud_db3;
```

Verify:

```sql
\l
```

---

### ✅ Step 5: Grant Privileges on Database

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db3 TO kodekloud_pop;
```

---

### ✅ Step 6: Exit PostgreSQL

Exit psql:

```sql
\q
```

Exit postgres OS user:

```bash
exit
```

---

## 🔍 Validation (Optional but Recommended)

```bash
psql -U kodekloud_pop -d kodekloud_db3 -h localhost
```

✔ Successful login confirms:

* User exists
* Database exists
* Permissions are correct

---

## 🚧 Constraints Followed

* ❌ PostgreSQL service **not restarted**
* ✅ All changes applied dynamically
* ✅ No disruption to existing services

---

## 📌 Real-World Use Cases

* Application database onboarding
* Secure database access setup
* Pre-deployment environment preparation
* Separation of database privileges
* Production-safe database operations

---

## 🧠 Key Learnings

* PostgreSQL users and databases can be created **without restarting the service**
* SQL commands must be executed inside the `psql` shell
* Application-specific DB users improve security
* Permission management is critical for stable deployments
* Proper validation prevents deployment-time failures

---

## ✅ Outcome

✔ Database user created successfully
✔ Database created and accessible
✔ Correct permissions granted
✔ PostgreSQL service left untouched
✔ Environment ready for application deployment

---

<img width="482" height="276" alt="image" src="https://github.com/user-attachments/assets/73373106-8fd7-4eac-aac6-66038b88d345" />
