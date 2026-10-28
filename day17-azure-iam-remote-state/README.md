# Day 17: Azure IAM, Remote State, and State Locking with Terraform

## Overview

Day 17 focuses on Azure identity and access management concepts, Terraform remote state configuration, and state locking.

The project uses Terraform with Microsoft Azure and the AzureRM provider. It demonstrates configuring an Azure Storage Account backend to store Terraform state remotely and observing Terraform's state-locking behavior.

During the project, I also encountered an Azure identity permission error while attempting an identity-related operation. This demonstrated that successful authentication does not necessarily provide permission to perform every Azure directory operation.

## Objectives

- Configure a Terraform project for Azure.
- Initialise the required Terraform providers.
- Verify the active Azure subscription and tenant.
- Format and validate the Terraform configuration.
- Review the Terraform execution plan.
- Configure an AzureRM remote backend.
- Store Terraform state in Azure Blob Storage.
- Observe Terraform state locking during operations.
- Understand the difference between authentication and authorisation.
- Document permission limitations encountered during testing.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Azure resource management |
| Azure Blob Storage | Remote Terraform state storage |
| Azure CLI | Azure authentication and subscription checks |
| Azure Active Directory / Microsoft Entra ID | Identity and access management |
| PowerShell | Run Terraform and Azure CLI commands |

## Project Structure

The project folder contains the Terraform configuration and evidence collected during the exercise.

```
day17-azure-iam-remote-state/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── backend.tf
└── README.md
```

The exact files may vary according to the configuration used locally. Terraform's generated working directory and provider lock file may also be present.

### File Purposes

- `main.tf` — Defines the Terraform resources.
- `providers.tf` — Configures the required Terraform provider.
- `variables.tf` — Declares input variables.
- `outputs.tf` — Defines useful output values.
- `backend.tf` — Configures remote state storage, if maintained separately.

## Step 1: Verify Azure Subscription and Tenant

I checked the active Azure account and subscription before working with Terraform.

```
az account show
```

This command displays information about the currently selected Azure subscription and tenant.

Verifying the active subscription helps ensure that Terraform operations use the intended Azure environment.

## Step 2: Initialise the Terraform Provider

I initialised the Terraform project and downloaded the required provider dependencies.

```
terraform init
```

The project evidence includes the Azure identity/provider setup and successful provider initialisation.

## Step 3: Validate the Terraform Configuration

I checked the Terraform configuration for syntax and configuration errors.

```
terraform fmt
terraform validate
```

The validation screenshot records a successful Terraform validation.

Validation does not guarantee that Azure will authorise every operation or that all resources can be deployed.

## Step 4: Review the Terraform Plan

I generated and reviewed the Terraform execution plan.

```
terraform plan
```

The plan helps identify the infrastructure changes Terraform proposes before applying them.

Reviewing the plan is important when working with cloud infrastructure because unintended changes can affect existing resources.

## Step 5: Configure AzureRM Remote State

I configured Terraform to use an AzureRM backend for remote state storage.

Remote state allows Terraform's state file to be stored in Azure Blob Storage rather than only on the local computer.

The project evidence includes the configured backend and the remote-state setup.

## Step 6: Verify the State Blob

I verified the Terraform state blob in Azure Storage.

Storing state remotely helps centralise the state used by the Terraform project. Restrict access to the storage account and state container to authorised identities.

Terraform state can contain sensitive infrastructure information, so its storage and access permissions must be managed carefully.

## Step 7: Observe Terraform State Locking

I captured evidence of Terraform acquiring and releasing a state lock.

State locking helps prevent multiple Terraform operations from modifying the same state simultaneously when the backend supports locking.

This is especially important when infrastructure is managed by multiple users or automation processes.

## Step 8: Investigate the Identity Permission Error

During an identity-related operation, Azure returned an identity permission error.

The error demonstrated an important distinction:

- **Authentication** verifies the identity attempting an operation.
- ** Authorisation ** determines whether that identity has permission to operate.

Azure directory operations can require specific Microsoft Entra ID directory roles or permissions. A user who can access Azure resources may still lack permission to list or manage directory objects.

The error was documented as part of the troubleshooting evidence rather than treated as proof that all identity and access management work had failed.

## Key Learnings

1. Terraform providers enable infrastructure management across cloud platforms.
2. Azure subscription and tenant verification help prevent working in the wrong environment.
3. Remote state stores Terraform state centrally in Azure Storage.
4. State locking helps protect against concurrent state modifications.
5. Authentication and authorisation are separate security concepts.
6. Azure resource permissions and Microsoft Entra ID directory permissions are not interchangeable.
7. Terraform validation, planning, and applying have different purposes.

## Screenshot Evidence

The Day 17 evidence ZIP contains the following screenshots:

1. `Screenshot Day17-01-Project-Folder.png`
2. `Screenshot Day17-02-AzureAD-Provider-Init.png`
3. `Screenshot Day17-03-Azure-Subscription-Tenant.png`
4. `Screenshot Day17-04-Terraform-Validate.png`
5. `Screenshot Day17-05-Terraform-Plan.png`
6. `Screenshot Day17-06-Terraform-Apply-Identity-Permission-Error.png`
7. `Screenshot Day17-07-Terraform-AzureRM-Backend-Configured.png`
8. `Screenshot Day17-08-Terraform-Remote-State.png`
9. `Screenshot Day17-09-Terraform-State-Blob.png`
10. `Screenshot Day17-10-Terraform-State-Locking.png`

## Security Notes

- Restrict access to the Azure Storage Account and Terraform state container.
- Never commit credentials, access tokens, or sensitive state files to a public repository.
- Review Terraform plans before applying changes.
- Assign only the permissions needed for the intended Azure operation.
- Record permission errors accurately and investigate them before making access changes.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 17 of 23

**Project:** Azure IAM, Remote State, and State Locking

**Status:** Completed — remote state and state-locking evidence captured; the identity-permission limitation was documented.
