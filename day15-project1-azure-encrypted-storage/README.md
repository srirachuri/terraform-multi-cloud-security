# Day 15: Project 1 — Azure Encrypted Storage Accounts with Terraform

## Overview

Day 15 is the first Terraform security project in this learning journey. The goal was to apply earlier Terraform concepts to build and manage Azure Storage Accounts with security-focused configuration.

I used variables to control configuration options and `for_each` to manage multiple Storage Account resources. I then formatted, initialized, validated, planned, and applied the Terraform configuration. After deployment, I verified the Storage Account configuration and encryption settings, inspected Terraform outputs, and cleaned up the resources.

This project demonstrates how Terraform can make cloud security configurations reusable, consistent, and easier to maintain.

## Objectives

- Build a Terraform project for Azure Storage security.
- Define reusable input variables.
- Configure Storage Account encryption.
- Use `for_each` to manage multiple Storage Accounts.
- Apply secure transport and public-access settings.
- Format and validate Terraform configuration.
- Review and apply a Terraform execution plan.
- Verify deployed Storage Account settings.
- Inspect Terraform outputs.
- Destroy test infrastructure when finished.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure Storage | Store data |
| Azure CLI | Verify deployed resources |
| PowerShell | Run Terraform and Azure CLI commands |
| HCL | Define infrastructure configuration |

## Project Structure

```
day15-project1-azure-encrypted-storage/
├── main.tf
├── variables.tf
├── providers.tf
├── outputs.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

The exact files may vary depending on the local project configuration.

## Step 1: Define Input Variables

I created `variables.tf` to control configuration options.

The project included variables for controlling resource creation and infrastructure encryption.

Example:

```
variable "create_resources" {
  description = "Whether to create the storage resources"
  type        = bool
  default     = true
}

variable "enable_infrastructure_encryption" {
  description = "Whether to enable infrastructure encryption"
  type        = bool
  default     = true
}
```

Variables help separate configuration choices from resource definitions, making the project easier to reuse.

## Step 2: Configure Multiple Storage Accounts

I used Terraform's `for_each` to manage multiple Storage Accounts through a single resource block.

Example pattern:

```
resource "azurerm_storage_account" "encrypted" {
  for_each = var.create_resources ? var.storage_accounts : {}

  # Storage Account configuration
}
```

This pattern allows Terraform to create one resource instance for each entry in the supplied map when resource creation is enabled.

The resource configuration should define the actual Storage Account properties, including the resource group, location, performance tier, replication type, encryption, and security settings.

## Step 3: Configure Storage Security

The project includes encryption-related configuration and secure Storage Account settings.

Relevant AzureRM Storage Account settings include:

```
https_traffic_only_enabled        = true
min_tls_version                   = "TLS1_2"
allow_nested_items_to_be_public   = false
infrastructure_encryption_enabled = var.enable_infrastructure_encryption
```

These settings enforce HTTPS-only access, require TLS 1.2 or later, prevent anonymous public blob access at the account level, and control infrastructure encryption.

Azure Storage provides encryption at rest by default. Infrastructure encryption adds another layer of encryption for supported scenarios.

## Step 4: Format the Configuration

I ran:

```
terraform fmt
```

This formats Terraform configuration files consistently.

## Step 5: Initialise Terraform

I initialised the working directory:

```
terraform init
```

Terraform installed the required provider and prepared the working directory.

## Step 6: Validate the Configuration

I ran:

```
terraform validate
```

The configuration passed validation.

Validation confirms that Terraform can parse and validate the configuration; it does not independently guarantee that every Azure resource can be deployed successfully.

## Step 7: Review the Terraform Plan

I ran:

```
terraform plan
```

I reviewed the proposed Storage Account changes before applying them.

The plan showed the expected resource actions for the configuration. Reviewing the plan helps identify unexpected changes before they affect Azure infrastructure.

## Step 8: Apply the Configuration

I executed:

```
terraform apply
```

After reviewing the plan, I confirmed the deployment.

Terraform created the configured resources and displayed outputs for the Storage Account names.

## Step 9: Verify Storage Account Settings

I used Azure CLI to inspect the deployed Storage Accounts:

```
az storage account list `
  --resource-group terraform-day15-rg `
  --query "[].{Name:name, HTTPS:httpsTrafficOnly, TLS:minimumTlsVersion, PublicBlobAccess:allowBlobPublicAccess}" `
  --output table
```

