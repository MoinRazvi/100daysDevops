# 🚀 Day 100 — Terraform EC2 Instance with CloudWatch Alarm

## 🎯 Project Overview

Successfully provisioned AWS infrastructure using Terraform by deploying:

✅ EC2 Instance
✅ CloudWatch Alarm
✅ SNS Notification Integration
✅ CPU Utilization Monitoring

This marks the successful completion of the **Terraform phase** and the entire **100 Days of DevOps Journey**.

---

# 📌 Task Objective

Provision AWS resources with the following configuration:

🔹 **EC2 Instance:** `nautilus-ec2`
🔹 **CloudWatch Alarm:** `nautilus-alarm`
🔹 **SNS Topic:** `nautilus-sns-topic`
🔹 **CPU Threshold:** `90%`
🔹 **Monitoring Period:** `5 Minutes`

---

# 🏗️ Architecture Flow

```text id="4g69w2"
Terraform
   ↓
EC2 Instance
   ↓
CloudWatch Monitoring
   ↓
CPU Threshold Check
   ↓
SNS Notification Trigger
```

---

# 📂 Project Structure

```text id="t6a1po"
terraform/
├── provider.tf
├── main.tf
└── outputs.tf
```

---

# ⚙️ main.tf

```hcl id="0m0mqn"
resource "aws_instance" "nautilus_ec2" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "nautilus-ec2"
  }
}

resource "aws_sns_topic" "sns_topic" {
  name = "nautilus-sns-topic"
}

resource "aws_cloudwatch_metric_alarm" "nautilus_alarm" {
  alarm_name          = "nautilus-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 90

  dimensions = {
    InstanceId = aws_instance.nautilus_ec2.id
  }

  alarm_actions = [
    aws_sns_topic.sns_topic.arn
  ]
}
```

---

# 📤 outputs.tf

```hcl id="7cg9w4"
output "KKE_instance_name" {
  value = aws_instance.nautilus_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.nautilus_alarm.alarm_name
}
```

---

# ▶️ Execution Commands

## 🧹 Format Configuration

```bash id="ij7z7n"
terraform fmt
```

---

## ✅ Validate

```bash id="axns0v"
terraform validate
```

---

## 🚀 Apply Infrastructure

```bash id="sh0gl8"
terraform apply -auto-approve
```

---

## 🔍 Verify Infrastructure

```bash id="tv2e6p"
terraform plan
```

Expected:

```text id="e9h4ry"
No changes. Your infrastructure matches the configuration.
```

---

## 📊 Check Outputs

```bash id="6j3q72"
terraform output
```

Expected:

```text id="08zffw"
KKE_instance_name = "nautilus-ec2"
KKE_alarm_name = "nautilus-alarm"
```

---

# 🐞 Interactive Troubleshooting Logs

## ❌ Error 1: Undeclared Resource Reference

### Error Log

```text id="4rrr7o"
Reference to undeclared resource
```

### Root Cause

Terraform internal resource references cannot use hyphens.

❌ Wrong

```hcl id="6ez5n0"
aws_instance.nautilus-ec2
```

✅ Fixed

```hcl id="nwxpb5"
aws_instance.nautilus_ec2
```

---

## ❌ Error 2: Pending Changes After Apply

### Error Log

```text id="0d60ei"
Terraform plan is still showing pending changes
```

### Resolution

Destroyed stale resources:

```bash id="6ykq9q"
terraform destroy -auto-approve
```

Recreated infrastructure:

```bash id="w6t4bx"
terraform apply -auto-approve
```

---

## ❌ Error 3: SNS Topic Reference Conflict

### Problem

Confusion between:

🔸 Terraform resource
🔸 Existing AWS data source

### Final Fix

```hcl id="8r9d54"
aws_sns_topic.sns_topic.arn
```

---

# 📚 Key Learnings

✨ Terraform resource naming conventions
✨ CloudWatch alarm automation
✨ SNS integration
✨ Terraform state consistency
✨ Infrastructure validation workflow
✨ AWS monitoring best practices

---

# 🏆 Final Validation

Successfully provisioned:

✅ EC2 Instance
✅ CloudWatch Alarm
✅ SNS Notification Trigger
✅ Terraform Validation Passed

---

# 🎉 Milestone Achieved

## 💯 Day 100 Completed

