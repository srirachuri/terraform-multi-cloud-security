# Day 1: Terraform Setup and Azure Authentication

## Overview

Day 1 focuses on setting up the Terraform development environment on Windows, authenticating with Microsoft Azure, configuring the AzureRM provider, and verifying that Terraform is ready for infrastructure-as-code development.

This is the foundation for the upcoming Azure cloud infrastructure and security projects.

## Objectives

- Install and verify Terraform.
- Authenticate to Microsoft Azure using Azure CLI.
- Verify the active Azure subscription.
- Create the Terraform project directory.
- Configure the AzureRM provider.
- Initialize the Terraform working directory.
- Validate the Terraform configuration.
- Verify the generated Terraform files.

## Technologies Used

| Technology | Version / Details |
|---|---|
| Terraform | v1.6.2 |
| AzureRM Provider | v4.81.0 |
| Cloud Platform | Microsoft Azure |
| CLI | Azure CLI |
| Operating System | Windows |
| Scripting / IaC | HashiCorp Configuration Language (HCL) |

## Project Structure

After completing the setup, the project directory contains:

```
terraform-azure-cloud-security/
├── providers.tf
├── .terraform.lock.hcl
└── .terraform/
```

**File and directory purposes:**

- `providers.tf` — Defines the Terraform version requirement, AzureRM provider requirement, and provider configuration.
- `.terraform.lock.hcl` — Records the selected provider version and its checksums.
- `.terraform/` — Contains Terraform's local working data, including installed provider components.

The `.terraform` directory is generated automatically and should generally not be committed to Git.

## Step 1: Verify Terraform Installation

Open PowerShell and execute:

```
terraform version
```

Expected output includes:

```
Terraform v1.6.2
on windows_amd64
```

This confirms that Terraform is installed and accessible from the command line.

## Step 2: Authenticate to Azure

Sign in to Azure using the Azure CLI:

```
az login
```

If the browser-based authentication flow completes successfully, verify the active account and subscription:

```
az account show
```

This command displays information about the authenticated Azure account and the active subscription.

**Security note:** Never commit access tokens, passwords, client secrets, or other authentication credentials to a public repository.

## Step 3: Create the Project Directory

Create a dedicated directory for the Azure Terraform project:

```
mkdir "$HOME\terraform-azure-cloud-security"
```

Navigate into the directory:

```
cd "$HOME\terraform-azure-cloud-security"
```

Confirm the current working directory:

```
pwd
```

## Step 4: Create the Provider Configuration

Create the Terraform configuration file:

```
notepad providers.tf
```

Add the following configuration and save the file:

```
terraform {
  required_version = ">= 1.6.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

### Configuration Explanation

- `required_version` specifies the minimum Terraform version required.
- `required_providers` declares the AzureRM provider dependency.
- `source` identifies the official HashiCorp AzureRM provider.
- `version` constrains the provider to the compatible 4.x release series.
- `provider "azurerm"` configures the Azure provider.
- `features {}` supplies the required AzureRM provider features configuration.

## Step 5: Initialize Terraform

Run:

```
terraform init
```

Terraform downloads the required provider and initializes the working directory.

A successful initialization displays a message similar to:

```
Terraform has been successfully initialized!
```

During the initial setup, Terraform reported a missing required provider because the provider configuration was not yet available. After creating and saving `providers.tf`, initialization completed successfully.

## Step 6: Validate the Configuration

Run:

```
terraform validate
```

Expected result:

```
Success! The configuration is valid.
```

Validation checks the Terraform configuration for syntax errors and configuration issues. It does not prove that Azure resources can be deployed successfully.

## Step 7: Verify the Project Files

List the files and directories:

```
Get-ChildItem -Force
```

Confirm that the directory contains `providers.tf`, `.terraform.lock.hcl`, and `.terraform`.

Check the installed Terraform version again if necessary:

```
terraform version
```

## Day 1 Results

- [x] Terraform installation verified.
- [x] Azure CLI authentication completed.
- [x] Active Azure subscription checked.
- [x] Project directory created.
- [x] `providers.tf` configured.
- [x] Terraform initialized successfully.
- [x] Configuration validation passed.
- [x] Generated Terraform working files verified.

## Key Learnings

1. Terraform uses declarative configuration files to define infrastructure.
2. Providers allow Terraform to interact with cloud platforms such as Azure.
3. Azure CLI authentication provides an identity that Terraform can use when configured to do so.
4. `terraform init` prepares the working directory and installs provider dependencies.
5. `terraform validate` checks the configuration without deploying infrastructure.
6. The dependency lock file helps maintain consistent provider selections across runs.

## Important Notes

- No Azure infrastructure resources were created as part of the documented Day 1 setup.
- Authentication does not automatically grant permission to create every Azure resource; the identity must have appropriate access.
- Keep local Terraform working data out of version control.
- Do not commit Terraform state files, secrets, or sensitive configuration values.

---

**Challenge:** 23-Day Terraform Learning Journey  
**Day:** 1 of 23  
**Status:** Completed
