# 🚀 Terraform AWS Jenkins Deployment

This Terraform project automates the creation of AWS infrastructure to run a Jenkins CI/CD server, along with Docker, SonarQube, and Trivy installed on an EC2 instance. It provisions networking, security, compute resources, and configures the server automatically using a user data script.

---

## 📁 Project Files Overview

| File            | Purpose                                                  |
|-----------------|----------------------------------------------------------|
| `main.tf`       | Defines AWS resources like VPC, Subnet, Security Group, EC2 instance, Key Pair, etc. |
| `provider.tf`   | Configures Terraform AWS provider and target region       |
| `output.tf`     | Defines outputs to display after infrastructure creation  |
| `script.sh`     | Bash script run on EC2 instance to install Jenkins, Docker, Trivy, SonarQube, etc. |
| `terraform-key.pub` | Your SSH public key for EC2 access (must be provided)  |

---

## 🛠️ Detailed Explanation of Files

---

## 🔧 Prerequisites

- AWS account with credentials (`aws configure`)
- Terraform installed (v1.3+ recommended)
- A valid public key file named `terraform-key.pub` in the project root

---

## 🌐 Infrastructure Overview

### 1️⃣ Custom VPC and Networking

Creates a VPC (`10.0.0.0/16`) with:

- Public Subnet (`10.0.1.0/24`)
- Internet Gateway
- Route Table for public internet access

```hcl
resource "aws_vpc" "custom_vpc" { ... }
resource "aws_internet_gateway" "gw" { ... }
resource "aws_subnet" "public_subnet" { ... }
resource "aws_route_table" "public_rt" { ... }
resource "aws_route_table_association" "a" { ... }


Security Group (Allow All Traffic)
Creates an open security group for demo/testing:


resource "aws_security_group" "allow_all" { ... }



Key Pair
Imports your local SSH public key into AWS to allow login access.


resource "aws_key_pair" "deployer_key" {
  key_name   = "terraform-key"
  public_key = file("terraform-key.pub")
}


EC2 Instance Setup
Creates a t3.large EC2 instance with:

Jenkins (via script.sh)

Docker (added to jenkins user)

Trivy (security scanner)

SonarQube (running in Docker)


resource "aws_instance" "web" {
  ami                         = "ami-042b4708b1d05f512"
  instance_type               = "t3.large"
  subnet_id                   = aws_subnet.public_subnet.id
  vpc_security_group_ids      = [aws_security_group.allow_all.id]
  key_name                    = aws_key_pair.deployer_key.key_name
  associate_public_ip_address = true
  user_data                   = file("script.sh")

  root_block_device {
    volume_size = 20
  }

  tags = {
    Name = "jenkins-ec2"
  }
}


User Data Script (script.sh)
This script runs on EC2 launch and performs:


