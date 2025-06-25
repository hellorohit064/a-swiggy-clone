# 🚀 Terraform Project: Jenkins on AWS

This project provisions a basic AWS infrastructure using Terraform to host a Jenkins CI/CD server on an EC2 instance. It also installs Docker, Trivy (for security scanning), and runs a SonarQube container for code quality analysis.

---

## 📁 Project Structure

├── main.tf # Core infrastructure (VPC, EC2, etc.)
├── provider.tf # AWS provider configuration
├── output.tf # Outputs (e.g., IP addresses)
├── terraform-key.pub # SSH public key (you must provide this)
├── script.sh # User data script to install Jenkins, Docker, Trivy, SonarQube


---

## 🔧 What Each File Does

### `main.tf` — **Main Infrastructure Setup**

This file creates all the AWS resources needed for running Jenkins:

1. **Custom VPC & Subnet**:
   - Creates a new VPC (`10.0.0.0/16`) and a public subnet (`10.0.1.0/24`).
   - Adds an internet gateway and associates a route table to allow outbound internet access.

2. **Security Group**:
   - Opens **all traffic** (insecure for production) for quick access.
   - Allows both inbound and outbound traffic (`0.0.0.0/0`).

3. **Key Pair**:
   - Adds a public SSH key (`terraform-key.pub`) so you can SSH into the EC2 instance.

4. **EC2 Instance**:
   - Launches a `t3.large` EC2 instance using Ubuntu AMI (`ami-042b4708b1d05f512`).
   - Associates the EC2 with the public subnet and security group.
   - Installs Jenkins, Docker, Trivy, and SonarQube via `script.sh`.

---

### `provider.tf` — **Provider Setup**

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "eu-north-1"
}

Sets the required AWS provider and specifies the region (eu-north-1).

Terraform will use this provider to interact with AWS APIs.


### `outsider.tf` — **Provider Setup**
output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.web.public_ip
}

output "instance_private_ip" {
  description = "Private IP address of the EC2 instance"
  value       = aws_instance.web.private_ip
}


After applying the configuration, Terraform will display:

The public IP to access Jenkins.

The private IP for internal use.


script.sh — Jenkins + DevOps Tools Installation
This is the user data script that automatically runs when the EC2 instance boots.

Here's what it does:

System Update
Updates all system packages.

Install Java (OpenJDK 17)
Required to run Jenkins.

Install Jenkins

Adds the Jenkins repository.

Installs Jenkins and enables the service.

Install Docker

Sets up the official Docker repository.

Installs Docker Engine and adds the jenkins and current user to the Docker group.

Runs SonarQube in a Docker container on port 9000.

Install Trivy

Adds the Trivy repository (by Aqua Security).

Installs Trivy CLI tool for image and repository vulnerability scanning.

Display Admin Password
Prints the initial Jenkins admin password and reminds the user to reboot/log out for Docker group changes to apply.

Prerequisites
Terraform installed

AWS CLI configured

SSH key pair (terraform-key.pub)

# Initialize Terraform
terraform init

# Plan the infrastructure
terraform plan

# Apply the changes
terraform apply



To remove all resources:

terraform destroy


📜 License
This project is open-source and available for educational or personal use.


---

Let me know if you’d like a version with Terraform diagrams (e.g., architecture image), security best practices, or if you're deploying via CI/CD (like GitHub Actions).

