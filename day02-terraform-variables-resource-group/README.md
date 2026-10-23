# Day 2: Terraform Variables, Outputs, and Azure Resource Groups

## Overview

Day 2 focused on using Terraform variables and outputs to manage an Azure Resource Group. I practised the Terraform workflow of formatting, validating, planning, applying, verifying, and destroying cloud infrastructure.

This exercise introduced the relationship between Terraform configuration, Azure resources, and infrastructure lifecycle management.

## Objectives

- Define and use Terraform variables.
- Reference variables in resource configuration.
- Create an Azure Resource Group using Terraform.
- Configure a Terraform output.
- Run Terraform formatting and validation.
- Review the execution plan before deployment.
- Apply the configuration and verify the resource in Azure.
- Destroy the test infrastructure and confirm cleanup.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify Azure resources |
| PowerShell | Run Terraform and Azure CLI commands |
| HCL | Write Terraform configuration |

## Project Structure

```
terraform-azure-cloud-security/
├── providers.tf
├── main.tf
├── .terraform.lock.hcl
├── .terraform/
└── .gitignore
```

### File Descriptions

- **`providers.tf`** — Configures the AzureRM provider and Terraform requirements.
- **`main.tf`** — Defines the resource group, variables, and output used in this exercise.
- **`.terraform.lock.hcl`** — Records the selected provider version and checksums.
- **`.terraform/`** — Contains local Terraform working data and should not normally be committed to Git.
- **`.gitignore`** — Excludes local or sensitive files from version control.

## Step 1: Define Terraform Variables

I defined variables for the Azure Resource Group name and deployment location.

```
variable "resource_group_name" {
  default = "terraform-day2-rg"
}

variable "location" {
  default = "eastus"
}
```

### Why Variables Matter

Variables make Terraform configurations easier to reuse and modify. Instead of repeating values throughout the configuration, resources can reference variables.

For this exercise, the resource group name and location were supplied through variables.

## Step 2: Create the Azure Resource Group

I configured an Azure Resource Group using the AzureRM provider.

```hcl
resource "azurerm_resource_group" "demo" {
  name     = var.resource_group_name
  location = var.location
}
```

The configuration references the variables defined earlier to set the resource group's name and location.

**Resource details:**

- Resource type: Azure Resource Group
- Resource name: `terraform-day2-rg`
- Location: `eastus`

## Step 3: Configure a Terraform Output

I added an output to display the resource group's name after Terraform creates it.

```
output "resource_group_name" {
  value = azurerm_resource_group.demo.name
}
```

Outputs make useful resource attributes available after an operation. They are helpful for verifying deployment results and passing values to other configurations or workflows.

## Step 4: Format and Validate

I formatted the Terraform configuration and checked its validity.

```
terraform fmt
terraform validate
```

Formatting keeps Terraform files consistent and readable. Validation checks the configuration for syntax and structural errors.

## Step 5: Review the Execution Plan

I ran:

```
terraform plan
```

The plan showed that Terraform intended to create one Azure Resource Group.

The reviewed plan was:

```
Plan: 1 to add, 0 to change, 0 to destroy.
```

Reviewing the plan before applying helps identify unexpected changes before they affect cloud infrastructure.

## Step 6: Apply the Configuration

After reviewing the plan, I ran:

```
terraform apply
```

Terraform created the Azure Resource Group. The operation completed successfully, and the output displayed the resource group name.

## Step 7: Verify the Azure Resource

I checked the Terraform output:

```
terraform output
```

I also used Azure CLI to inspect the resource groups:

```
az group list --output table
```

This provided an additional verification step using Azure rather than relying only on Terraform's output.

## Step 8: Destroy the Test Infrastructure

After verifying the resource group, I removed the test infrastructure:

```
terraform destroy
```

Terraform displayed the planned deletion and requested confirmation before proceeding.

The destroy operation completed successfully, reporting that one resource was destroyed. I then checked the resource group list again to verify the cleanup.

**Cleanup is an important cloud security and cost-management practice:** resources that are no longer required should not be left running unnecessarily.

## Day 2 Results

- [x] Terraform variables defined and referenced.
- [x] Azure Resource Group configuration created.
- [x] Terraform output configured.
- [x] Configuration formatted and validated.
- [x] Execution plan reviewed.
- [x] Resource Group created successfully.
- [x] Resource verified using Terraform output and Azure CLI.
- [x] Test infrastructure destroyed successfully.
- [x] Cleanup verified.

## Key Learnings

1. Terraform variables make configurations easier to reuse and maintain.
2. Terraform resources describe the infrastructure to be managed.
3. Outputs expose useful information about managed resources.
4. `terraform plan` previews intended changes before execution.
5. `terraform apply` applies the reviewed configuration.
6. Azure CLI provides an independent way to inspect cloud resources.
7. `terraform destroy` removes resources managed by the configuration.
8. Verifying cleanup helps reduce unnecessary cloud costs and resource exposure.

## Security and Cost Considerations

- Review Terraform plans before applying changes.
- Use appropriate Azure permissions when managing infrastructure.
- Avoid committing credentials, secrets, or Terraform state files.
- Exclude the local `.terraform/` directory from version control.
- Destroy temporary resources when they are no longer needed.
- Verify the result of cleanup rather than assuming that deletion succeeded.

## Screenshot Evidence

The following screenshots document the Day 2 workflow:

1. `Screenshot Terraform-Day2-01-Terraform-Validate.png`
2. `Screenshot Terraform-Day2-02-Plan-Resource-Group.png`
3. `Screenshot Terraform-Day2-03-main.tf.png`
4. `Screenshot Terraform-Day2-04-Variables-Validate.png`
5. `Screenshot Terraform-Day2-05-Variables-Outputs-Plan.png`
6. `Screenshot Terraform-Day2-06-Apply-Output.png`
7. `Screenshot Terraform-Day2-07-Verification.png`
8. `Screenshot Terraform-Day2-08-destroyed-and-Verified.png`

## Challenge: 23-Day Terraform Learning Journey

**Day:** 2 of 23  
**Status:** Completed
