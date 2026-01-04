# 🚀 Day 02 – Temporary Linux User Setup with Expiry

## 🎯 Objective
Learn how to create **temporary Linux users with an expiration date** — a common **DevOps and system administration practice** to ensure **time-bound access** and automatic cleanup.

---

## 👤 Why Temporary Users?
Temporary users are widely used for:
- Contractors
- Auditors
- Interns
- Short-term production access

Once the expiry date is reached:
- Login access is disabled
- SSH access is blocked
- Password authentication stops

This improves **security, compliance, and access control**.

---

## 🛠️ Commands Used
- `useradd`
- `passwd`
- `chage`
- `id`
- `usermod`

---

## 🧪 Hands-on Commands Used

```bash
# Create a user with expiry date
sudo useradd -m -e 2026-01-31 tempuser

# Set password for the user
sudo passwd tempuser

# Verify user creation
id tempuser

# Check account expiry details
sudo chage -l tempuser

# Add expiry date to an existing user
sudo useradd -m tempuser2
sudo passwd tempuser2
sudo chage -E 2026-01-31 tempuser2

# Remove expiry date (extend access)
sudo chage -E -1 tempuser

# Lock and unlock user account
sudo usermod -L tempuser
sudo usermod -U tempuser
```

---

## 📌 Real-World Use Cases
- Contractor or vendor access
- Audit and compliance activities
- Temporary production troubleshooting
- Intern or trainee onboarding
- Short-term project collaboration

---

## 🧠 Key Learnings
- Temporary users help enforce least privilege
- Account expiry prevents forgotten access
- `chage` is critical for access lifecycle management
- Combining expiry with audits improves system hygiene
- This is a real production practice, not just theory
  
