# Day 8: Terraform Apply

## Overview

Day 8 focused on understanding the `terraform apply` command and how Terraform applies a configuration to manage cloud infrastructure.

I applied the existing Terraform configuration in Microsoft Azure. Terraform compared the configuration with the current infrastructure, reported that no changes were required, and completed successfully. I then reviewed the Terraform outputs and independently verified the Azure Resource Group using Azure CLI.

This exercise demonstrates how to apply a Terraform configuration and confirm the resulting infrastructure state.

## Objectives

- Understand the purpose of `terraform apply`.
- Apply an existing Terraform configuration.
- Review Terraform's assessment of the current infrastructure.
- Understand a no-change apply operation.
- Inspect Terraform output values.
- Verify the Azure Resource Group using Azure CLI.
- Confirm the resource provisioning state.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify deployed infrastructure |
| PowerShell | Execute Terraform and Azure CLI commands |
| HCL | Terraform configuration language |

## Project Structure

```
terraform-azure-cloud-security/
├── providers.tf
├── main.tf
├── data.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

The exact files may vary as the project develops.

### File Descriptions

- **`providers.tf`** — Configures the Terraform requirements and AzureRM provider.
- **`main.tf`** — Defines the Azure Resource Group and Terraform outputs.
- **`data.tf`** — Defines the Azure client configuration data source.
- **`terraform.tfvars`** — Supplies input variable values.
- **`.terraform.lock.hcl`** — Records the selected provider version and checksums.
- **`.terraform/`** — Contains local Terraform working data and should generally be excluded from Git.

## Step 1: Apply the Terraform Configuration

From the project directory, I executed:

```
terraform apply
```

Terraform refreshed the existing Azure Resource Group and read the Azure client configuration data source.

It compared the current infrastructure against the declared configuration and reported:

```
No changes. Your infrastructure matches the configuration.
```

Terraform then completed the apply operation successfully.

## Step 2: Review the Apply Result

The terminal displayed:

```
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
```

This means Terraform did not need to create, modify, or destroy any infrastructure resources during this operation.

The existing Azure Resource Group already matched the Terraform configuration.

**Important:** A successful apply does not necessarily mean that new infrastructure was created. Terraform may complete successfully without changing resources when the configuration and existing infrastructure already match.

## Step 3: Review Terraform Outputs

After applying the configuration, Terraform displayed the configured outputs, including:

- `resource_group_id`
- `resource_group_location`
- `resource_group_name`
- `subscription_id`
- `tenant_id`

These outputs provide useful information about the existing Azure Resource Group and the Azure provider's authenticated context.

Subscription and tenant identifiers are not passwords, but avoid exposing them unnecessarily in public screenshots or documentation.

## Step 4: Verify the Azure Resource Group

I used Azure CLI to verify the existing Resource Group:

```
az group show --name terraform-day3-rg
```

The command returned the Resource Group details, including its name, location, resource ID, and provisioning state.

The output confirmed:

```
{
  "name": "terraform-day3-rg",
  "location": "eastus",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Resources/resourceGroups"
}
```

The Azure CLI verification independently confirmed that the Resource Group existed and its provisioning state was `Succeeded`.

## Day 8 Results

- [x] `terraform apply` executed successfully.
- [x] Existing Azure infrastructure refreshed.
- [x] No infrastructure changes were required.
- [x] Apply completed with 0 resources added.
- [x] Apply completed with 0 resources changed.
- [x] Apply completed with 0 resources destroyed.
- [x] Terraform outputs reviewed.
- [x] Azure Resource Group verified using Azure CLI.
- [x] Provisioning state confirmed as `Succeeded`.

## Key Learnings

1. `terraform apply` applies the planned changes to infrastructure.
2. Terraform can complete an apply without changing resources.
3. A no-change apply indicates that no infrastructure modifications were required for the current configuration and state.
4. Terraform outputs expose useful resource attributes and configuration information.
5. Azure CLI can independently verify cloud resource details.
6. A successful Terraform operation and an Azure provisioning-state check provide complementary evidence.

## Security and Best Practices

- Review the Terraform plan before applying infrastructure changes.
- Investigate unexpected resource replacements or deletions.
- Use appropriate Azure permissions for the resources managed by Terraform.
- Never commit credentials, access tokens, or sensitive Terraform state files to a public repository.
- Avoid publishing sensitive values in Terraform outputs or screenshots.
- Keep `.terraform/` and local state files out of version control.

## Screenshot Evidence

1. `Screenshot Terraform-Day8-01-Terraform-Apply.png` — Shows the successful apply operation, no-change result, and Terraform outputs.
2. `Screenshot Terraform-Day8-02-Azure-Verification.png` — Shows the Azure CLI Resource Group details and successful provisioning state.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 8 of 23  
**Status:** Completed
