# Day 6: Terraform Initialisation

## Overview

Day 6 focused on understanding the `terraform init` command and how Terraform prepares a working directory for infrastructure management.

I initialised the existing Azure Terraform project, verified the AzureRM provider installation, inspected the dependency lock file, repeated initialization, and validated the configuration. Finally, I ran `terraform plan` to confirm that the existing Azure infrastructure matched the Terraform configuration.

## Objectives

- Understand the purpose of `terraform init`.
- Initialise the Terraform working directory.
- Verify the AzureRM provider installation.
- Inspect the `.terraform` directory.
- Understand the `.terraform.lock.hcl` dependency lock file.
- Run initialisation more than once.
- Validate the Terraform configuration.
- Review the execution plan and confirm that no infrastructure changes are required.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure infrastructure |
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
│   └── providers/
└── README.md
```

The exact configuration files in the working directory may vary as the project develops.

### File and Directory Descriptions

- **`providers.tf`** — Declares the Terraform and AzureRM provider requirements.
- **`main.tf`** — Contains the Terraform resource and output configuration.
- **`data.tf`** — Defines the Azure client configuration data source used in the project.
- **`terraform.tfvars`** — Supplies input variable values.
- **`.terraform.lock.hcl`** — Records the selected provider version and package checksums.
- **`.terraform/`** — Contains local Terraform working data, including provider components.

## Step 1: Verify the Terraform Version

I ran the following command in PowerShell:

```
terraform version
```

The output showed:

```
Terraform v1.16.2
on windows_amd64
```

It also identified the installed AzureRM provider as version `4.81.0`.

This confirmed that Terraform was available from the command line and that the AzureRM provider was installed.

## Step 2: Initialise the Terraform Working Directory

I executed:

```
terraform init
```

Terraform initialised the working directory and reused the AzureRM provider version recorded in the dependency lock file.

The command completed successfully:

```
Terraform has been successfully initialised!
```

### What `terraform init` Does

- Initialises the Terraform working directory.
- Downloads or reuses the required providers.
- Reads provider requirements from the configuration.
- Creates or updates Terraform's local working data.
- Creates or updates the dependency lock file when necessary.

Initialisation prepares the directory for commands such as `terraform validate`, `terraform plan`, and `terraform apply`.

## Step 3: Inspect the Provider Directory

I inspected Terraform's local working directory:

```
dir .terraform
```

The output showed the `providers` directory.

This demonstrated that Terraform had created local working data for the installed provider.

The `.terraform` directory is generated automatically and should generally be excluded from version control.

## Step 4: Inspect the Dependency Lock File

I opened the dependency lock file:

```
notepad .terraform.lock.hcl
```

The file recorded the AzureRM provider version as `4.81.0`, along with its version constraint and package checksums.

The lock file helps Terraform select consistent provider packages across future initialisation runs.

**Best practice:** Let Terraform maintain `.terraform.lock.hcl` automatically rather than manually editing provider versions or checksum entries.

## Step 5: Run Terraform Initialisation Again

I ran the initialisation command a second time:

```
terraform init
```

Terraform reused the previously selected AzureRM provider and completed initialisation successfully again.

This showed that you can safely repeat initialisation when the configuration and provider requirements haven't changed.

Terraform may also ask for initialisation again if provider requirements, modules, or backend configuration change.

## Step 6: Validate the Configuration

I executed:

```
terraform validate
```

The output confirmed:

```
Success! The configuration is valid.
```

Validation checks whether the Terraform configuration is syntactically and structurally valid. It does not guarantee that every planned cloud operation will succeed.

## Step 7: Review the Terraform Plan

I ran:

```
terraform plan
```

Terraform refreshed the existing Azure Resource Group information and read the Azure client configuration data source.

The result was:

```
No changes. Your infrastructure matches the configuration.
```

This confirmed that Terraform detected no differences between the current infrastructure and the declared configuration at that time.

This planning operation created, modified, or destroyed no Azure resources.

## Day 6 Results

- [x] Terraform version verified.
- [x] AzureRM provider version verified.
- [x] Terraform working directory initialised successfully.
- [x] Provider directory inspected.
- [x] Dependency lock file inspected.
- [x] `terraform init` repeated successfully.
- [x] Configuration validation passed.
- [x] Terraform plan reviewed.
- [x] Existing infrastructure reported no changes.

## Key Learnings

1. `terraform init` prepares a Terraform working directory.
2. Providers allow Terraform to communicate with cloud platforms.
3. The `.terraform` directory stores local working data.
4. The `.terraform.lock.hcl` file records provider selections and checksums.
5. Repeating `terraform init` can reuse previously installed providers.
6. `terraform validate` checks configuration structure and syntax.
7. `terraform plan` previews changes without applying them.
8. A no-change plan indicates that Terraform found no differences requiring changes at that time.

## Security and Best Practices

- Do not manually modify the dependency lock file unless you have a specific reason and understand the consequences.
- Commit `.terraform.lock.hcl` so collaborators can use consistent provider selections.
- Exclude the `.terraform` directory from Git.
- Never commit Terraform state files or credentials to a public repository.
- Review the plan before applying infrastructure changes.
- Keep provider version constraints appropriate for your project.

## Screenshot Evidence

The following screenshots document the Day 6 workflow:

1. `Screenshot Terraform-Day6-01-Terraform-Version.png`
2. `Screenshot Terraform-Day6-02-Terraform-Init.png`
3. `Screenshot Terraform-Day6-03-Terraform-Provider-Directory.png`
4. `Screenshot Terraform-Day6-04-Terraform-Lock-File.png`
5. `Screenshot Terraform-Day6-05-Terraform-Init-Repeat.png`
6. `Screenshot Terraform-Day6-06-Init-Validate.png`
7. `Screenshot Terraform-Day6-07-Plan-No-Infrastructure-Changes.png`

## Challenge: 23-Day Terraform Learning Journey

**Day:** 6 of 23  
**Status:** Completed
