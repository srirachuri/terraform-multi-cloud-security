# Day 1: Terraform Setup and Azure Authentication

## Overview

Day 1 focused on setting up the Terraform development environment on Windows, authenticating with Microsoft Azure, configuring the AzureRM provider, and verifying that Terraform was ready for Infrastructure as Code (IaC) development.

This setup established the foundation for the Azure infrastructure, identity and access management, networking, and cloud security projects in this 23-day learning journey.

## Objectives

- Install and verify Terraform.
- Authenticate to Microsoft Azure using Azure CLI.
- Verify the active Azure subscription.
- Create the Terraform project directory.
- Configure the AzureRM provider.
- Initialise the Terraform working directory.
- Validate the Terraform configuration.
- Verify the generated Terraform files.

## Technologies Used

| Technology | Version / Details |
|---|---|
| Terraform | v1.6.2 |
| AzureRM Provider | v4.81.0, if confirmed by the installed provider |
| Cloud Platform | Microsoft Azure |
| CLI | Azure CLI |
| Operating System | Windows |
| IaC Language | HashiCorp Configuration Language (HCL) |

## Project Structure

The initial Terraform working directory contains the following files and directories:

```
terraform-azure-cloud-security/
├── providers.tf
├── .terraform.lock.hcl
├── .terraform/
└── .gitignore
```

### File and Directory Purposes

- **` providers.tf`** — Defines the Terraform version requirement, AzureRM provider requirement, and Azure provider configuration.
- **`.terraform.lock.hcl`** — Records the selected provider version and package checksums to support consistent provider installation.
- **`.terraform/`** — Contains local Terraform working data and downloaded provider components. This directory should not normally be committed to Git.
- **`.gitignore`** — Specifies files and directories that Git should exclude from version control.

The exact files present may vary depending on the setup and commands executed.

## Step 1: Verify Terraform Installation

I opened PowerShell and ran:

```
terraform version
```

Expected output includes the installed Terraform version, for example:

```
Terraform v1.6.2
on windows_amd64
```

This confirms that Terraform is installed and accessible from the command line.

## Step 2: Authenticate to Azure

I sign in using Azure CLI:

```
az login
```

After successful authentication, I checked the active account and subscription:

```
az account show
```

This displays information about the signed-in Azure account and the currently selected subscription.

**Security note:** Never commit access tokens, passwords, client secrets, or other authentication credentials to a public repository.

## Step 3: Create the Project Directory

I created a dedicated directory for the Azure Terraform project:

```
mkdir "$HOME\terraform-azure-cloud-security"
```

Navigated into the directory:

```
cd "$HOME\terraform-azure-cloud-security"
```

I verify the current working directory:

```
Get-Location
```

If the directory already exists, navigate into it instead of creating it again.

## Step 4: Configure the AzureRM Provider

I created the provider configuration file:

```
notepad providers.tf
```

Added the following configuration and save the file:

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

- **`required_version`** — Specifies the minimum Terraform version required.
- **`required_providers`** — Declares the provider dependency required by the configuration.
- **`source`** — Identifies the HashiCorp AzureRM provider.
- **`version`** — Restricts provider selection to compatible versions in the AzureRM 4.x series.
- **`provider "azurerm"`** — Configures Terraform to use the Azure provider.
- **` features {}`** — Provides the required features configuration for the AzureRM provider.

The version constraint `~> 4.0` does not pin the provider to exactly v4.81.0. The selected version is recorded in `.terraform.lock.hcl`.

## Step 5: Initialise Terraform


```
terraform init
```

Terraform initialises the working directory and installs the required provider dependencies.

A successful initialisation displays a message similar to:

```
Terraform has been successfully initialised!
```

During the initial setup, Terraform reported a missing required provider because the provider configuration was not yet available. After creating and saving `providers.tf`, initialisation completed successfully.

## Step 6: Validate the Configuration


```
terraform validate
```

Expected result:

```
Success! The configuration is valid.
```

Validation checks the configuration for syntax and structural errors. It does not deploy resources or guarantee that Azure will accept a future deployment.

## Step 7: Verify the Project Files

List the files and directories, including hidden items:

```
Get-ChildItem -Force
```

Check for the expected files, including `providers.tf`, `.terraform.lock.hcl`, and the `.terraform` directory.

The exact contents depend on which commands ran successfully.

## Day 1 Results

- [x] Terraform installation verified.
- [x] Azure CLI authentication and subscription checked, as supported by the recorded evidence.
- [x] Project directory prepared.
- [x] AzureRM provider configuration created.
- [x] Terraform initialised successfully.
- [x] Configuration validation passed.
- [x] Generated Terraform working files inspected.

## Key Learnings

1. Terraform uses declarative configuration files to describe infrastructure.
2. Providers allow Terraform to interact with cloud platforms such as Azure.
3. Terraform can use Azure CLI authentication in a compatible local authentication setup.
4. `terraform init` prepares the working directory and installs provider dependencies.
5. `terraform validate` checks configuration structure without deploying infrastructure.
6. The dependency lock file records the selected provider version and checksums.

## Security Notes

- No Azure infrastructure resources were created as part of this Day 1 setup.
- Authentication does not automatically grant permission to create every Azure resource. The identity must have the appropriate permissions.
- Do not commit the `.terraform/` directory or Terraform state files.
- Keep credentials and sensitive configuration values out of the repository.
- Commit `.terraform.lock.hcl` so the selected provider version can be reproduced consistently.

## Screenshot Evidence

Store the original Day 1 screenshots in a `screenshots/` subdirectory. Use the original filenames and include evidence of the Terraform version, Azure authentication, provider configuration, initialisation, and validation where available.

## Challenge: 22-Day Terraform Learning Journey

**Day:** 1 of 22  
**Status:** Completed
