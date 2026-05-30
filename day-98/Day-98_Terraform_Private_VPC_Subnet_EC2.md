# 🚀 Day 98 — Building Private AWS Infrastructure with Terraform

## 📌 Objective

Provisioned a fully isolated AWS private infrastructure using Terraform by creating:

✅ Private VPC
✅ Private Subnet
✅ Security Group (Internal-only access)
✅ EC2 Instance inside private subnet
✅ Variables and Outputs management

This task focused on designing secure cloud networking using Infrastructure as Code.

---

# 📋 Task Requirements

Create:

| Resource | Name                   |
| -------- | ---------------------- |
| VPC      | datacenter-priv-vpc    |
| Subnet   | datacenter-priv-subnet |
| EC2      | datacenter-priv-ec2    |
| Region   | us-east-1              |

---

## Networking Requirements

### VPC CIDR

```text
10.0.0.0/16
```

### Subnet CIDR

```text
10.0.1.0/24
```

### Security Rule

Allow access **only within VPC CIDR**

```text
10.0.0.0/16
```

---

# 📂 Project Structure

```bash
/home/bob/terraform/
├── provider.tf
├── variables.tf
├── main.tf
└── outputs.tf
```

---

# ⚙️ Variable Configuration

## **variables.tf**

```hcl
variable "KKE_VPC_CIDR" {
  default = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  default = "10.0.1.0/24"
}
```

---

# 🛠️ Terraform Infrastructure Code

## **main.tf**

```hcl
resource "aws_vpc" "datacenter_vpc" {
  cidr_block = var.KKE_VPC_CIDR

  tags = {
    Name = "datacenter-priv-vpc"
  }
}

resource "aws_subnet" "datacenter_subnet" {
  vpc_id                  = aws_vpc.datacenter_vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false

  tags = {
    Name = "datacenter-priv-subnet"
  }
}

resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-private-sg"
  description = "Private access only"
  vpc_id      = aws_vpc.datacenter_vpc.id

  ingress {
    from_port   = 0
    to_port     = 65535
    protocol    = "tcp"
    cidr_blocks = [var.KKE_VPC_CIDR]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "datacenter_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.datacenter_subnet.id
  vpc_security_group_ids = [aws_security_group.datacenter_sg.id]

  tags = {
    Name = "datacenter-priv-ec2"
  }
}
```

---

# 📤 Outputs Configuration

## **outputs.tf**

```hcl
output "KKE_vpc_name" {
  value = aws_vpc.datacenter_vpc.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.datacenter_subnet.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.datacenter_ec2.tags["Name"]
}
```

---

# 🔍 Resource Breakdown

---

## 🔹 Private VPC

Creates isolated network boundary.

```hcl
aws_vpc
```

CIDR:

```text
10.0.0.0/16
```

Supports:

65,536 private IP addresses

---

## 🔹 Private Subnet

Creates internal subnet.

```hcl
map_public_ip_on_launch = false
```

This ensures:

❌ No public IP assignment
✅ Internal-only communication

---

## 🔹 Security Group

Restricts access to VPC-only traffic.

Allowed Source:

```text
10.0.0.0/16
```

This blocks external inbound access.

---

## 🔹 EC2 Instance

Private compute resource inside subnet.

Configuration:

* Amazon Linux AMI
* t2.micro
* Private-only access

---

# ⚙️ Execution Steps

## Step 1 — Navigate to Directory

```bash
cd /home/bob/terraform
```

---

## Step 2 — Initialize Terraform

```bash
terraform init
```

---

## Step 3 — Format Files

```bash
terraform fmt
```

---

## Step 4 — Validate Configuration

```bash
terraform validate
```

Output:

```text
Success! The configuration is valid.
```

---

## Step 5 — Review Execution Plan

```bash
terraform plan
```

Expected:

```text
Plan: 4 to add
```

---

## Step 6 — Apply Infrastructure

```bash
terraform apply -auto-approve
```

---

# 📊 Interactive Execution Logs

## Terraform Plan Output

```bash
Terraform will perform the following actions:

+ aws_vpc.datacenter_vpc
+ aws_subnet.datacenter_subnet
+ aws_security_group.datacenter_sg
+ aws_instance.datacenter_ec2
```

---

## Apply Output

```bash
Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

---

## Final Validation

```bash
terraform plan
```

Output:

```bash
No changes. Your infrastructure matches the configuration.
```

---

# ☁️ AWS Resources Provisioned

Successfully deployed:

✅ Private VPC
✅ Private Subnet
✅ Internal Security Group
✅ Private EC2 Instance
✅ Output Variables

---

# 🧠 Key Terraform Concepts Learned

## Variables

Reusable infrastructure values.

---

## Outputs

Expose created resource details.

---

## Private Networking

Restricts external exposure.

---

## Resource Dependency Management

Terraform automatically provisions resources in correct order.

---

# 🔐 Security Concept Learned

This task implemented **network isolation**.

Private resources:

* Cannot receive public traffic
* Can communicate only internally
* Improve cloud security posture

---

# 🎯 Key Takeaways

✅ Built secure private AWS infrastructure
✅ Used variables for modular design
✅ Configured outputs for visibility
✅ Created internal-only security boundaries
✅ Achieved idempotent Terraform deployment

---
