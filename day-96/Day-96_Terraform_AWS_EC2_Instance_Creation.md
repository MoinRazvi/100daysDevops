
# 🚀 Day 96 — Launching AWS EC2 Instance with Terraform

## 📌 Objective

Provisioned an AWS EC2 instance named **xfusion-ec2** in **us-east-1** using Terraform.

This task introduced compute provisioning along with secure access configuration using an RSA key pair.

---

# 📋 Task Requirement

Create an EC2 instance with:

| Requirement    | Value                 |
| -------------- | --------------------- |
| Name Tag       | xfusion-ec2           |
| AMI            | ami-0c101f26f147fa7fd |
| Instance Type  | t2.micro              |
| Key Pair       | xfusion-kp            |
| Security Group | default               |
| Region         | us-east-1             |

---

# 📂 Project Structure

```bash
/home/bob/terraform/
├── provider.tf
└── main.tf
```

---

# ⚙️ Existing Provider Configuration

Already available in:

## **provider.tf**

```hcl
provider "aws" {
  region = "us-east-1"
}
```

---

# 🛠️ Initial Terraform Configuration

## First Attempt

```hcl
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = ["default"]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

---

# ❌ Error Encountered

## Terraform Apply Failed

```bash
Error: creating EC2 Instance

InvalidGroup.NotFound:
The security group 'default' does not exist
```

---

# 🔍 Root Cause Analysis

The issue was caused by:

```hcl
vpc_security_group_ids = ["default"]
```

### Why it failed

`vpc_security_group_ids` expects **Security Group IDs**

Example:

```text
sg-0abc123456
```

But:

```text
default
```

is the **Security Group name**, not its ID.

---

# ✅ Solution Applied

Replaced:

```hcl
vpc_security_group_ids = ["default"]
```

With:

```hcl
security_groups = ["default"]
```

---

# 🛠️ Final Working Configuration

## **main.tf**

```hcl
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "aws_instance" "xfusion_ec2" {
  ami             = "ami-0c101f26f147fa7fd"
  instance_type   = "t2.micro"
  key_name        = aws_key_pair.xfusion_kp.key_name
  security_groups = ["default"]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

---

# 🔍 Configuration Breakdown

## 🔹 Key Pair Resource

Creates AWS key pair:

```hcl
resource "aws_key_pair"
```

Used for secure SSH access.

---

## 🔹 Public Key Import

```hcl
file("~/.ssh/id_rsa.pub")
```

Terraform uploads the local SSH public key to AWS.

---

## 🔹 EC2 Resource

```hcl
resource "aws_instance"
```

Launches virtual machine.

---

## 🔹 AMI

```hcl
ami-0c101f26f147fa7fd
```

Amazon Linux image.

---

## 🔹 Instance Type

```hcl
t2.micro
```

Free-tier eligible instance.

---

## 🔹 Default Security Group

```hcl
security_groups = ["default"]
```

Attaches instance to AWS default SG.

---

# ⚙️ Execution Steps

## Step 1 — Generate RSA Key

```bash
ssh-keygen -t rsa
```

Generated:

```bash
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

## Step 2 — Initialize Terraform

```bash
cd /home/bob/terraform
terraform init
```

---

## Step 3 — Validate Configuration

```bash
terraform validate
```

Output:

```text
Success! The configuration is valid.
```

---

## Step 4 — Review Plan

```bash
terraform plan
```

Expected:

```text
Plan: 2 to add
```

---

## Step 5 — Apply Infrastructure

```bash
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Logs

## First Failed Apply

```bash
Error: InvalidGroup.NotFound
The security group 'default' does not exist
```

---

## Fixed Configuration Apply

```bash
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## Verify Terraform State

```bash
terraform state list
```

Output:

```bash
aws_key_pair.xfusion_kp
aws_instance.xfusion_ec2
```

---

# ☁️ AWS Resources Created

Successfully provisioned:

✅ RSA Key Pair
✅ EC2 Instance
✅ Amazon Linux AMI
✅ Default Security Group Attachment
✅ Tagged Compute Resource

---

# 🧠 Key Terraform Concepts Learned

## Key Pair Management

Terraform can import existing SSH keys.

---

## EC2 Provisioning

Infrastructure compute resource creation.

---

## Security Group Attachment

Difference between:

### Security Group Name

```hcl
security_groups
```

### Security Group ID

```hcl
vpc_security_group_ids
```

---

## Declarative Compute Deployment

Compute infrastructure defined as code.

---

# 🎯 Key Takeaways

✅ EC2 instances can be provisioned declaratively
✅ RSA key pairs enable secure access
✅ Security group name vs ID matters
✅ Terraform simplifies compute deployment
✅ Error debugging improves infrastructure understanding

---
