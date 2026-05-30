# 🚀 Day 97 — Creating AWS IAM Policy with Terraform

## 📌 Objective

Provisioned an AWS IAM policy named **iampolicy_jim** in **us-east-1** using Terraform.

This task introduced **AWS Identity and Access Management (IAM)** concepts and policy-based access control for EC2 console read-only permissions.

---

# 📋 Task Requirement

Create an IAM policy with:

| Requirement | Value                           |
| ----------- | ------------------------------- |
| Policy Name | iampolicy_jim                   |
| Region      | us-east-1                       |
| Access Type | Read-only                       |
| Service     | Amazon EC2                      |
| Permissions | View Instances, AMIs, Snapshots |

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

# 🛠️ Terraform Configuration

## **main.tf**

```hcl
resource "aws_iam_policy" "iampolicy_jim" {
  name        = "iampolicy_jim"
  description = "Read-only access to EC2 console"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots"
        ]
        Resource = "*"
      }
    ]
  })
}
```

---

# 🔍 Configuration Breakdown

## 🔹 IAM Policy Resource

Defines an AWS IAM policy.

```hcl
resource "aws_iam_policy"
```

---

## 🔹 Policy Name

```hcl
iampolicy_jim
```

AWS identifier for the policy.

---

## 🔹 Description

```hcl
Read-only access to EC2 console
```

Helps identify policy purpose.

---

## 🔹 jsonencode()

Used to convert Terraform HCL into valid IAM JSON policy syntax.

```hcl
jsonencode()
```

---

## 🔹 Policy Version

```hcl
2012-10-17
```

Standard AWS IAM policy version.

---

## 🔹 Effect

```hcl
Allow
```

Grants permission.

---

## 🔹 Resource

```hcl
"*"
```

Applies permission across all EC2 resources.

---

# 🔐 IAM Permissions Granted

## View EC2 Instances

```hcl
ec2:DescribeInstances
```

Allows listing EC2 instances.

---

## View AMIs

```hcl
ec2:DescribeImages
```

Allows viewing machine images.

---

## View Snapshots

```hcl
ec2:DescribeSnapshots
```

Allows viewing EBS snapshots.

---

# ⚙️ Execution Steps

## Step 1 — Navigate to Working Directory

```bash
cd /home/bob/terraform
```

---

## Step 2 — Initialize Terraform

```bash
terraform init
```

Output:

```text
Terraform has been successfully initialized!
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
Plan: 1 to add, 0 to change, 0 to destroy
```

---

## Step 5 — Apply Infrastructure

```bash
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Logs

## Terraform Apply Output

```bash
aws_iam_policy.iampolicy_jim: Creating...

aws_iam_policy.iampolicy_jim: Creation complete
```

---

## Apply Completion

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
aws_iam_policy.iampolicy_jim
```

---

# ☁️ AWS Resource Created

Successfully provisioned:

✅ IAM Policy
✅ EC2 Read-only Access
✅ Console View Permissions
✅ Terraform-managed IAM Resource

---

# 🧠 Key Terraform Concepts Learned

## IAM as Code

Permissions can be defined declaratively.

---

## Policy-based Access Control

AWS security managed through reusable policies.

---

## jsonencode()

Simplifies IAM JSON creation.

---

## Fine-grained Permission Design

Granting exact access required.

---

# 🌐 Why This Matters

IAM is foundational for:

* Secure cloud access
* Least privilege principle
* Access governance
* Enterprise-grade AWS security

---

# 🎯 Key Takeaways

✅ IAM policies can be provisioned using Terraform
✅ `jsonencode()` simplifies JSON policy writing
✅ Read-only access improves security posture
✅ Fine-grained permissions follow least privilege
✅ Terraform enables repeatable IAM governance

---

