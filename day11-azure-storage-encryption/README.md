# Day 11: Azure Storage Account Encryption and TLS Security

## Overview

Day 11 focused on securing an Azure Storage Account using Terraform. I configured storage encryption, enforced HTTPS-only traffic, specified a minimum TLS version, and enabled infrastructure encryption.

I used Terraform to initialise, format, validate, plan, and apply the configuration. After deployment, I verified the Storage Account security settings using Azure CLI and the Azure Portal.

This exercise demonstrates how infrastructure-as-code can be used to implement and verify cloud storage security controls.

## Objectives

- Create an Azure Resource Group and Storage Account using Terraform.
- Configure encryption at rest.
- Enable infrastructure encryption.
- Enforce HTTPS-only traffic.
- Configure a minimum TLS version.
- Validate and deploy the Terraform configuration.
- Verify storage security settings in Azure.
- Understand and correct a duplicate-resource configuration error.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Inspect deployed resources and security settings |
| PowerShell | Execute Terraform and Azure CLI commands |
| HCL | Define infrastructure configuration |

## Project Structure

```
terraform-azure-cloud-security/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

The exact files may vary depending on the configuration used for this exercise.

## Step 1: Configure the Azure Storage Account

I configured the Storage Account in `main.tf` with the following security settings:

```
resource "azurerm_storage_account" "secure" {
  name                     = "your_unique_storage_name"
  resource_group_name      = azurerm_resource_group.day11.name
  location                 = azurerm_resource_group.day11.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  infrastructure_encryption_enabled = true
  min_tls_version                   = "TLS1_2"
  https_traffic_only_enabled        = true

  tags = {
    Environment = "Learning"
    Project     = "Terraform-Day11"
  }
}
```

**Note:** The example uses a placeholder Storage Account name. Azure Storage Account names must be globally unique and follow Azure's naming requirements.

## Step 2: Configure Encryption at Rest

Azure Storage automatically encrypts stored data at rest. The Storage Account configuration can use Microsoft-managed keys by default or customer-managed keys when additional key-management control is required.

I enabled infrastructure encryption:

```
infrastructure_encryption_enabled = true
```

This provides an additional layer of encryption for supported Azure Storage scenarios.

Encryption helps protect stored data, but it does not replace access control, network security, or appropriate key management.

## Step 3: Enforce HTTPS and TLS

I configured the Storage Account to require secure transport:

```
https_traffic_only_enabled = true
min_tls_version           = "TLS1_2"
```

These settings enforce HTTPS-only access and establish TLS 1.2 as the minimum accepted TLS version.

Using HTTPS helps protect data in transit between clients and the Storage Account.

## Step 4: Format and Validate the Configuration

I formatted the Terraform configuration:

```
terraform fmt
```

Then I validated it:

```
terraform validate
```

Terraform returned:

```
Success! The configuration is valid.
```

This confirmed that Terraform could parse and validate the configuration.

## Step 5: Resolve the Duplicate-Resource Error

During the exercise, Terraform reported a duplicate `azurerm_storage_account` resource declaration.

The issue occurred because a Storage Account resource named `secure` had already been declared in the same Terraform module.

I reviewed the configuration and removed the duplicate declaration so that the resource was defined only once.

This reinforced the importance of unique resource addresses within a Terraform module.

## Step 6: Initialise Terraform

I initialised the working directory:

```powershell
terraform init
```

Terraform confirmed successful initialisation and used the AzureRM provider selected by the project's dependency lock file.

## Step 7: Review the Terraform Plan

I ran:

```
terraform plan
```

The plan showed two resources to add and no resources to change or destroy.

I reviewed the planned changes before applying them.

A Terraform plan is useful for identifying unexpected changes before they affect cloud infrastructure.

## Step 8: Apply the Configuration

I deployed the configuration:

```
terraform apply
```

After reviewing the proposed changes, I confirmed the operation.

Terraform reported successful completion with two resources added and no resources changed or destroyed.

The deployment created the Azure Resource Group and Storage Account defined by the configuration.

## Step 9: Verify the Storage Account

I inspected the Storage Account security properties using Azure CLI and verified the configuration in the Azure Portal.

The checks included:

- Storage Account encryption settings.
- Infrastructure encryption configuration.
- HTTPS-only traffic enforcement.
- Minimum TLS version.
- Successfully provisioned Storage Account.

The Azure Portal screenshot provides visual evidence of the Storage Account encryption configuration.

## Day 11 Results

- [x] Azure Storage Account configuration created.
- [x] Encryption at rest reviewed.
- [x] Infrastructure encryption enabled.
- [x] HTTPS-only traffic configured.
- [x] Minimum TLS version set to TLS 1.2.
- [x] Duplicate-resource configuration error identified and corrected.
- [x] Terraform initialisation completed.
- [x] Terraform formatting and validation completed.
- [x] Terraform plan reviewed.
- [x] Terraform apply completed successfully.
- [x] Storage security settings checked in Azure.

## Screenshot Evidence

1. `Screenshot Terraform-Day11-01-Project-Folder.png` — Project directory and existing Terraform files.
2. `Screenshot Terraform-Day11-02-main.tf.png` — Storage Account security configuration.
3. `Screenshot Terraform-Day11-03-Storage-Account-Name-Check.png` — Storage Account name availability check.
4. `Screenshot Terraform-Day11-04-Terraform-Fmt-duplicate-resource.png` — Initial formatting attempt.
5. `Screenshot Terraform-Day11-05-Terraform-duplicate-resource.png` — Duplicate-resource error and troubleshooting.
6. `Screenshot Terraform-Day11-06-Terraform-Validate.png` — Successful Terraform validation.
7. `Screenshot Terraform-Day11-07-Terraform-Init.png` — Successful initialization.
8. `Screenshot Terraform-Day11-08-Terraform-Plan.png` — Planned resource creation.
9. `Screenshot Terraform-Day11-09-Terraform-Apply.png` — Successful deployment.
10. `Screenshot Terraform-Day11-10-Azure-Storage-Encryption.png` — Azure CLI verification of Storage Account settings.
11. `Screenshot Terraform-Day11-11-HTTPS-TLS-Verification.png` — HTTPS and TLS configuration verification.
12. `Screenshot Terraform-Day11-12-Azure-Portal-Encryption.png` — Azure Portal encryption settings.

## Key Learnings

1. Azure Storage encrypts stored data at rest.
2. Infrastructure encryption provides an additional encryption layer for supported configurations.
3. HTTPS-only access helps protect data in transit.
4. TLS 1.2 or later is an important transport-security baseline.
5. Terraform resource names must be unique within a module.
6. `terraform validate` checks configuration correctness, while `terraform plan` previews proposed changes.
7. Azure CLI and the Azure Portal can help verify deployed security settings.

## Security Best Practices

- Enforce HTTPS-only access for storage services.
- Use a secure minimum TLS version.
- Keep storage accounts private unless public access is explicitly required.
- Apply least-privilege access controls.
- Consider customer-managed keys when required by the security or compliance design.
- Never commit credentials, Terraform state files, or sensitive values to a public repository.
- Review Terraform plans before deploying changes.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 11 of 23  
**Status:** Completed
