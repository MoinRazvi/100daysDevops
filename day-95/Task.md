### Use Terraform to create a security group under the default VPC with the following requirements:
#
  1) The name of the security group must be nautilus-sg.<br>
  2) The description must be Security group for Nautilus App Servers.<br>
  3) Add an inbound rule of type HTTP, with a port range of 80, and source CIDR range 0.0.0.0/0.<br>
  4) Add another inbound rule of type SSH, with a port range of 22, and source CIDR range 0.0.0.0/0.<br>
<br>
Ensure that the security group is created in the us-east-1 region using Terraform. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.<br>
<br>
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.
