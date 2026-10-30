# Day 12: Azure Blob Versioning and Data Recovery

## Overview

Day 12 focused on protecting Azure Blob Storage data against accidental changes and deletions using Terraform and Azure Storage data protection features.

I configured an Azure Storage Account with Blob versioning enabled and a seven-day deletion-retention policy. I then uploaded and modified a test blob to generate multiple versions and practised recovering an earlier version of its content.

This exercise demonstrates how blob versioning and retention settings can support data recovery and help reduce the impact of accidental changes.

## Objectives

- Configure Azure Storage using Terraform.
- Enable Blob versioning.
- Configure Blob deletion retention.
- Configure container deletion retention.
- Deploy the storage configuration.
- Verify data protection settings in the Azure Portal.
- Create and inspect multiple blob versions.
- Recover an earlier version of blob content.
- Understand the difference between versioning and retention.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Azure Storage | Store blob data |
| Azure Portal | Verify storage settings and blob versions |
| PowerShell | Execute Terraform commands |
| HCL | Define infrastructure configuration |

## Project Structure

```
day12-azure-blob-versioning/
├── main.tf
├── providers.tf
├── .terraform.lock.hcl
├── .terraform/
├── README.md
└── screenshots/
```

The exact files may vary depending on how the project is organised locally.

## Step 1: Create the Azure Resource Group

I defined an Azure Resource Group in Terraform:

```
resource "azurerm_resource_group" "day12" {
  name     = "terraform-day12-rg"
  location = "eastus"
}
```

The Resource Group provides a logical container for the resources used in this exercise.

## Step 2: Configure the Storage Account

I configured an Azure Storage Account with the following settings:

```
resource "azurerm_storage_account" "versioned" {
  name                     = "your_unique_storage_name"
  resource_group_name      = azurerm_resource_group.day12.name
  location                 = azurerm_resource_group.day12.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  infrastructure_encryption_enabled = true
  min_tls_version                   = "TLS1_2"
  https_traffic_only_enabled        = true

  blob_properties {
    versioning_enabled = true

    delete_retention_policy {
      days = 7
    }

    container_delete_retention_policy {
      days = 7
    }
  }
}
```

The Storage Account name in this example is a placeholder. Use the actual unique name from the local Terraform configuration when documenting the project.

## Step 3: Enable Blob Versioning

The configuration includes:

```
blob_properties {
  versioning_enabled = true
}
```

Blob versioning automatically maintains previous versions of a blob when supported changes are made to it.

This allows an earlier version to be retrieved if the current content is accidentally overwritten or modified.

## Step 4: Configure Deletion Retention

I configured seven-day retention periods for deleted blobs and containers:

```
delete_retention_policy {
  days = 7
}

container_delete_retention_policy {
  days = 7
}
```

These settings provide a recovery window for supported deletion scenarios.

Versioning and deletion retention are complementary protections. Versioning preserves earlier blob versions, while retention settings help recover supported deleted data or containers during the configured retention period.

## Step 5: Format and Initialise Terraform

I formatted the Terraform configuration:

```
terraform fmt
```

Then I initialised the working directory:

```
terraform init
```

Terraform downloaded the required provider and initialised the project.

## Step 6: Review and Apply the Terraform Plan

I generated the execution plan:

```
terraform plan
```

After reviewing the proposed resource changes, I applied the configuration:

```
terraform apply
```

I confirmed the operation when prompted. Terraform completed successfully, creating the configured Azure resources.

## Step 7: Verify Blob Versioning

I checked the Storage Account data protection settings in the Azure Portal.

The verification included:

- Blob versioning enabled.
- Blob deletion retention configured for seven days.
- Container deletion retention configured for seven days.

The portal screenshots provide evidence of the configured data protection settings.

## Step 8: Create Multiple Blob Versions

I used a recovery-test container and a test blob named `important-data.txt`.

I created and modified the blob content to produce multiple versions. The Azure Portal showed three versions of the blob.

The test demonstrated that versioning preserved earlier content rather than leaving only the most recent version available.

## Step 9: Recover Earlier Blob Content

I inspected the available blob versions and performed a recovery test using the earlier content.

The recovered content was:

```
Version 1 - Original Cloud Security Configuration
```

This confirmed that the original test content could be retrieved from a previous version.

**Important:** Blob versioning provides a recovery mechanism, but it is not a complete backup strategy. Separate backups, access controls, monitoring, and recovery testing may still be necessary.

## Day 12 Results

- [x] Azure Resource Group configured.
- [x] Azure Storage Account configured.
- [x] Blob versioning enabled.
- [x] Blob deletion retention set to seven days.
- [x] Container deletion retention set to seven days.
- [x] Terraform formatting and initialisation completed.
- [x] Terraform plan reviewed and applied.
- [x] Azure Portal data protection settings verified.
- [x] Multiple blob versions created and inspected.
- [x] Earlier blob content successfully recovered.

## Screenshot Evidence

1. `Screenshot Terraform-Day12-01-Project-Folder.png` — Existing Terraform project files.
2. `Screenshot Terraform-Day12-02-Day12-Folder.png` — Day 12 project directory.
3. `Screenshot Terraform-Day12-03-main.tf.png` — Storage Account, versioning, and retention configuration.
4. `Screenshot Terraform-Day12-04-Storage-Account-Name-Check.png` — Storage Account name check.
5. `Screenshot Terraform-Day12-05-Terraform-Fmt.png` — Terraform formatting.
6. `Screenshot Terraform-Day12-06-Terraform-Init.png` — Terraform initialisation.
7. `Screenshot Terraform-Day12-07-Terraform-Plan.png` — Proposed infrastructure changes.
8. `Screenshot Terraform-Day12-08-Terraform-Apply.png` — Successful deployment.
9. `Screenshot Terraform-Day12-09-Azure-Versioning-Verification.png` — Azure CLI verification.
10. `Screenshot Terraform-Day12-10-Azure-Portal-Data-Protection.png` — Storage data protection settings.
11. `Screenshot Terraform-Day12-11-Recovery-Test-Container.png` — Recovery test container.
12. `Screenshot Terraform-Day12-12-Version-1-Blob.png` — Original blob version.
13. `Screenshot Terraform-Day12-13-Version-2-Blob.png` — Second blob version.
14. `Screenshot Terraform-Day12-14-Version-3-Blob.png` — Third blob version.
15. `Screenshot Terraform-Day12-15-Blob-Versions.png` — Available blob versions.
16. `Screenshot Terraform-Day12-16-Blob-Recovery.png` — Blob recovery details.
17. `Screenshot Terraform-Day12-17-Recovered-Blob-Content.txt` — Recovered original blob content.

## Key Learnings

1. Blob versioning preserves earlier versions of blob data.
2. Retention settings help recover supported deleted data.
3. Terraform makes storage protection settings repeatable and reviewable.
4. The Azure Portal can be used to inspect blob versions and data protection settings.
5. Recovery testing verifies that stored versions are useful in practice.
6. Versioning and retention reduce recovery risk but do not replace a complete backup and disaster recovery strategy.

## Security Best Practices

- Enable Blob versioning for important data when appropriate.
- Configure retention periods based on recovery and compliance requirements.
- Restrict storage access using least-privilege permissions.
- Require HTTPS and use a secure minimum TLS version.
- Monitor storage access and changes.
- Protect Terraform state and never commit credentials or secrets to GitHub.
- Periodically test recovery procedures.

## Challenge: 22-Day Terraform Learning Journey

**Day:** 12 of 22  
**Status:** Completed
