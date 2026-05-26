# 🚀 Day 94 — Terraform AWS VPC Creation on AWS

## 📌 Objective

Provisioned an AWS VPC named **xfusion-vpc** in **us-east-1** using Terraform as part of the Terraform phase of my **100 Days of DevOps** journey.

This marks the transition from **Configuration Management (Ansible)** to **Infrastructure Provisioning (Terraform)**.

---

## 📋 Task Requirement

Create a VPC named:

```text
xfusion-vpc
```

Using:

* **Terraform**
* Region: **us-east-1**
* Working directory:

```text
/home/bob/terraform
```

Create only:

```text
main.tf
```

---

# 📜 Existing Configuration

The environment already contained:

## **provider.tf**

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Important Learning

Terraform automatically loads all `.tf` files in the same directory.

That means:

* `provider.tf`
* `main.tf`

are merged into one execution plan.

So there was **no need to redefine provider configuration in main.tf**.

---

# 🛠️ main.tf Configuration

## **main.tf**

```hcl
resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "xfusion-vpc"
  }
}
```

---

# 🔍 Configuration Breakdown

## 1️⃣ Resource Block

Defines the AWS resource to create.

```hcl
resource "aws_vpc"
```

---

## 2️⃣ Resource Name

```hcl
xfusion_vpc
```

Terraform internal identifier.

---

## 3️⃣ CIDR Block

```hcl
10.0.0.0/16
```

Provides:

* 65,536 IP addresses
* Private network range
* Standard VPC addressing

---

## 4️⃣ Tagging

```hcl
Name = "xfusion-vpc"
```

Helps identify the VPC inside AWS Console.

---

# ⚙️ Execution Steps

## Initialize Terraform

```bash
cd /home/bob/terraform
terraform init
```

### Output

```text
Terraform has been successfully initialized!
```

---

## Validate Configuration

```bash
terraform validate
```

### Output

```text
Success! The configuration is valid.
```

---

## Review Execution Plan

```bash
terraform plan
```

Expected:

```text
Plan: 1 to add, 0 to change, 0 to destroy
```

---

## Apply Infrastructure

```bash
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Log

## Terraform Apply

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
aws_vpc.xfusion_vpc
```

---

# ☁️ AWS Resource Created

Successfully provisioned:

✅ Custom VPC
✅ Tagged Resource
✅ us-east-1 Deployment
✅ Infrastructure as Code

---

# 🧠 Key Terraform Concepts Learned

## Provider Separation

Keeping provider configuration in separate file:

```text
provider.tf
```

Improves modularity.

---

## Resource Declaration

Infrastructure is declared declaratively.

---

## State Management

Terraform tracks infrastructure using:

```text
terraform.tfstate
```

---

## Execution Workflow

Terraform follows:

```text
Write → Validate → Plan → Apply
```

---

# 🎯 Key Takeaways

✅ Terraform combines all `.tf` files automatically
✅ Avoid duplicate provider definitions
✅ VPC is foundational AWS networking component
✅ Terraform simplifies cloud provisioning
✅ Infrastructure becomes repeatable and version-controlled

---
