# Day 10: Terraform Validation, Variables, and Sensitive Data

## Overview

Day 10 focused on improving Terraform configuration quality and learning how to handle sensitive values.

I used `terraform fmt` to format the configuration, `terraform validate` to check its validity, and `terraform plan` to review the proposed changes without deploying infrastructure. I also practiced using a `.tfvars` file and marking a variable and output as sensitive.

This exercise reinforced the importance of configuration validation, clear variable definitions, and careful handling of sensitive information in infrastructure-as-code projects.

## Objectives

- Format Terraform configuration files.
- Validate Terraform configuration.
- Understand the purpose of `terraform.tfvars`.
- Define and use Terraform input variables.
- Mark sensitive variables and outputs.
- Review a Terraform plan without applying changes.
- Understand warnings about undeclared variables.
- Clean up a temporary environment variable.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure-as-code |
| Microsoft Azure | Cloud platform used in earlier exercises |
| HCL | Terraform configuration language |
| PowerShell | Run Terraform commands and manage environment variables |
| VS Code | Edit Terraform configuration files |

## Project Structure

```
terraform-azure-cloud-security/
├── data.tf
├── main.tf
├── terraform.tfvars
├── .terraform.lock.hcl
├── .terraform/
└── README.md
```

The exact files may vary depending on the configuration used during the exercise.

## Step 1: Verify the Working Directory

I checked the current working directory in PowerShell:

```
pwd
```

This confirmed that the commands were being executed from the Terraform project directory.

## Step 2: Format the Configuration

I ran:

```
terraform fmt
```

This formats Terraform configuration files according to Terraform's standard formatting conventions.

Consistent formatting makes configuration easier to read, review, and maintain.

## Step 3: Validate the Configuration

I ran:

```
terraform validate
```

The command returned:

```
Success! The configuration is valid.
```

This confirmed that Terraform could parse and validate the configuration.

**Important:** Successful validation does not guarantee that a deployment will succeed. Azure permissions, provider configuration, resource constraints, and other runtime conditions can still affect deployment.

## Step 4: Use a Terraform Variables File

I used `terraform.tfvars` to supply input values separately from the main configuration.

Example:

```
resource_group_name = "terraform-day10-rg"
location            = "eastus"
```

Terraform can automatically load values from a file named `terraform.tfvars`.

Input variables should be declared in the Terraform configuration with matching names and appropriate types.

## Step 5: Define a Sensitive Variable and Output

The configuration includes a sensitive input variable and a corresponding sensitive output.

Example:

```
variable "admin_password" {
  type      = string
  sensitive = true
}

output "admin_password" {
  value     = var.admin_password
  sensitive = true
}
```

The `sensitive = true` setting instructs Terraform to redact the value in many command-line displays.

This setting does **not** encrypt the value or guarantee that it will be absent from Terraform state or other stored files.

For real projects, avoid placing passwords directly in committed `.tfvars` files. Prefer a suitable secret-management solution or securely supplied environment variables, and restrict access to Terraform state.

## Step 6: Validate the Sensitive Variable Configuration

I supplied a temporary environment variable in PowerShell and ran validation again.

```
$env:TF_VAR_admin_password = Read-Host "Enter a temporary test value"
terraform validate
```

The configuration validated successfully.

Terraform can use environment variables with the `TF_VAR_` prefix to provide input variable values.

For example, `TF_VAR_admin_password` supplies a value for the Terraform variable named `admin_password`.

## Step 7: Review the Terraform Plan

I ran:

```
terraform plan
```

The plan showed a sensitive output value without deploying new infrastructure.

The output also contained warnings about `resource_group_name` and `location` being present in `terraform.tfvars` without matching declared root input variables.

These warnings highlighted the importance of keeping variable declarations and variable values consistent.

To resolve such warnings in a configuration where these values are intended inputs, declare the corresponding variables with appropriate types and descriptions. If they are not needed, remove the unused values from the variables file.

## Step 8: Clean Up the Temporary Environment Variable

After the exercise, I removed the temporary environment variable from the current PowerShell session:

```
Remove-Item Env:TF_VAR_admin_password
```

This prevents the value from remaining available through that environment variable in the current session.

Removing the variable does not erase copies that may already exist in terminal history, logs, state files, or other locations.

## Day 10 Results

- [x] Terraform formatting command executed.
- [x] Terraform configuration validated successfully.
- [x] `terraform.tfvars` reviewed.
- [x] Sensitive input variable and output configured.
- [x] Terraform plan reviewed without applying changes.
- [x] Undeclared-variable warnings identified.
- [x] Temporary environment variable cleanup performed.

## Screenshot Evidence

1. `Screenshot Terraform-Day10-01-Terraform-Fmt.png` — Terraform formatting command.
2. `Screenshot Terraform-Day10-02-Terraform-Validate.png` — Successful configuration validation.
3. `Screenshot Terraform-Day10-03-Terraform-tfvars.png` — Variables file.
4. `Screenshot Terraform-Day10-04-Terraform-main.tf.png` — Sensitive variable and output configuration.
5. `Screenshot Terraform-Day10-05-Terraform-Sensitive-Variable-Validate.png` — Validation and sensitive variable testing.
6. `Screenshot Terraform-Day10-06-Terraform-Sensitive-Variable-Plan.png` — Plan output and undeclared-variable warnings.
7. `Screenshot Terraform-Day10-07-Terraform-temporarily-set-cleanup.png` — Temporary environment variable cleanup.

## Key Learnings

1. `terraform fmt` improves consistency and readability.
2. `terraform validate` checks Terraform configuration syntax and internal consistency.
3. `terraform.tfvars` separates input values from resource configuration.
4. Input variables must be declared when their values are supplied through a variables file.
5. Terraform's `sensitive` setting helps reduce accidental display of sensitive values but does not replace secure storage.
6. `terraform plan` allows changes to be reviewed before applying them.
7. Temporary environment variables should be cleaned up when no longer needed.

## Security Best Practices

- Never commit real passwords, tokens, or credentials to a public GitHub repository.
- Do not treat Terraform's `sensitive` flag as encryption.
- Protect Terraform state and restrict access to sensitive files.
- Use secret-management services for production credentials.
- Review Terraform warnings and plans before making infrastructure changes.
- Remove temporary credentials and environment variables after testing.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 10 of 23  
**Status:** Completed
