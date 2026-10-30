# Multi-Cloud Security with Terraform — Azure & AWS

## Overview

This repository documents my hands-on learning journey in **multi-cloud infrastructure and cloud security using Terraform**. It covers Azure infrastructure, identity and access management, network security, storage security, AWS networking, IAM, EC2, S3 security, and infrastructure-as-code security scanning.

The project is organised into daily learning stages, with Terraform configuration files, documentation, screenshots, validation results, and security-hardening exercises.

**Primary goal:** Build practical skills for a future career in cloud security engineering.

## Technologies and Tools

- **Infrastructure as Code:** Terraform
- **Cloud platforms:** Microsoft Azure and Amazon Web Services (AWS)
- **Identity and access management:** Azure RBAC, Microsoft Entra ID concepts, AWS IAM roles and policies
- **Networking:** Azure Virtual Networks, Network Security Groups, AWS VPC, subnets, route tables, Internet Gateway, security groups
- **Storage security:** Azure Storage, encryption, blob versioning, AWS S3 encryption, versioning and public-access blocking
- **Compute:** AWS EC2 and Linux fundamentals
- **Monitoring and logging:** Azure Monitor concepts, Amazon CloudWatch and VPC Flow Logs
- **Security scanning:** Checkov
- **Operating systems and scripting:** Linux and Bash

## Repository Structure

```
terraform-multi-cloud-security/
├── README.md
├── .gitignore
├── azure/
│   ├── day01-terraform-setup-azure-authentication/
│   ├── day02-terraform-variables-resource-group/
│   ├── day03-terraform-variables-tfvars/
│   ├── ...
│   └── day19-azure-vnet-routing/
├── aws/
│   ├── day20-aws-security-capstone-foundation/
│   ├── day21-aws-security-capstone-iam-ec2-s3/
│   └── day22-aws-security-capstone-checkov-hardening/

```

*The tree above is the intended organisation. Replace the illustrative `...` with your actual folders and include `docs/` only if you create it.*

## Azure Projects — Days 1–19

The Azure exercises cover Terraform fundamentals and progressively introduce cloud security concepts.

| Day | Project | Main focus |
|---|---|---|
| 1 | Terraform setup and Azure authentication | Provider setup and authentication |
| 2 | Variables and resource groups | Reusable configuration |
| 3 | Variable files | External configuration with `.tfvars` |
| 4 | Terraform outputs | Exporting resource information |
| 5 | Data sources | Referencing existing infrastructure |
| 6 | Terraform initialization | Provider and module initialization |
| 7 | Terraform plan | Reviewing proposed infrastructure changes |
| 8 | Terraform apply | Applying configuration |
| 9 | Terraform destroy | Resource lifecycle management |
| 10 | Validation and sensitive variables | Configuration checks and sensitive inputs |
| 11 | Azure Storage encryption | Storage protection |
| 12 | Blob versioning and recovery | Data recovery concepts |
| 13 | Storage public access | Storage exposure and access controls |
| 14 | Minimal RBAC | Identity permissions and least privilege |
| 15 | Encrypted Azure Storage project | Combining storage security controls |
| 16 | Terraform modules and Checkov | Reusable configuration and security scanning |
| 17 | Azure IAM and remote state | Access management and state-management concepts |
| 18 | Azure Network Security Group | Network traffic filtering |
| 19 | Azure VNet and routing | Network segmentation and routing |

Each daily folder should contain its own `README.md` and the relevant Terraform files and evidence.

## AWS Security Capstone — Days 20–22

### Day 20 — Networking foundation

Builds the foundation for an AWS security lab using Terraform.

Key areas:

- VPC and public/private subnets
- Internet Gateway
- Public route table and association
- Security group rules for SSH, HTTP and HTTPS
- Terraform initialisation, validation and plan review

The saved evidence includes a successful validation and a plan reporting seven resources to add at that stage.

### Day 21 — IAM, EC2 and S3 security

Extends the AWS capstone with compute, identity and storage controls.

Key areas:

- IAM role and EC2 instance profile
- Scoped S3 read permissions
- Amazon Linux EC2 instance
- Encrypted root EBS volume
- S3 server-side encryption and versioning
- S3 public-access blocking
- Terraform apply and subsequent plan checks

### Day 22 — Checkov scanning and security hardening

Focuses on infrastructure-as-code security analysis and additional monitoring controls.

Key areas:

- Checkov scans and findings review
- EC2 hardening and metadata-service configuration
- Default security group restrictions
- S3 lifecycle configuration and access logging
- VPC Flow Logs
- CloudWatch log retention and KMS encryption
- Terraform plan review and remediation documentation

**Security-scanning note:** Checkov findings should be reviewed individually. This repository will document the actual scan results and any remaining findings rather than claiming that all checks pass without supporting evidence.

## Security Practices

The projects explore the following practices:

- Apply least-privilege principles to cloud permissions.
- Restrict administrative access to trusted sources.
- Encrypt storage and logs where configured.
- Block unintended public access to storage.
- Use versioning and lifecycle policies where appropriate.
- Enable logging and monitoring for investigation and troubleshooting.
- Review Terraform plans before applying changes.
- Use Checkov to identify infrastructure-as-code security risks.
- Keep credentials, state files and saved plans out of source control.

These controls are documented according to the configuration and evidence available for each project. Not every control has necessarily been independently tested at runtime.

## Validation and Testing

Depending on the exercise, the repository includes evidence from commands such as:

```
terraform fmt
terraform init
terraform validate
terraform plan
checkov -d . --framework terraform
```

These commands support formatting, initialisation, configuration validation, change review and security scanning.

Run them from the relevant project directory. Review the plan and any Checkov findings before making changes to deployed infrastructure.

## Repository Safety

Before publishing, confirm that the repository excludes:

- AWS access keys, passwords and other credentials
- Real `terraform.tfvars` files containing sensitive values
- Terraform state and backup files
- `.terraform/` working directories
- Saved plan files such as `*.tfplan`
- Sensitive information in screenshots

Use a sanitised `terraform.tfvars.example` where an example configuration is helpful. Never commit real cloud credentials.

## Learning Outcomes

Through these exercises, I am developing practical experience with:

- Terraform configuration and resource lifecycle management
- Multi-cloud infrastructure fundamentals
- Cloud identity and access management
- Network segmentation and traffic restrictions
- Storage encryption and access controls
- Infrastructure-as-code security scanning
- Logging, monitoring and troubleshooting
- Technical documentation and security evidence

## Future Improvements

- Add architecture diagrams for the Azure and AWS projects.
- Document Checkov findings, severity, remediation and any justified residual risks.
- Verify deployed security settings directly in the cloud consoles or through appropriate CLI/API checks.
- Add automated formatting, validation and security scanning to a CI/CD workflow.
- Continue building practical cloud-security and automation projects.

## About This Portfolio

This repository represents hands-on learning and project work, not a claim of professional production experience. It is intended to demonstrate my technical development, practical problem-solving, and interest in cloud security engineering.

