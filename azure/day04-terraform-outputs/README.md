# Day 4: Terraform Outputs

## Overview

Day 4 focused on using Terraform outputs to display useful information about Azure resources. I configured outputs for an Azure Resource Group's name, location, and resource ID, then verified the values through Terraform and Azure CLI.

This exercise builds on the previous days by showing how Terraform can expose resource attributes after deployment.

## Objectives

- Understand Terraform output blocks.
- Retrieve resource attributes from an Azure Resource Group.
- Configure outputs in `main.tf`.
- Format and validate the Terraform configuration.
- Review the execution plan.
- Apply the configuration and inspect output values.
- Verify the Azure resource using Azure CLI.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify Azure resources |
| PowerShell | Execute Terraform and Azure CLI commands |
| HCL | Terraform configuration language |

## Project Structure

```
terraform-azure-cloud-security/
├── providers.tf
├── main.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

### File Descriptions

- **`providers.tf`** — Configures Terraform requirements and the AzureRM provider.
- **`main.tf`** — Defines the Azure Resource Group and Terraform outputs.
- **`terraform.tfvars`** — Supplies input variable values.
- **`.terraform.lock.hcl`** — Records the selected provider version and checksums.
- **`.terraform/`** — Contains local Terraform working data and should generally be excluded from Git.

## Step 1: Configure Terraform Outputs

I added output blocks to `main.tf` to retrieve useful attributes from the Azure Resource Group.

```
output "resource_group_name" {
  value = azurerm_resource_group.demo.name
}

output "resource_group_location" {
  value = azurerm_resource_group.demo.location
}

output "resource_group_id" {
  value = azurerm_resource_group.demo.id
}
```

### Output Explanation

- **`resource_group_name`** — Displays the name of the Resource Group.
- **`resource_group_location`** — Displays the Azure region where the Resource Group exists.
- **`resource_group_id`** — Displays the resource's full Azure resource ID.

The outputs reference attributes of `azurerm_resource_group.demo`, so Terraform obtains their values from the resource configuration and state.

## Step 2: Format and Validate

I executed the following PowerShell commands:

```
terraform fmt
terraform validate
```

The configuration passed validation:

```
Success! The configuration is valid.
```

Formatting improves readability, while validation checks the configuration for syntax and structural issues.

## Step 3: Review the Execution Plan

I ran:

```
terraform plan
```

Terraform displayed the proposed output values.

The plan indicated that the outputs would be added without creating, modifying, or destroying Azure infrastructure.

This demonstrates that Terraform can update its output information without requiring changes to the underlying resource.

## Step 4: Apply the Configuration

I executed:

```
terraform apply
```

Terraform completed successfully:

```
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
```

No infrastructure resources were created, modified, or destroyed during this apply operation. Terraform updated the output information associated with the existing configuration.

## Step 5: Display All Outputs

I ran:

```
terraform output
```

This displayed the configured output values, including the Resource Group name, location, and resource ID.

## Step 6: Display Individual Outputs

I also retrieved each output separately:

```
terraform output resource_group_name
terraform output resource_group_location
terraform output resource_group_id
```

Retrieving individual outputs is useful when checking a specific attribute or using Terraform output values in scripts and other workflows.

## Step 7: Verify the Azure Resource

I used Azure CLI to inspect the Resource Group:

```
az group show --name terraform-day3-rg --output json
```

The response displayed the Resource Group's metadata, including its location, resource ID, and provisioning state.

This provided an independent check of the Azure resource beyond the Terraform output commands.

## Day 4 Results

- [x] Terraform outputs configured.
- [x] Resource Group name output retrieved.
- [x] Resource Group location output retrieved.
- [x] Resource Group ID output retrieved.
- [x] Terraform formatting completed.
- [x] Configuration validation passed.
- [x] Execution plan reviewed.
- [x] Terraform apply completed successfully.
- [x] All outputs displayed.
- [x] Azure Resource Group verified using Azure CLI.

## Key Learnings

1. Terraform outputs expose useful attributes of managed resources.
2. Outputs can reference resource attributes using Terraform expressions.
3. `terraform output` displays output values saved in Terraform state.
4. Individual outputs can be retrieved by specifying their names.
5. A Terraform apply can update outputs without changing infrastructure.
6. Azure CLI can independently verify resource details.

## Security and Best Practices

- Avoid exposing passwords, access tokens, or other secrets through outputs.
- Mark sensitive output values as `sensitive = true` when appropriate, while remembering that sensitive values can still exist in Terraform state.
- Never commit Terraform state files or credentials to a public repository.
- Review Terraform plans before applying changes.
- Keep local Terraform working data out of version control.

## Screenshot Evidence

The following screenshots document the Day 4 workflow:

1. `Screenshot Terraform-Day4-01-Project-Structure.png`
2. `Screenshot Terraform-Day4-02-Outputs-main.tf.png`
3. `Screenshot Terraform-Day4-03-Fmt-Validate.png`
4. `Screenshot Terraform-Day4-04-Plan-Outputs.png`
5. `Screenshot Terraform-Day4-05-Apply-Outputs.png`
6. `Screenshot Terraform-Day4-06-Terraform-Output-All.png`
7. `Screenshot Terraform-Day4-07-Individual-Outputs.png`
8. `Screenshot Terraform-Day4-08-Azure-Verification.png`

## Challenge: 22-Day Terraform Learning Journey

**Day:** 4 of 22  
**Status:** Completed
