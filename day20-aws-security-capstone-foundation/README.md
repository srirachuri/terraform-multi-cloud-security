# Day 20: AWS Security Capstone — Terraform Networking Foundation

## Overview

Day 20 marks the transition from Azure-focused Terraform exercises to an AWS cloud security capstone.

In this project, I used Terraform to define an AWS Virtual Private Cloud (VPC), public and private subnets, an Internet Gateway, a public route table, and an EC2 security group.

The goal was to establish a network and security foundation for the next stages of the capstone, which will introduce IAM roles, EC2 configuration, S3 security controls, and Checkov security scanning.

## Objectives

- Configure the AWS provider for Terraform.
- Define an AWS VPC and CIDR address space.
- Create public and private subnets.
- Configure an Internet Gateway.
- Create a public route table and associate it with the public subnet.
- Configure an EC2 security group.
- Restrict SSH access to a trusted IP range.
- Define Terraform outputs for important resource identifiers.
- Format and validate the configuration.
- Review the Terraform execution plan before deployment.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as code |
| Amazon Web Services (AWS) | Cloud platform |
| AWS Provider | Manage AWS resources |
| Amazon VPC | Network isolation |
| EC2 Security Groups | Control instance network traffic |
| HCL | Terraform configuration language |
| PowerShell | Execute Terraform commands |

## Project Structure

```
day20-aws-security-capstone-foundation/
├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
├── terraform.tfvars
└── README.md
```

### File Descriptions

- `versions.tf` — Specifies the required Terraform version, AWS provider version constraint, and AWS region configuration.
- `variables.tf` — Declares input variables for the AWS region, VPC CIDR, subnet CIDRs, and trusted SSH CIDR.
- `main.tf` — Defines the VPC, subnets, Internet Gateway, public route table, and EC2 security group.
- `outputs.tf` — Exposes the VPC, subnet, Internet Gateway, route table, and security group IDs.
- `terraform.tfvars` — Supplies environment-specific input values. Use a sanitised example file for public GitHub documentation rather than committing personal or sensitive values.

## Step 1: Configure the AWS Provider

I configured the AWS provider in `versions.tf`.

```
terraform {
  required_version = ">= 1.6.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

The provider configuration specifies the AWS region through an input variable and constrains the AWS provider to compatible 6.x versions.

## Step 2: Define Input Variables

I declared variables for the region and network configuration in `variables.tf`.

```
variable "aws_region" {
  description = "AWS region for the capstone"
  type        = string
}

variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  type        = string
}

variable "public_subnet_cidr" {
  description = "CIDR block for the public subnet"
  type        = string
}

variable "private_subnet_cidr" {
  description = "CIDR block for the private subnet"
  type        = string
}

variable "ssh_allowed_cidr" {
  description = "CIDR allowed to access SSH"
  type        = string
}
```

Variables make the network configuration easier to change without editing resource definitions directly.

## Step 3: Create the VPC and Subnets

I defined a VPC and two subnets using Terraform.

| Network component | CIDR |
|---|---|
| VPC | `10.0.0.0/16` |
| Public subnet | `10.0.1.0/24` |
| Private subnet | `10.0.2.0/24` |

The VPC provides an isolated network environment. The public and private subnets separate workloads according to their intended connectivity requirements.

The public subnet is configured to assign public IPv4 addresses to launched instances by default. The private subnet does not have that setting enabled.

## Step 4: Configure the Internet Gateway and Public Route Table

I created an Internet Gateway and attached it to the VPC.

I then configured a public route table with a default route:

```
route {
  cidr_block = "0.0.0.0/0"
  gateway_id = aws_internet_gateway.main.id
}
```

The public route table was associated with the public subnet.

This provides a route toward the Internet Gateway for eligible traffic from the public subnet. A working internet connection also depends on the instance's addressing, security rules, and other network requirements.

The private subnet was not associated with this public route table.

## Step 5: Configure the EC2 Security Group

I defined a security group for the capstone's EC2 workload.

The configured inbound rules allow:

| Port | Protocol | Source | Purpose |
|---|---|---|---|
| 22 | TCP | Trusted CIDR variable | SSH administration |
| 80 | TCP | `0.0.0.0/0` | HTTP traffic |
| 443 | TCP | `0.0.0.0/0` | HTTPS traffic |

SSH access is restricted using the `ssh_allowed_cidr` variable. HTTP and HTTPS are allowed from the internet to support a potential public web workload.

The configuration also allows outbound traffic to any destination. This is broad egress access and may be narrowed in a more restrictive production design.

## Step 6: Define Terraform Outputs

I configured outputs to expose important resource identifiers:

- VPC ID
- Public subnet ID
- Private subnet ID
- Internet Gateway ID
- Public route table ID
- EC2 security group ID

These outputs make it easier to inspect resource identifiers after deployment and reference them in later stages of the capstone.

## Step 7: Initialise and Validate Terraform

I initialised the working directory and validated the configuration.

```
terraform init
terraform fmt
terraform validate
```

Terraform successfully initialised the AWS provider, and the validation output confirmed that the configuration was valid.

## Step 8: Review the Execution Plan

I ran:

```
terraform plan
```

The plan showed:

```
Plan: 7 to add, 0 to change, 0 to destroy.
```

The seven planned resources were:

1. VPC
2. Public subnet
3. Private subnet
4. Internet Gateway
5. Public route table
6. Public route table association
7. EC2 security group

The plan was reviewed before proceeding. No infrastructure deployment was performed as part of this Day 1 evidence.

## Day 20 Results

- [x] AWS provider configuration created.
- [x] Input variables declared.
- [x] VPC configuration defined.
- [x] Public and private subnets configured.
- [x] Internet Gateway configured.
- [x] Public route table and subnet association configured.
- [x] EC2 security group defined.
- [x] Terraform outputs configured.
- [x] Terraform initialised successfully.
- [x] Terraform configuration formatted and validated.
- [x] Execution plan reviewed.
- [x] Seven resources identified in the plan.

## Screenshot Evidence

1. `Screenshot Capstone-Day1-01-Project-Structure.png` — Terraform project files.
2. `Screenshot Capstone-Day1-02-AWS-Provider-Init.png` — AWS provider configuration and successful initialisation.
3. `Screenshot Capstone-Day1-03-Terraform-Validate.png` — Successful Terraform validation.
4. `Screenshot Capstone-Day1-04-Terraform-Plan.png` — Execution plan showing seven resources to add.

## Key Learnings

1. Terraform can manage AWS infrastructure using declarative configuration.
2. VPCs and subnets provide network isolation and segmentation.
3. Internet Gateways and route tables determine how eligible traffic is routed.
4. Security groups control network traffic associated with AWS resources.
5. Restricting SSH to a trusted CIDR is safer than allowing access from every IP address.
6. Terraform outputs provide convenient access to resource identifiers.
7. Reviewing a plan before deployment helps identify unexpected changes.

## Security and Best Practices

- Restrict SSH access to trusted IP ranges.
- Allow only the network ports required by the workload.
- Review outbound rules and reduce broad egress access where appropriate.
- Keep the private subnet separate from the public route table.
- Never commit AWS credentials, Terraform state files, or sensitive variable files to GitHub.
- Use a sanitized `terraform.tfvars.example` for public documentation.
- Review the Terraform plan before applying infrastructure changes.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 20 of 23  
**Project:** AWS Security Capstone — Networking Foundation  
**Status:** Completed — configuration, validation, and planning evidence captured.
