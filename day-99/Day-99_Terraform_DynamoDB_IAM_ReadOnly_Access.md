# 🚀 Day 99 — Securing DynamoDB with Terraform IAM Policies

## 📌 Objective

Provisioned secure AWS infrastructure using Terraform by creating:

✅ DynamoDB Table
✅ IAM Role
✅ Read-only IAM Policy
✅ Policy Attachment to Role
✅ Variable-driven configuration

This task focused on implementing **fine-grained AWS access control** using Infrastructure as Code.

---

# 📋 Task Requirements

Provision the following resources:

| Resource       | Name                   |
| -------------- | ---------------------- |
| DynamoDB Table | devops-table           |
| IAM Role       | devops-role            |
| IAM Policy     | devops-readonly-policy |

---

# 🔐 Access Requirements

The IAM policy must allow only:

```text id="ph03yy"
GetItem
Scan
Query
```

Restricted only to:

```text id="jlmw73"
devops-table
```

---

# 📂 Project Structure

```bash id="6q2v4w"
/home/bob/terraform/
├── provider.tf
├── variables.tf
├── terraform.tfvars
├── main.tf
└── outputs.tf
```

---

# ⚙️ Variable Definitions

## **variables.tf**

```hcl id="dnlzqk"
variable "KKE_TABLE_NAME" {}
variable "KKE_ROLE_NAME" {}
variable "KKE_POLICY_NAME" {}
```

---

# 📝 Variable Values

## **terraform.tfvars**

```hcl id="1v9pna"
KKE_TABLE_NAME  = "devops-table"
KKE_ROLE_NAME   = "devops-role"
KKE_POLICY_NAME = "devops-readonly-policy"
```

---

# 🛠️ Terraform Infrastructure Code

## **main.tf**

```hcl id="1r2wso"
resource "aws_dynamodb_table" "devops_table" {
  name         = var.KKE_TABLE_NAME
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "id"

  attribute {
    name = "id"
    type = "S"
  }
}

resource "aws_iam_role" "devops_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
}

resource "aws_iam_policy" "readonly_policy" {
  name = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Action = [
        "dynamodb:GetItem",
        "dynamodb:Scan",
        "dynamodb:Query"
      ]
      Resource = aws_dynamodb_table.devops_table.arn
    }]
  })
}

resource "aws_iam_role_policy_attachment" "attach_policy" {
  role       = aws_iam_role.devops_role.name
  policy_arn = aws_iam_policy.readonly_policy.arn
}
```

---

# 📤 Outputs Configuration

## **outputs.tf**

```hcl id="x8v38p"
output "kke_dynamodb_table" {
  value = aws_dynamodb_table.devops_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.devops_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.readonly_policy.name
}
```

---

# 🔍 Resource Breakdown

---

## 🔹 DynamoDB Table

Creates a NoSQL table.

Configuration:

```hcl id="gmy5h2"
billing_mode = "PAY_PER_REQUEST"
```

Benefits:

* Serverless
* Auto-scaling
* Cost-efficient

---

## 🔹 IAM Role

Defines AWS trusted identity.

Trusted Service:

```hcl id="wvx7hb"
ec2.amazonaws.com
```

Allows EC2 instances to assume this role.

---

## 🔹 IAM Read-only Policy

Grants minimal DynamoDB access.

Allowed Actions:

```hcl id="a0j9w8"
dynamodb:GetItem
dynamodb:Scan
dynamodb:Query
```

---

## 🔹 Policy Attachment

Binds policy to IAM role.

This enables the role to securely access DynamoDB.

---

# ⚙️ Execution Steps

## Step 1 — Navigate to Directory

```bash id="9lwn8n"
cd /home/bob/terraform
```

---

## Step 2 — Initialize Terraform

```bash id="n7l1tt"
terraform init
```

---

## Step 3 — Format Files

```bash id="0f4nph"
terraform fmt
```

---

## Step 4 — Validate

```bash id="d8n7y7"
terraform validate
```

Output:

```text id="hx4n1w"
Success! The configuration is valid.
```

---

## Step 5 — Review Plan

```bash id="xv3g5r"
terraform plan
```

Expected:

```text id="pb2n1s"
Plan: 4 to add
```

---

## Step 6 — Apply Infrastructure

```bash id="z6k4v2"
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Logs

## Terraform Plan Output

```bash id="1g8i1g"
Terraform will perform the following actions:

+ aws_dynamodb_table.devops_table
+ aws_iam_role.devops_role
+ aws_iam_policy.readonly_policy
+ aws_iam_role_policy_attachment.attach_policy
```

---

## Apply Output

```bash id="6ynp8z"
Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

---

## Output Verification

```bash id="h5v7i9"
terraform output
```

Expected:

```bash id="f2w2jk"
kke_dynamodb_table = "devops-table"
kke_iam_role_name = "devops-role"
kke_iam_policy_name = "devops-readonly-policy"
```

---

## Final Validation

```bash id="3phs9g"
terraform plan
```

Output:

```bash id="wkg73n"
No changes. Your infrastructure matches the configuration.
```

---

# ☁️ AWS Resources Created

Successfully provisioned:

✅ DynamoDB Table
✅ IAM Role
✅ Read-only Policy
✅ Role-Policy Attachment

---

# 🧠 Key Terraform Concepts Learned

## Variables

Reusable configuration values.

---

## terraform.tfvars

Externalized variable assignment.

---

## IAM Fine-Grained Access Control

Restrict permissions precisely.

---

## Resource Referencing

Used resource ARN dynamically.

---

## Least Privilege Principle

Grant only required permissions.

---

# 🔐 Security Concept Learned

Implemented **Principle of Least Privilege**

The IAM policy allows:

✅ Read access only

Blocks:

❌ Write
❌ Delete
❌ Update

This strengthens AWS security posture.

---

# 🎯 Key Takeaways

✅ Built secure DynamoDB infrastructure
✅ Created IAM Role with trust policy
✅ Attached restricted read-only access
✅ Used Terraform variables and outputs
✅ Implemented fine-grained AWS access control

---
