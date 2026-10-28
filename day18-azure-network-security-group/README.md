# Day 18: Azure Network Security Group Using Terraform

## Overview

Day 18 focuses on creating and configuring an Azure Network Security Group (NSG) using Terraform.

A Network Security Group controls inbound and outbound network traffic through security rules. In this project, I used Terraform to define an NSG, configure rules for SSH and HTTP traffic, and associate the NSG with an Azure subnet.

The project demonstrates how infrastructure as code can be used to implement and manage basic network security controls in Microsoft Azure.

## Objectives

- Understand Azure Network Security Groups.
- Define an NSG using Terraform.
- Configure inbound security rules for SSH and HTTP.
- Understand the security implications of allowing network ports.
- Initialise and validate the Terraform configuration.
- Review the Terraform execution plan.
- Apply the configuration.
- Verify the NSG and its rules in Azure.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Azure resource management |
| Azure Network Security Group | Network traffic filtering |
| HCL | Terraform configuration language |
| PowerShell | Execute Terraform commands |

## Project Structure

```
day18-azure-network-security-group/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
└── README.md
```

The exact files may vary according to the configuration used in the project.

### File Purposes

- `main.tf` — Defines the Azure network resources and NSG rules.
- `providers.tf` — Configures Terraform and the AzureRM provider.
- `variables.tf` — Declares configurable input values, if used.
- `outputs.tf` — Exposes useful resource information, if configured.
- `README.md` — Documents the project, implementation, and results.

## Step 1: Configure the NSG

I defined an Azure Network Security Group in Terraform and configured inbound security rules for the required traffic.

The project focuses on these ports:

| Port | Protocol | Purpose |
|---|---|---|
| 22 | TCP | SSH administration |
| 80 | TCP | HTTP web traffic |

The NSG is associated with the relevant Azure subnet so that its security rules can be applied to network traffic for resources in that subnet.

## Step 2: Initialise Terraform

I initialised the Terraform working directory to download the required provider and prepare the project.

```powershell
terraform init
```

Successful initialisation confirms that Terraform has prepared the working directory and installed the required provider dependencies.

## Step 3: Validate the Configuration

I checked the Terraform configuration for syntax and configuration errors.

```
terraform fmt
terraform validate
```

Formatting improves consistency, while validation checks the configuration structure. Neither command independently confirms that Azure will authorize every operation.

## Step 4: Review the Terraform Plan

I generated the execution plan before deploying the NSG.

```
terraform plan
```

The plan provides a preview of the resources and changes Terraform intends to make.

Reviewing this output before applying changes helps identify unexpected infrastructure modifications.

## Step 5: Apply the Configuration

After reviewing the execution plan, I applied the Terraform configuration.

```
terraform apply
```

I reviewed the deployment output and captured evidence of the NSG application.

## Step 6: Verify the Inbound Security Rules

I inspected the configured inbound rules in Azure.

The rules demonstrate how network access can be controlled by specifying the protocol, port, source, and access decision.

The source scope is important: SSH access should generally be restricted to a trusted IP address or approved network rather than exposed to the entire internet.

HTTP access may need to be publicly reachable for a public website, depending on the intended architecture.

## Security Considerations

- Avoid exposing SSH to `0.0.0.0/0` unless there is a specific, justified requirement.
- Permit only the ports required by the workload.
- Use narrow source address ranges where practical.
- Associate the NSG with the intended subnet or network interface.
- Review rule priorities and ensure that the intended traffic is allowed or denied.
- Revisit security rules as application requirements change.

## Screenshot Evidence

The Day 18 evidence ZIP contains:

1. `Screenshot Terraform-Day18-01-Project-Folder.png` — Project folder.
2. `Screenshot Terraform-Day18-02-Terraform-Init-Validate.png` — Terraform initialisation and validation.
3. `Screenshot Terraform-Day18-03-Terraform-Plan-NSG.png` — Terraform execution plan.
4. `Screenshot Terraform-Day18-04-Terraform-Apply-NSG.png` — Applying the NSG configuration.
5. `Screenshot Terraform-Day18-05-Azure-NSG-Inbound-Rules.png` — Azure inbound security rules.
6. `Screenshot Terraform-Day18-06-Security-Rules-Understanding.png` — Security-rule concepts.

## Key Learnings

1. Azure NSGs filter network traffic using security rules.
2. Terraform allows network security controls to be defined as code.
3. SSH and HTTP serve different purposes and should have appropriate access scopes.
4. Terraform initialisation, validation, planning, and applying are separate steps.
5. Reviewing the execution plan helps prevent unintended changes.
6. Network security requires limiting access to only what the workload needs.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 18 of 23

**Project:** Project 3 — Azure Network Security Group

**Status:** Completed
