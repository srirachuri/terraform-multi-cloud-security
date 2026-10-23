# Day 3: Terraform Variables and `terraform.tfvars`

## Overview

Day 3 focused on making Terraform configurations more flexible by separating input values from resource definitions. I practiced declaring variables, assigning values through a `terraform.tfvars` file, and using Terraform outputs to verify an Azure Resource Group deployment.

This exercise builds on the previous days by introducing a cleaner way to manage configuration values.

## Objectives

- Declare Terraform input variables.
- Use variables in Azure resource definitions.
- Store configuration values in `terraform.tfvars`.
- Format and validate Terraform files.
- Review the execution plan before deployment.
- Apply the Terraform configuration.
- Display outputs and verify the Azure resource.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify deployed resources |
| PowerShell | Execute commands |
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

- **`providers.tf`** — Configures the AzureRM provider and Terraform requirements.
- **`main.tf`** — Declares input variables, the Azure Resource Group, and a Terraform output.
- **`terraform.tfvars`** — Supplies values for the declared variables.
- **`.terraform.lock.hcl`** — Records the selected provider version and checksums.
- **`.terraform/`** — Contains local Terraform working data and should generally be excluded from Git.

## Step 1: Declare Variables

I declared variables for the resource group name and Azure region in `main.tf`.

```
variable "resource_group_name" {
  type        = string
  description = "Name of the Azure Resource Group"
  default     = "terraform-day3-rg"
}

variable "location" {
  type        = string
  description = "Azure region where the Resource Group will be created"
  default     = "eastus"
}
```

Variables help avoid hardcoding configuration values directly into resource definitions. They also make it easier to reuse a configuration with different names or locations.

## Step 2: Configure `terraform.tfvars`

I created a `terraform.tfvars` file to supply the values used by the configuration.

```
resource_group_name = "terraform-day3-rg"
location            = "eastus"
```

Terraform automatically loads values from a file named `terraform.tfvars` in the current working directory.

This separates the input values from the main resource configuration and makes the project easier to maintain.

## Step 3: Define the Azure Resource Group

I used the declared variables to configure an Azure Resource Group.

```
resource "azurerm_resource_group" "demo" {
  name     = var.resource_group_name
  location = var.location
}
```

The resource references the input variables instead of repeating fixed values.

**Resource details**

- Resource type: Azure Resource Group
- Resource name: `terraform-day3-rg`
- Location: `eastus`

## Step 4: Configure a Terraform Output

I configured an output to display the resource group's name.

```
output "resource_group_name" {
  value = azurerm_resource_group.demo.name
}
```

Outputs provide useful information about resources managed by Terraform and help confirm deployment results.

## Step 5: Format and Validate

I ran the following commands in PowerShell:

```
terraform fmt
terraform validate
```

Formatting keeps the configuration consistent and readable. Validation checks for configuration and syntax errors.

The validation command completed successfully.

## Step 6: Review the Execution Plan

Before deploying the resource, I ran:

```
terraform plan
```

The plan showed that Terraform intended to create one Azure Resource Group.

```
Plan: 1 to add, 0 to change, 0 to destroy.
```

Reviewing the plan helps identify unexpected infrastructure changes before applying them.

## Step 7: Apply the Configuration

After reviewing the plan, I executed:

```
terraform apply
```

Terraform created the Azure Resource Group successfully.

The apply output confirmed that one resource was added, with no changes or destructions.

## Step 8: Verify the Output and Azure Resource

I displayed the Terraform output:

```
terraform output resource_group_name
```

I also checked the resource group through Azure CLI:

```
az group show --name terraform-day3-rg --output json
```

The Azure CLI response confirmed that the resource group existed and had a successful provisioning state.

## Day 3 Results

- [x] Input variables declared.
- [x] `terraform.tfvars` configured.
- [x] Azure Resource Group defined using variables.
- [x] Terraform output configured.
- [x] Configuration formatted and validated.
- [x] Execution plan reviewed.
- [x] Resource Group created successfully.
- [x] Terraform output checked.
- [x] Azure Resource Group verified using Azure CLI.

## Key Learnings

1. Input variables make Terraform configurations reusable.
2. The `terraform.tfvars` file supplies values for declared variables.
3. Resource definitions can reference variables using `var.variable_name`.
4. Terraform outputs display useful resource attributes.
5. `terraform plan` previews intended changes before deployment.
6. `terraform apply` creates or updates infrastructure according to the configuration.
7. Azure CLI can independently verify the existence and provisioning state of a resource.

## Security and Best Practices

- Avoid hardcoding passwords, tokens, and other secrets in Terraform files.
- Do not commit sensitive values or Terraform state files to a public repository.
- Use `.gitignore` to exclude local Terraform working data and sensitive files.
- Review the execution plan before applying changes.
- Verify cloud resources after deployment.
- Remove temporary infrastructure when it is no longer required, after checking that it is safe to delete.

## Screenshot Evidence

The following screenshots document the Day 3 workflow:

1. `Screenshot Terraform-Day3-01-Project-Folder.png`
2. `Screenshot Terraform-Day3-02-Variables-main.tf.png`
3. `Screenshot Terraform-Day3-03-Fmt-Validate.png`
4. `Screenshot Terraform-Day3-04-terraform-tfvars.png`
5. `Screenshot Terraform-Day3-05-Plan-Variables.png`
6. `Screenshot Terraform-Day3-06-Apply-Variables.png`
7. `Screenshot Terraform-Day3-07-Terraform-Output.png`
8. `Screenshot Terraform-Day3-08-Azure-Verification.png`

## Challenge: 23-Day Terraform Learning Journey

**Day:** 3 of 23  
**Status:** Completed
