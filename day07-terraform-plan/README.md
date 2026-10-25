# Day 7: Terraform Plan

## Overview

Day 7 focused on understanding the `terraform plan` command and how Terraform compares the declared configuration with the current infrastructure state.

I ran `terraform plan` in my Azure Terraform project. Terraform refreshed the existing Azure Resource Group and read the Azure client configuration data source. The plan completed successfully and reported no infrastructure changes.

This exercise demonstrates how to preview Terraform changes before applying them.

## Objectives

- Understand the purpose of `terraform plan`.
- Review the difference between Terraform configuration and deployed infrastructure.
- Refresh information about existing Azure resources.
- Read data source information from Azure.
- Identify whether infrastructure changes are required.
- Understand why reviewing a plan is important before deployment.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Interact with Azure resources |
| Azure CLI authentication | Authenticate to Azure |
| PowerShell | Execute Terraform commands |
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

The configuration files shown are part of the existing project. The exact directory contents may vary as the project develops.

## Step 1: Run Terraform Plan

From the project directory, I executed:

```
terraform plan
```

Terraform compared the declared configuration with the existing infrastructure and refreshed information from Azure.

The output showed the following operations:

- Read the `azurerm_client_config.current` data source.
- Refresh the existing `azurerm_resource_group.demo` resource.
- Compare the infrastructure with the Terraform configuration.

## Step 2: Review the Result

The command completed with the following result:

```
No changes. Your infrastructure matches the configuration.
```

Terraform also explained that it had compared the real infrastructure against the configuration and found no differences requiring changes.

### What Does “No Changes” Mean?

A no-change plan means Terraform did not identify any infrastructure changes to make for the current configuration and state.

In this exercise, the existing Azure Resource Group already matched the declared configuration. No new resource creation, modification, or destruction was required.

## Step 3: Understand the Terraform Workflow

The typical Terraform workflow is:

```
Write Configuration
        |
        v
terraform init
        |
        v
terraform validate
        |
        v
terraform plan
        |
        v
Review Proposed Changes
        |
        v
terraform apply
```

The plan step allows you to inspect intended changes before applying them.

**Important:** Running `terraform plan` does not apply the proposed infrastructure changes.

## Day 7 Results

- [x] Terraform plan executed successfully.
- [x] Existing Azure Resource Group refreshed.
- [x] Azure client configuration data source read successfully.
- [x] Plan output reviewed.
- [x] No infrastructure changes were required.
- [x] Terraform configuration matched the existing infrastructure at the time of the plan.

## Key Learnings

1. `terraform plan` previews proposed infrastructure changes.
2. Terraform can refresh information about existing cloud resources during planning.
3. Data sources allow Terraform to read information from the provider.
4. A no-change plan indicates that Terraform found no differences requiring changes.
5. Reviewing a plan helps identify unexpected infrastructure modifications before applying them.
6. A successful plan does not guarantee that every future apply operation will succeed.

## Security and Best Practices

- Review the complete plan before applying infrastructure changes.
- Investigate unexpected resource replacements or deletions.
- Do not expose sensitive values in screenshots or public documentation.
- Never commit credentials or sensitive Terraform state files to a public repository.
- Use appropriate Azure permissions for the resources managed by Terraform.

## Screenshot Evidence

**Screenshot:** `Screenshot Terraform-Day7-01-Terraform-Plan.png`

The screenshot shows the `terraform plan` command, the Azure data source read, the Resource Group refresh, and the successful no-change result.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 7 of 23  
**Status:** Completed
