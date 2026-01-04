🚀 Day 02 – Temporary Linux User Setup with Expiry
🎯 Objective

Create Linux users with an account expiry date to ensure automatic access removal for temporary users such as contractors, auditors, or interns.

👤 Why Temporary Users?

Temporary users provide time-bound access to systems.
Once the expiry date is reached:

Login access is disabled

SSH access is blocked

Password authentication stops

This improves security, compliance, and access hygiene in production environments.

🛠️ Commands Used

useradd

passwd

chage

id

usermod

🧪 Hands-on Commands Used
✅ Create User with Expiry Date
sudo useradd -m -e 2026-01-31 tempuser

✅ Set Password for the User
sudo passwd tempuser

✅ Verify User Creation
id tempuser

✅ Check Account Expiry Details
sudo chage -l tempuser

✅ Add Expiry Date to Existing User
sudo useradd -m tempuser2
sudo passwd tempuser2
sudo chage -E 2026-01-31 tempuser2

✅ Remove Expiry Date (Extend Access)
sudo chage -E -1 tempuser

✅ Lock and Unlock User Account
sudo usermod -L tempuser
sudo usermod -U tempuser

📌 Real-World Use Cases

Contractor or vendor access

Audit and compliance activities

Temporary production troubleshooting

Intern or trainee onboarding

🧠 Key Learnings

Account expiry prevents forgotten access

chage manages the full user access lifecycle

Temporary users enforce least privilege

This is a real production DevOps practice
