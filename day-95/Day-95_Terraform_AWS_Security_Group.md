
# 🚀 Day 95 — Creating AWS Security Group with Terraform

## 📌 Objective

Provisioned an AWS Security Group named **nautilus-sg** under the **default VPC** in **us-east-1** using Terraform.

This task focused on implementing **network-level access control** for Nautilus application servers.

---

# 📋 Task Requirement

Create a Security Group with:

### **Name**

```text
nautilus-sg
```

### **Description**

```text
Security group for Nautilus App Servers
```

### **Inbound Rules**

## Rule 1 — HTTP

| Property | Value     |
| -------- | --------- |
| Type     | HTTP      |
| Port     | 80        |
| Protocol | TCP       |
| Source   | 0.0.0.0/0 |

---

## Rule 2 — SSH

| Property | Value     |
| -------- | --------- |
| Type     | SSH       |
| Port     | 22        |
| Protocol | TCP       |
| Source   | 0.0.0.0/0 |

---

---

# ⚙️ Existing Provider Configuration

Already available in:

## **provider.tf**

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Key Learning

Terraform automatically merges all `.tf` files in the same directory.

So only **main.tf** needed to be created.

---

# 🛠️ Terraform Configuration

## **main.tf**

```hcl
resource "aws_security_group" "nautilus_sg" {
  name        = "nautilus-sg"
  description = "Security group for Nautilus App Servers"

  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "nautilus-sg"
  }
}
```

---

# 🔍 Configuration Breakdown

# 🔹 Resource Block

Defines AWS Security Group resource.

```hcl
resource "aws_security_group"
```

---

# 🔹 Security Group Name

```hcl
nautilus-sg
```

Visible in AWS Console.

---

# 🔹 Description

Provides metadata for identification.

```hcl
Security group for Nautilus App Servers
```

---

# 🔹 Ingress Rule 1 — HTTP

Allows web traffic.

```hcl
from_port = 80
to_port   = 80
```

---

# 🔹 Ingress Rule 2 — SSH

Allows remote access.

```hcl
from_port = 22
to_port   = 22
```

---

# 🔹 Open CIDR Access

```hcl
0.0.0.0/0
```

Allows access from anywhere.

---

# ⚠️ Important Lab Concept

The task specifically required:

**Create Security Group under default VPC**

Therefore:

❌ No need to specify:

```hcl
vpc_id
```

Terraform automatically associates it with the default VPC.

---

# ⚙️ Execution Steps

## Step 1 — Initialize Terraform

```bash
cd /home/bob/terraform
terraform init
```

---

## Step 2 — Validate Configuration

```bash
terraform validate
```

Output:

```text
Success! The configuration is valid.
```

---

## Step 3 — Review Plan

```bash
terraform plan
```

Expected:

```text
Plan: 1 to add, 0 to change, 0 to destroy
```

---

## Step 4 — Apply Configuration

```bash
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Logs

## Terraform Apply Output

```bash
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## Verify Terraform State

```bash
terraform state list
```

Output:

```bash
aws_security_group.nautilus_sg
```

---

# ☁️ AWS Resource Created

Successfully provisioned:

✅ Security Group
✅ HTTP Access Rule
✅ SSH Access Rule
✅ Default VPC Association
✅ Tagged Resource

---

# 🧠 Core Terraform Concepts Learned

## Security Groups

AWS virtual firewall controlling traffic.

---

## Ingress Rules

Control incoming traffic.

---

## Declarative Security Configuration

Infrastructure access defined as code.

---

## State Tracking

Terraform tracks resource lifecycle.

---

# 🌐 Why This Matters

Security Groups are essential for:

* Web server accessibility
* Secure SSH management
* Controlled infrastructure exposure
* Cloud networking security

---

# 🎯 Day 95 Key Takeaways

✅ Security Groups act as virtual firewalls
✅ Terraform simplifies firewall automation
✅ Default VPC resources can be created without specifying `vpc_id`
✅ Multiple ingress rules can be defined declaratively
✅ Infrastructure security becomes repeatable and version-controlled

---
