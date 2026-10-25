# Day 13: Azure Storage Public Access Security

## Overview

Day 13 focused on preventing unintended public access to Azure Storage data using Terraform.

I configured an Azure Storage Account with anonymous blob access disabled and deployed the configuration using Terraform. I then verified the storage account settings through Azure CLI and the Azure Portal.

This exercise demonstrates how infrastructure-as-code can help enforce cloud storage security controls and reduce the risk of accidental data exposure.

## Objectives

- Create an Azure Resource Group and Storage Account using Terraform.
- Disable anonymous public access to blobs.
- Configure secure storage transport settings.
- Format and validate Terraform configuration.
- Review the Terraform execution plan.
- Deploy the configuration.
- Verify public access settings using Azure CLI.
- Confirm the settings in the Azure Portal.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify storage configuration |
| Azure Portal | Inspect storage security settings |
| PowerShell | Execute Terraform and Azure CLI commands |
| HCL | Define infrastructure configuration |

## Project Structure

```
day13-azure-storage-public-access/
├── main.tf
├── providers.tf
├── .terraform.lock.hcl
├── .terraform/
├── README.md
└── screenshots/
```

The exact project structure may vary depending on the files used in the local configuration.

## Step 1: Configure the Azure Resource Group

I configured an Azure Resource Group to contain the Storage Account:

```
resource "azurerm_resource_group" "day13" {
  name     = "terraform-day13-rg"
  location = "eastus"
}
```

The Resource Group provides a logical container for the infrastructure created during this exercise.

## Step 2: Configure the Storage Account

I configured the Storage Account with public blob access disabled and secure transport settings.

The relevant Terraform settings were:

```
resource "azurerm_storage_account" "private" {
  name                     = "your_unique_storage_name"
  resource_group_name      = azurerm_resource_group.day13.name
  location                 = azurerm_resource_group.day13.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  allow_nested_items_to_be_public = false
  min_tls_version                 = "TLS1_2"
  https_traffic_only_enabled      = true
}
```

**Note:** The Storage Account name is a placeholder. Use a valid, globally unique name when adapting this example to your own environment.

## Step 3: Disable Anonymous Public Blob Access

The key security setting is:

```
allow_nested_items_to_be_public = false
```

This prevents containers and blobs from being configured to allow anonymous public access through the Storage Account's public-access setting.

Disabling anonymous access helps reduce the risk of exposing data unintentionally.

This setting does not, by itself, make the entire Storage Account private or block every network path. Network restrictions, identity-based access controls, and other security settings should be configured according to the intended design.

## Step 4: Format and Initialise Terraform

I formatted the configuration:

```
terraform fmt
```

Then I initialised the working directory:

```
terraform init
```

Terraform successfully initialised the project and installed the required AzureRM provider.

## Step 5: Validate the Configuration

I ran:

```
terraform validate
```

Validation checks whether Terraform can parse and validate the configuration. It does not independently prove that every Azure security setting has been deployed correctly.

## Step 6: Review the Terraform Plan

I ran:

```
terraform plan
```

The plan showed:

```
Plan: 2 to add, 0 to change, 0 to destroy.
```

I reviewed the planned changes before proceeding with deployment.

Reviewing a plan helps identify unexpected resource changes before they are applied.

## Step 7: Apply the Configuration

I deployed the configuration:

```
terraform apply
```

After reviewing the proposed changes, I confirmed the operation.

Terraform completed successfully and reported that two resources had been added, with no changes or destructions.

## Step 8: Verify the Storage Account Using Azure CLI

I checked the Storage Account configuration:

```
az storage account show `
  --name YOUR_STORAGE_ACCOUNT_NAME `
  --resource-group terraform-day13-rg `
  --query "{name:name,allowBlobPublicAccess:allowBlobPublicAccess}" `
  --output json
```

The result showed that `allowBlobPublicAccess` was `false`.

This provided command-line evidence that anonymous public blob access was disabled at the Storage Account level.

Replace `YOUR_STORAGE_ACCOUNT_NAME` with the actual name of your deployed Storage Account.

## Step 9: Verify the Azure Portal Settings

I opened the Storage Account configuration in the Azure Portal and inspected its configuration settings.

The screenshot showed anonymous blob access set to **Disabled**.

This provided additional visual evidence that the Storage Account was configured to prevent anonymous public blob access.

## Day 13 Results

- [x] Azure Resource Group configured.
- [x] Storage Account configured using Terraform.
- [x] Anonymous public blob access disabled.
- [x] HTTPS-only access configured.
- [x] Minimum TLS version set to TLS 1.2.
- [x] Terraform formatting completed.
- [x] Terraform initialisation completed.
- [x] Terraform plan reviewed.
- [x] Terraform apply completed successfully.
- [x] Storage Account settings verified using Azure CLI.
- [x] Azure Portal configuration inspected.

## Screenshot Evidence

1. `Screenshot Terraform-Day13-01-Project-Folder.png` — Day 13 project directory.
2. `Screenshot Terraform-Day13-02-main.tf.png` — Terraform Storage Account configuration.
3. `Screenshot Terraform-Day13-03-Storage-Account-Name-Check.png` — Storage Account name check.
4. `Screenshot Terraform-Day13-04-Terraform-Fmt.png` — Terraform formatting.
5. `Screenshot Terraform-Day13-05-Terraform-Init.png` — Successful Terraform initialization.
6. `Screenshot Terraform-Day13-06-Terraform-Validate.png` — Terraform validation.
7. `Screenshot Terraform-Day13-07-Terraform-Plan.png` — Planned infrastructure changes.
8. `Screenshot Terraform-Day13-08-Terraform-Apply.png` — Successful deployment.
9. `Screenshot Terraform-Day13-09-Azure-Public-Access-Verification.png` — Azure CLI verification showing public blob access disabled.
10. `Screenshot Terraform-Day13-10-Azure-Portal-Public-Access-Disabled.png` — Azure Portal evidence of the public-access setting.

## Key Learnings

1. Azure Storage Account settings can prevent anonymous public blob access.
2. Terraform makes security configurations repeatable and reviewable.
3. Azure CLI can be used to verify deployed configuration values.
4. Azure Portal inspection provides a visual confirmation of security settings.
5. Disabling anonymous blob access is different from restricting all network access.
6. Identity-based permissions and network security remain important parts of storage protection.

## Security Best Practices

- Disable anonymous public blob access unless a documented requirement exists.
- Apply least-privilege access controls.
- Require HTTPS and use a secure minimum TLS version.
- Consider network restrictions for sensitive storage workloads.
- Monitor access and configuration changes.
- Protect Terraform state and never commit credentials or sensitive values to GitHub.
- Verify deployed settings rather than relying only on Terraform configuration.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 13 of 23  
**Status:** Completed
