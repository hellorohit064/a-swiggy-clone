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

### `main.tf` — AWS Infrastructure

This is the core of your infrastructure as code. It contains several resource blocks:

#### 1. Custom VPC and Networking

```hcl
resource "aws_vpc" "custom_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = { Name = "custom-vpc" }
}

Creates a Virtual Private Cloud (VPC) — a logically isolated AWS network — with a private IP range of 10.0.0.0/16.


Creates a Virtual Private Cloud (VPC) — a logically isolated AWS network — with a private IP range of 10.0.0.0/16.
