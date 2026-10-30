# Day 21: AWS Security Capstone — IAM, EC2, and S3 Security

## Overview

This project extends the Day 20 AWS networking foundation using Terraform. It adds an EC2 instance, an IAM role with limited S3 read permissions, and an S3 bucket configured with encryption, versioning, and public-access protections.

The goal is to practise deploying AWS infrastructure as code while applying foundational cloud-security controls.

## Objectives

- Extend the existing VPC and subnet configuration.
- Create an IAM role that EC2 can assume.
- Attach an instance profile to the EC2 instance.
- Grant the EC2 role read-only access to the project’s S3 bucket.
- Deploy an Amazon Linux 2023 EC2 instance.
- Encrypt the EC2 root EBS volume.
- Configure S3 encryption, versioning, public-access blocking, and bucket ownership controls.
- Validate the Terraform configuration and review the execution plan.
- Apply the configuration and verify the resulting Terraform outputs.

## Architecture

The project includes:

- **VPC:** Existing networking foundation from Day 20.
- **Public subnet:** Hosts the EC2 instance and is configured to assign public IPv4 addresses on launch.
- **Private subnet:** Created as part of the network foundation; this configuration does not associate it with a private route table.
- **Internet Gateway and public route table:** Provide the public subnet's internet route.
- **EC2 security group:** Allows SSH from a configured trusted CIDR and allows inbound HTTP and HTTPS from the internet.
- **EC2 instance:** Uses an Amazon Linux 2023 AMI and the `t3.micro` instance type.
- **IAM role and instance profile:** Allow EC2 to assume the role.
- **S3 bucket:** Configured with server-side encryption, versioning, public-access blocking, and bucket-owner-enforced object ownership.

## Security Controls

### 1. IAM least privilege

An IAM role is created for EC2, with an instance profile to attach it to the instance.

The inline policy permits:

- `s3:ListBucket` on the project S3 bucket.
- `s3:GetObject` on objects within that bucket.

This limits the policy's S3 permissions to listing and reading the designated bucket rather than granting general S3 access.

### 2. EC2 security

- Uses an Amazon Linux 2023 AMI selected through an AWS AMI data source.
- Uses a `t3.micro` instance.
- Associates the instance with the EC2 security group and IAM instance profile.
- Configures a 30 GB `gp3` root EBS volume with encryption enabled.
- Deletes the root volume when the instance is terminated.

**Security note:** The supplied configuration does not explicitly set EC2 metadata options to require IMDSv2. Add and verify that control separately before describing IMDSv2 enforcement as completed.

### 3. S3 security

The S3 bucket has the following controls:

- **Server-side encryption:** AES-256.
- **Versioning:** Enabled.
- **Public-access block:** All four public-access-block settings are enabled.
- **Object ownership:** `BucketOwnerEnforced`.

These settings help protect stored data and reduce the risk of accidental public exposure.

### 4. Network access

The EC2 security group permits:

- SSH (TCP 22) from the configured trusted CIDR.
- HTTP (TCP 80) from `0.0.0.0/0`.
- HTTPS (TCP 443) from `0.0.0.0/0`.
- Outbound traffic to `0.0.0.0/0`.

Keep SSH restricted to a trusted IP address or CIDR. Opening ports 80 and 443 allows inbound network traffic, but does not by itself prove that a web server or HTTPS service is installed and working.

## Terraform Validation and Deployment

The screenshots supplied for this day show the following workflow:

1. Ran `terraform fmt`.
2. Ran `terraform validate` and received `Success! The configuration is valid.`
3. Reviewed Terraform plans while refining the configuration.
4. Applied the configuration; the terminal screenshot shows `Apply complete!`.
5. Ran `terraform plan` again; the supplied no-changes screenshot shows Terraform refreshing the deployed resources.

The final deployment screenshot includes outputs for the VPC, subnets, Internet Gateway, public route table, security group, EC2 instance ID, public IP, IAM role name, and S3 bucket name.

A no-changes plan indicates that Terraform did not identify further changes in that plan. It is not, by itself, a complete security audit of the deployed resources.

## Files

The uploaded Day 21 ZIP includes:

- `main.tf`
- `outputs.tf`

The working project also depends on the provider and variable definitions from the existing Terraform setup. Include the required configuration files when preparing the complete project folder for GitHub.

## Screenshot Evidence

The supplied archive contains these screenshots:

1. `Screenshot Capstone-Day2-01-Terraform-Validate.png`
2. `Screenshot Capstone-Day2-02-CloudSecurity-Capstone-Deploy-Policy-added.png`
3. `Screenshot Capstone-Day2-03-CloudSecurity-Capstone-Deploy-Policy-Updated.png`
4. `Screenshot Capstone-Day2-04-Terraform-Plan.png`
5. `Screenshot Capstone-Day2-05-destroyed.png`
6. `Screenshot Capstone-Day2-06-Terraform-Plan.png`
7. `Screenshot Capstone-Day2-06-Terraform-Saved-Plan`
8. `Capstone-Day2-07-Terraform-Plan-No-Changes.png`
9. `Screenshot Capstone-Day2-08-Terraform-Plan-Review`
10. `Screenshot Capstone-Day2-09-Terraform-Plan-Clean`
11. `Screenshot Capstone-Day2-10-Terraform-apply`

**Evidence clarification:** The file named `Screenshot Capstone-Day2-05-destroyed.png` shows Terraform state backup and resource untaint commands, not a confirmed destruction of the infrastructure. The README therefore does not treat it as proof of a successful destroy operation.

## Important GitHub Security Practices

Before publishing this project:

- Do not commit AWS access keys, credentials, Terraform state files, or saved plan files.
- Keep local `terraform.tfvars` out of version control if it contains personal IP addresses or other private values.
- Provide a sanitised `terraform.tfvars.example` containing placeholders rather than real personal values.
- Review screenshots and redact public IP addresses, AWS account identifiers, and other details you do not want publicly exposed.
- Avoid uploading `day2.tfplan` or state backups.
- Review the deployed resources in the AWS console before considering the environment production-ready.

## Learning Outcomes

By completing this exercise, I practised:

- Deploying AWS infrastructure using Terraform.
- Connecting EC2 to an IAM role through an instance profile.
- Applying resource-scoped S3 permissions.
- Configuring S3 encryption, versioning, and public-access protections.
- Enabling encryption for an EC2 root EBS volume.
- Reviewing Terraform plans, applying infrastructure changes, and checking for further drift or changes.

## Status

**Completed with deployment evidence.** The supplied screenshots show successful Terraform validation, an apply operation, and a subsequent plan with no changes. Additional review is still needed for controls not explicitly configured in the supplied code, including requiring IMDSv2.
