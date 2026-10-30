# Day 14: Azure RBAC and Least-Privilege Access

## Overview

Day 14 focused on implementing role-based access control (RBAC) in Microsoft Azure using Terraform.

I configured an Azure Storage Account access assignment using the built-in **Storage Blob Data Reader** role. This role provides read access to blob data without granting the broader permissions associated with storage administration.

I used Terraform to format, initialise, validate, plan, and apply the configuration. After deployment, I verified the role assignment using Azure CLI and the Azure Portal.

This exercise demonstrates how least-privilege access can be implemented and verified through infrastructure-as-code.

## Objectives

- Understand Azure role-based access control (RBAC).
- Learn the principle of least privilege.
- Configure a Storage Blob Data Reader role assignment.
- Define the role assignment using Terraform.
- Format and validate Terraform configuration.
- Review and apply the execution plan.
- Verify the assigned role using Azure CLI.
- Confirm the role assignment in the Azure Portal.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure RBAC | Control access to Azure resources |
| Azure CLI | Inspect role assignments |
| Azure Portal | Verify access configuration |
| PowerShell | Execute Terraform and Azure CLI commands |
| HCL | Define infrastructure configuration |

## Project Structure

```
day14-azure-minimal-rbac/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

The exact files may vary depending on the local configuration.

## Step 1: Understand Azure RBAC

Azure RBAC determines who can access Azure resources and which actions they can perform.

A role assignment consists of three main elements:

- **Security principal:** The user, group, service principal, or managed identity receiving access.
- **Role definition:** The permissions granted, such as reading blob data.
- **Scope:** The Azure resource or resource hierarchy where the permissions apply.

Assigning a role at a narrower scope helps limit unnecessary access.

## Step 2: Configure the Storage Blob Data Reader Role

The project used the built-in **Storage Blob Data Reader** role to grant read access to blob data.

An Azure role assignment can be defined in Terraform as follows:

```
resource "azurerm_role_assignment" "blob_reader" {
  scope                = azurerm_storage_account.secure.id
  role_definition_name = "Storage Blob Data Reader"
  principal_id         = var.principal_id
}
```

This example assumes that the Storage Account resource and `principal_id` input variable are declared elsewhere in the configuration.

The principal ID must identify the intended user, group, service principal, or managed identity.

The scope shown is the Storage Account. If access is only required to a particular container, consider whether a narrower supported scope is appropriate.

## Step 3: Understand Least Privilege

The Storage Blob Data Reader role grants read access to blob data without granting general write or delete permissions to that data.

This is preferable to assigning a broad administrative role when the principal only needs to read storage content.

However, effective permissions depend on all applicable role assignments, including inherited roles and other access mechanisms. A role assignment alone does not prove that the principal has no additional permissions.

## Step 4: Format and Initialise Terraform

I formatted the configuration:

```
terraform fmt
```

Then I initialised the working directory:

```
terraform init
```

Terraform successfully initialised the project using the AzureRM provider.

## Step 5: Validate the Configuration

I ran:

```
terraform validate
```

The command returned:

```
Success! The configuration is valid.
```

This confirmed that Terraform could parse and validate the configuration.

Validation does not independently confirm that the Azure identity has permission to create the role assignment.

## Step 6: Review the Terraform Plan

I generated the execution plan:

```
terraform plan
```

I reviewed the planned changes before applying them.

Role assignments affect access permissions, so it is important to confirm the intended principal, role, and scope before proceeding.

## Step 7: Apply the Configuration

I applied the configuration:

```
terraform apply
```

After reviewing the proposed changes, I confirmed the operation.

Terraform completed the deployment and created the configured infrastructure and role assignment.

## Step 8: Verify the Role Assignment Using Azure CLI

I used Azure CLI to inspect the role assignment at the configured scope:

```
az role assignment list `
  --scope "$(terraform output -raw rbac_scope)" `
  --query "[].{Role:roleDefinitionName, PrincipalId:principalId,Scope:scope}" `
  --output table
```

The command output showed the **Storage Blob Data Reader** role and its associated principal and scope.

This provided command-line evidence that the role assignment existed.

## Step 9: Verify the Azure Portal Configuration

I opened the relevant Azure resource in the Azure Portal and inspected its Access Control (IAM) role assignments.

The screenshot showed the assigned role and principal information, providing additional visual evidence of the RBAC configuration.

## Day 14 Results

- [x] Azure RBAC and least-privilege concepts reviewed.
- [x] Storage Blob Data Reader role assignment configured.
- [x] Terraform configuration formatted.
- [x] Terraform initialised successfully.
- [x] Terraform validation passed.
- [x] Terraform plan reviewed.
- [x] Terraform configuration applied.
- [x] Role assignment verified using Azure CLI.
- [x] Azure Portal RBAC configuration inspected.

## Screenshot Evidence

1. `Screenshot Terraform-Day14-01-Project-Folder.png` — Day 14 project directory.
2. `Screenshot Terraform-Day14-02-Day14-main.tf.png` — Terraform configuration for Azure resources and RBAC.
3. `Screenshot Terraform-Day14-03-Terraform-Fmt.png` — Terraform formatting.
4. `Screenshot Terraform-Day14-04-Terraform-Init.png` — Successful initialisation.
5. `Screenshot Terraform-Day14-05-Terraform-Validate.png` — Successful validation.
6. `Screenshot Terraform-Day14-06-Terraform-Plan.png` — Proposed infrastructure and access changes.
7. `Screenshot Terraform-Day14-07-Terraform-Apply.png` — Terraform deployment results.
8. `Screenshot Terraform-Day14-08-Azure-RBAC-Role-Verification.png` — Azure CLI verification of the Storage Blob Data Reader role assignment.
9. `Screenshot Terraform-Day14-09-Azure-Portal-RBAC.png` — Azure Portal access-control configuration.

## Key Learnings

1. Azure RBAC controls access through role assignments.
2. A role assignment connects a principal, role definition, and scope.
3. Storage Blob Data Reader provides read access to blob data.
4. Least privilege means granting only the permissions required for a task.
5. Narrower scopes can reduce unnecessary access.
6. Terraform can manage role assignments as code.
7. Azure CLI and the Azure Portal can help verify access configuration.
8. Effective access must be evaluated across all applicable permissions and role assignments.

## Security Best Practices

- Avoid broad administrative roles when a narrower role is sufficient.
- Assign permissions at the narrowest practical scope.
- Verify the intended principal before creating a role assignment.
- Review access changes before applying Terraform plans.
- Audit existing role assignments for excessive permissions.
- Protect credentials, Terraform state, and sensitive configuration values.
- Remove access when it is no longer required.

## Challenge: 22-Day Terraform Learning Journey

**Day:** 14 of 22  
**Status:** Completed
