# 🚀 Terraform AWS VPC & EC2 Setup

This project provisions the following AWS infrastructure using [Terraform](https://www.terraform.io/):

- A **Custom VPC**
- A **Public Subnet** with an Internet Gateway
- A **Security Group** that allows all inbound/outbound traffic (⚠️ Not recommended for production)
- An optional **SSH Key Pair** (`terraform-key`)
- An **EC2 Instance** (e.g., for Jenkins)

---

## 📁 Project Structure

```bash
.
├── main.tf             # Main Terraform configuration file
├── terraform-key.pub   # Public SSH key (generated manually)
├── script.sh           # User-data script (e.g., install Jenkins)
└── README.md           # Project documentation
