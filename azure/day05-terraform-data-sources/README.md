# Day 5: Terraform Data Sources

## Overview

Day 5 focused on using Terraform data sources to retrieve information about existing Azure configuration and the currently authenticated Azure identity.

I used the AzureRM provider's `azurerm_client_config` data source to retrieve the active subscription ID and tenant ID. I then exposed these values through Terraform outputs and verified the results.

This exercise demonstrates how Terraform can read existing information without creating additional cloud resources.

## Objectives

- Understand Terraform data sources.
- Configure the AzureRM client configuration data source.
- Retrieve the active Azure subscription ID.
- Retrieve the Azure tenant ID.
- Define outputs for data source values.
- Format and validate the configuration.
- Review the execution plan.
- Apply the configuration and verify the outputs.
- Confirm the existing Azure Resource Group using Azure CLI.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Interact with Azure resources and data |
| Azure CLI | Verify Azure configuration |
| PowerShell | Run Terraform and Azure CLI commands |
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

### File Descriptions

- **`providers.tf`** — Configures the Terraform requirements and AzureRM provider.
- **`main.tf`** — Defines the Azure Resource Group, input variables, and Terraform outputs.
- **`data.tf`** — Defines the Azure client configuration data source.
- **`terraform.tfvars`** — Supplies input variable values.
- **`.terraform.lock.hcl`** — Records the selected provider version and checksums.
- **`.terraform/`** — Contains local Terraform working data and should generally be excluded from Git.

## Step 1: Define the Azure Client Configuration Data Source

I created `data.tf` and added the following configuration:

```
data "azurerm_client_config" "current" {}
```

### Explanation

- `data` declares a Terraform data source.
- `azurerm_client_config` identifies the AzureRM client configuration data source.
- `current` is the local name used to reference the data source.
- `{}` is an empty configuration block because this data source does not require additional arguments for this example.

The data source reads information about the Azure identity and configuration used by the provider.

## Step 2: Expose Data Source Values Through Outputs

I added outputs in `main.tf` to retrieve the subscription ID and tenant ID.

```
data "azurerm_client_config" "current" {}

output "subscription_id" {
  value = data.azurerm_client_config.current.subscription_id
}

output "tenant_id" {
  value = data.azurerm_client_config.current.tenant_id
}
```

These outputs reference attributes returned by the data source.

**Important:** The `data "azurerm_client_config" "current" {}` block should be declared only once across the Terraform configuration. If it is already in `data.tf`, keep the outputs in `main.tf` without duplicating the data source declaration.

## Step 3: Format and Validate

I executed the following PowerShell commands:

```
terraform fmt
terraform validate
```

The validation completed successfully:

```
Success! The configuration is valid.
```

Formatting standardises the configuration, while validation checks the Terraform configuration for syntax and structural issues.

## Step 4: Review the Execution Plan

I ran:

```
terraform plan
```

Terraform refreshed the data source and displayed the proposed output changes.

The plan indicated that Terraform could update the output values without creating, modifying, or destroying Azure infrastructure.

Reviewing the plan before applying changes helps confirm that the configuration behaves as expected.

## Step 5: Apply the Configuration

I executed:

```
terraform apply
```

Terraform completed successfully:

```
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
```

No Azure infrastructure resources were created, modified, or destroyed during this operation. Terraform updated the output information using the data source values.

## Step 6: Display Terraform Outputs

I displayed all configured outputs:

```
terraform output
```

I also retrieved individual values:

```
terraform output subscription_id
terraform output tenant_id
```

These commands displayed the subscription and tenant identifiers associated with the active Azure provider configuration.

## Step 7: Verify the Existing Azure Resource Group

I used Azure CLI to inspect the existing Resource Group:

```
az group show --name terraform-day3-rg --output json
```

The response displayed the Resource Group's details, including its name, location, resource ID, and provisioning state.

This provided an additional check that the existing Azure resource was still available.

## Day 5 Results

- [x] Azure client configuration data source defined.
- [x] Subscription ID retrieved.
- [x] Tenant ID retrieved.
- [x] Terraform outputs configured.
- [x] Configuration formatted.
- [x] Terraform validation passed.
- [x] Execution plan reviewed.
- [x] Terraform apply completed successfully.
- [x] Individual outputs retrieved.
- [x] Existing Azure Resource Group verified.

## Key Learnings

1. Terraform data sources read information about existing resources or external configuration.
2. The `azurerm_client_config` data source exposes details about the Azure provider's authenticated context.
3. Data source attributes can be referenced using expressions such as `data.azurerm_client_config.current.subscription_id`.
4. Outputs make selected data source values easy to inspect.
5. Terraform can update outputs without creating or modifying cloud resources.
6. Data sources complement Terraform-managed resources by allowing configurations to use information that already exists.

## Security and Best Practices

- Never commit passwords, access tokens, client secrets, or other authentication credentials.
- Avoid exposing sensitive values through outputs.
- Subscription IDs and tenant IDs are identifiers rather than passwords, but consider whether you want them visible in a public portfolio.
- Do not commit Terraform state files or sensitive configuration files.
- Exclude local Terraform working data using `.gitignore`.
- Review the execution plan before applying changes.

## Screenshot Evidence

The following screenshots document the Day 5 workflow:

1. `Screenshot Terraform-Day5-02-Data-Source-data.tf.png`
2. `Screenshot Terraform-Day5-03-Data-Source-main.tf.png`
3. `Screenshot Terraform-Day5-04-Fmt-Validate.png`
4. `Screenshot Terraform-Day5-05-Plan-Data-Source.png`
5. `Screenshot Terraform-Day5-06-Apply-Data-Source.png`
6. `Screenshot Terraform-Day5-07-Terraform-Output.png`
7. `Screenshot Terraform-Day5-08-Individual-Data-Outputs.png`
8. `Screenshot Terraform-Day5-09-Azure-Verification.png`

## Challenge: 22-Day Terraform Learning Journey

**Day:** 5 of 22  
**Status:** Completed
