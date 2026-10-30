# Day 16: Terraform Modules and Security Scanning with Checkov

## Overview

Day 16 focuses on organising Terraform configurations into reusable modules and scanning infrastructure-as-code for potential security misconfigurations using Checkov.

The project uses an Azure Storage Account module to demonstrate modular Terraform design. I organised the configuration into separate files for resources, variables, and outputs, then installed Checkov and scanned the Terraform configuration.

The initial security scan identified findings that required review and remediation. After making security improvements, I ran Checkov again to verify the final results.

## Objectives

- Understand Terraform modules and reusable infrastructure components.
- Create a Terraform module for an Azure Storage Account.
- Separate resource definitions, input variables, and outputs.
- Initialise and validate Terraform configurations.
- Install and run Checkov.
- Review security findings in Terraform code.
- Remediate identified security issues.
- Perform a final security scan.
- Record the results as project evidence.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Manage Azure resources |
| Checkov | Terraform security scanning |
| HCL | Terraform configuration language |
| PowerShell | Run commands and inspect results |
| Git | Track local project changes |

## Project Structure

The project separates the root Terraform configuration from the reusable Storage Account module.

```
day16-terraform-modules-checkov/
├── main.tf
├── variables.tf
├── outputs.tf
├── modules/
│   └── storage-account/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── checkov-results.json
└── README.md
```

The exact local structure may contain additional Terraform files or generated working files.

### File Purposes

- `main.tf` — Defines the root configuration and uses the module.
- `variables.tf` — Declares input variables.
- `outputs.tf` — Exposes selected resource information.
- `modules/storage-account/` — Contains the reusable Storage Account module.
- `checkov-results.json` — Stores security scan results, if generated and retained.

## Step 1: Create the Terraform Module

I organised the Storage Account configuration into a dedicated module directory:

```
modules/storage-account/
```

The module separates resource definitions, inputs, and outputs into their own files.

Terraform modules improve reusability and make infrastructure code easier to organise, maintain, and review.

## Step 2: Configure the Root Terraform Project

I configured the root Terraform files to supply the required values and call the Storage Account module.

This structure separates the configuration that uses the module from the implementation inside the module itself.

## Step 3: Validate the Module

I formatted and validated the Terraform configuration.

```
terraform fmt
terraform init
terraform validate
```

These commands help ensure consistent formatting, initialise provider and module dependencies, and check configuration validity.

## Step 4: Install Checkov

I installed Checkov to scan the Terraform configuration for potential security misconfigurations.

Checkov examines infrastructure-as-code against security policies and reports findings that require review.

## Step 5: Run the Initial Security Scan

I ran the initial Checkov scan against the Terraform project.

```
checkov -d .
```

The initial scan reported **11 failed checks and 4 passed checks**.

I reviewed the findings to understand which configuration settings needed attention before proceeding with remediation.

## Step 6: Review and Remediate Security Findings

I reviewed the reported findings and improved the Terraform configuration to address the identified security issues.

This step helped me understand how infrastructure-as-code can be assessed before deployment and how security controls can be incorporated directly into Terraform configurations.

Security findings should be reviewed individually rather than fixed blindly. The appropriate remediation depends on the resource, the finding, and the project's requirements.

## Step 7: Run the Final Checkov Scan

After making the changes, I ran Checkov again to verify the updated configuration.

Final scan results:

| Result | Count |
|---|---:|
| Passed checks | 30 |
| Failed checks | 0 |
| Skipped checks | 0 |

The final scan showed that all checks reported in this run passed, with no failed or skipped checks.

These results demonstrate the improvement between the initial and final scans. They do not guarantee that every possible security risk has been eliminated.

## Step 8: Validate the Updated Terraform Configuration

I ran Terraform validation after the security improvements.

```
terraform fmt
terraform validate
terraform plan
```

The configuration validated successfully, and the plan was reviewed to inspect the proposed infrastructure changes.

## Security Scan Comparison

| Metric | Initial scan | Final scan |
|---|---:|---:|
| Passed checks | 4 | 30 |
| Failed checks | 11 | 0 |
| Skipped checks | 0 | 0 |

The scan results show that the initial findings were addressed and the final Checkov run reported zero failed checks.

## Screenshot Evidence

The following files were included in the Day 16 evidence ZIP:

1. `Screenshot Terraform-Day16-01-Project-Folder.png` — Project directory.
2. `Screenshot Terraform-Day16-02-Terraform-Module-Structure.png` — Module structure.
3. `Screenshot Terraform-Day16-03-Terraform-main.tf.png` — Root Terraform configuration.
4. `Screenshot Terraform-Day16-04-Terraform-variables.tf.png` — Root input variables.
5. `Screenshot Terraform-Day16-05-Terraform-outputs.tf.png` — Root outputs.
6. `Screenshot Terraform-Day16-06-Terraform-variables.tf.png` — Module input variables.
7. `Screenshot Terraform-Day16-07-Terraform-main.tf.png` — Module resource configuration.
8. `Screenshot Terraform-Day16-08-Terraform-outputs.tf.png` — Module outputs.
9. `Screenshot Terraform-Day16-09-Terraform-Module-Validate.png` — Module validation.
10. `Screenshot Terraform-Day16-10-Terraform-Checkov-Installation.png` — Checkov installation.
11. `Screenshot Terraform-Day16-11-Terraform-Checkov-Initial-Scan.png` — Initial security scan.
12. `Screenshot Terraform-Day16-12-Terraform-Secure-Terraform-Validate.png` — Validation after security improvements.
13. `Screenshot Terraform-Day16-13-Terraform-Checkov-Final-Scan.png` — Final security scan.
14. `Screenshot Terraform-Day16-15-Terraform-Checkov-Report.png` — Checkov report.
15. `Screenshot Terraform-Day16-16-Git-Status.png` — Local Git status.
16. `Screenshot Terraform-Day16-17-Terraform-Plan.png` — Terraform plan evidence.

## Key Learnings

1. Terraform modules help create reusable infrastructure components.
2. Variables allow modules to accept configurable inputs.
3. Outputs expose useful information from Terraform resources.
4. Checkov can identify potential security misconfigurations before deployment.
5. Security findings need to be reviewed and remediated according to their context.
6. Re-running a security scan helps verify whether changes improved the configuration.
7. Terraform validation and security scanning serve different purposes and complement each other.

## Important Notes

- Checkov results represent the policies and configuration examined during the scan.
- A successful Terraform validation does not prove that infrastructure is secure.
- Review Terraform plans before applying infrastructure changes.
- Never commit credentials, secrets, or sensitive Terraform state to a public repository.
- Keep the screenshots and scan reports as evidence of the work performed.

## Challenge: 22-Day Terraform Learning Journey

**Day:** 16 of 22  
**Project:** Terraform Modules and Checkov Security Scanning  
**Status:** Completed
