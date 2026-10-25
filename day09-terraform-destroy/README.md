# Day 9: Terraform Destroy

## Overview

Day 9 focused on understanding the `terraform destroy` command and how Terraform removes infrastructure managed through its state.

I used Terraform to destroy the Azure Resource Group created during the earlier exercises. After reviewing the proposed destruction, I confirmed the operation. Terraform completed successfully and reported that one resource had been destroyed.

This exercise demonstrates the importance of reviewing destructive operations and understanding how Terraform manages infrastructure throughout its lifecycle.

## Objectives

- Understand the purpose of `terraform destroy`.
- Review the resources Terraform plans to remove.
- Confirm a destructive operation.
- Observe Terraform's destruction progress.
- Verify the destroy completion message.
- Understand the relationship between Terraform state and managed infrastructure.
- Recognise the risks associated with deleting cloud resources.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure lifecycle management |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure CLI | Verify Azure infrastructure |
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

The exact files may vary as the project develops.

## Step 1: Verify the Existing Resource Group

Before destruction, I verified the Azure Resource Group using Azure CLI:

```
az group show --name terraform-day3-rg
```

The command returned the Resource Group details, including its name, location, resource ID, and provisioning state.

The output showed:

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

This confirmed that the Resource Group existed before the destruction exercise.

## Step 2: Run Terraform Destroy

From the Terraform project directory, I executed:

```
terraform destroy
```

Terraform refreshed the current state and generated a destruction plan.

The plan showed that the managed Azure Resource Group would be destroyed. Terraform also displayed the output values that would become unavailable after the resource was removed.

## Step 3: Review and Confirm the Destruction

Terraform requested explicit confirmation before proceeding.

After reviewing the proposed action, I entered:

```
yes
```

**Important:** `terraform destroy` is a destructive operation. Always review the plan and confirm that the resources selected for deletion are safe to remove. Deleting a Resource Group can also delete the resources contained within it.

## Step 4: Observe the Destruction Process

Terraform displayed progress messages while Azure processed the deletion.

The operation eventually completed with:

```
Destroy complete! Resources: 1 destroyed.
```

This confirms that Terraform successfully destroyed one managed resource during the operation.

## Step 5: Understand the Result

The destroy operation removed the Azure Resource Group managed by this configuration.

The Terraform output values were also cleared because the corresponding resource no longer existed in the managed configuration state.

The successful destroy operation demonstrates how Terraform can create, update, and remove infrastructure through declarative configuration and state management.

## Day 9 Results

- [x] Existing Azure Resource Group verified.
- [x] `terraform destroy` executed.
- [x] Proposed destruction plan reviewed.
- [x] Destructive operation explicitly confirmed.
- [x] Terraform destruction progress observed.
- [x] Destroy operation completed successfully.
- [x] Terraform reported 1 resource destroyed.
- [x] Resource lifecycle and deletion risks reviewed.

## Screenshot Evidence

1. `Screenshot Terraform-Day9-01-Destroy-Complete.png` — Shows the Terraform destruction plan, confirmation, and successful completion.
2. `Screenshot Terraform-Day9-02-Azure-Verification-Before-Destroy.png` — Shows the Azure Resource Group and its successful provisioning state before destruction.

The second screenshot is evidence of the Resource Group's pre-destruction state; it does not independently verify its absence after destruction.

## Key Learnings

1. `terraform destroy` removes infrastructure managed by the current Terraform configuration and state.
2. Terraform displays a destruction plan before requesting confirmation.
3. Destructive operations should be reviewed carefully before approval.
4. Deleting an Azure Resource Group can delete its contained resources.
5. Terraform's state and outputs change as managed resources are removed.
6. Terraform supports the infrastructure lifecycle: creation, modification, and destruction.

## Security and Best Practices

- Never run destructive commands against production infrastructure without authorisation and a reviewed plan.
- Check the active Azure subscription and Terraform working directory before deleting resources.
- Confirm that the targeted resources are safe to remove.
- Avoid storing credentials or sensitive information in Git.
- Retain appropriate backups and recovery plans for important infrastructure.
- Use separate environments for practice and production workloads.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 9 of 23  
**Status:** Completed