The verification covered the Storage Account configuration, including HTTPS-only access, the minimum TLS version, and anonymous public blob access.

I also checked the encryption configuration using Azure CLI and inspected the results in the Azure Portal.

**Note:** Replace the example Resource Group name with the actual name used in your Terraform configuration.

## Step 10: Inspect Terraform Outputs

I ran:

```
terraform output
```

The output displayed the Storage Account names managed by the project, including the primary and secondary instances.

Terraform outputs make important resource information easier to retrieve without manually searching through the full plan.

## Step 11: Destroy the Test Resources

After completing the exercise and verification, I ran:

```
terraform destroy
```

I reviewed the destruction plan and confirmed the operation.

The cleanup screenshots show Terraform destroying the project resources and a subsequent Azure CLI check indicating that the test Resource Group no longer existed.

Destroying test resources helps prevent unnecessary cloud costs.

## Day 15 Results

- [x] Project directory created.
- [x] Input variables defined.
- [x] Multiple Storage Accounts configured with `for_each`.
- [x] Storage security settings configured.
- [x] Terraform formatting completed.
- [x] Terraform initialisation completed.
- [x] Terraform validation passed.
- [x] Terraform plan reviewed.
- [x] Terraform apply completed.
- [x] Storage Account settings verified.
- [x] Terraform outputs inspected.
- [x] Test infrastructure cleanup performed.

## Screenshot Evidence

1. `Screenshot Terraform-Day15-01-Project-Folder.png` — Project directory and Terraform files.
2. `Screenshot Terraform-Day15-02-Variables.png` — Input variables.
3. `Screenshot Terraform-Day15-03-main.tf.png` — Storage Account configuration and `for_each`.
4. `Screenshot Terraform-Day15-04-Storage-Account-Name-Check.png` — Storage Account name checks.
5. `Screenshot Terraform-Day15-05-Terraform-Fmt.png` — Terraform formatting.
6. `Screenshot Terraform-Day15-06-Terraform-Init.png` — Terraform initialisation.
7. `Screenshot Terraform-Day15-07-Terraform-Validate.png` — Successful configuration validation.
8. `Screenshot Terraform-Day15-08-Terraform-Plan.png` — Proposed resource changes.
9. `Screenshot Terraform-Day15-09-Terraform-Apply.png` — Terraform deployment output.
10. `Screenshot Terraform-Day15-10-Azure-Storage-Verification.png` — Storage Account settings verification.
11. `Screenshot Terraform-Day15-11-Azure-Encryption-Verification.png` — Encryption configuration verification.
12. `Screenshot Terraform-Day15-12-Terraform-ForEach-Output.png` — Terraform outputs for the Storage Accounts.
13. `Screenshot Terraform-Day15-13-Terraform-Destroy.png` — Terraform cleanup.
14. `Screenshot Terraform-Day15-14-Azure-Resource-Cleanup.png` — Azure CLI check after cleanup.

## Key Learnings

1. Terraform variables make configurations reusable.
2. `for_each` can manage multiple resource instances from a map or set.
3. Azure Storage Account security settings can be managed declaratively.
4. `terraform validate` checks configuration correctness.
5. `terraform plan` previews proposed infrastructure changes.
6. Terraform outputs provide convenient access to resource information.
7. `terraform destroy` removes resources managed by the configuration and state.
8. Verification and cleanup are important parts of a cloud security project.

## Security Best Practices

- Require HTTPS and a secure minimum TLS version.
- Disable anonymous public blob access unless explicitly required.
- Understand the difference between encryption at rest and infrastructure encryption.
- Apply least-privilege permissions.
- Review Terraform plans before deployment or destruction.
- Protect Terraform state and avoid committing credentials or sensitive values.
- Clean up temporary infrastructure after testing.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 15 of 23  
**Project:** Project 1 — Azure Encrypted Storage  
**Status:** Completed
