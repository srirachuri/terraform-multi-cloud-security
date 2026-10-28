# Day 19: Azure Virtual Network and Routing with Terraform

## Overview

Day 19 focuses on creating an Azure Virtual Network (VNet), configuring subnets, and associating a route table using Terraform.

The project demonstrates how network infrastructure can be defined and deployed through infrastructure as code. It also explores how subnets divide a virtual network into smaller network segments and how route tables influence network traffic routing.

## Objectives

- Create an Azure Virtual Network using Terraform.
- Configure separate web and application subnets.
- Understand IPv4 CIDR blocks and subnet addressing.
- Create an Azure route table.
- Associate the route table with the intended subnet.
- Initialise and validate Terraform.
- Review the execution plan before deployment.
- Apply the configuration and verify the resources in Azure.

## Technologies Used

| Technology | Purpose |
|---|---|
| Terraform | Infrastructure as code |
| Microsoft Azure | Cloud platform |
| AzureRM Provider | Azure resource management |
| Azure Virtual Network | Private network infrastructure |
| Azure Subnets | Network segmentation |
| Azure Route Table | Custom network routing |
| PowerShell | Execute Terraform commands |

## Project Structure

```
day19-azure-vnet-routing/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
└── README.md
```

The exact files depend on the Terraform configuration used in the local project.

### File Purposes

- `main.tf` — Defines the VNet, subnets, route table, and associations.
- `providers.tf` — Configures Terraform and the AzureRM provider.
- `variables.tf` — Declares configurable input values, if used.
- `outputs.tf` — Exposes useful resource information, if configured.
- `README.md` — Documents the project and its results.

## Step 1: Configure the Virtual Network

I configured an Azure Virtual Network with the following address space:

| Network component | CIDR block |
|---|---|
| Virtual Network | `10.30.0.0/16` |
| Web subnet | `10.30.1.0/24` |
| Application subnet | `10.30.2.0/24` |

The VNet provides the network address space for the project. The web and application subnets divide that address space into separate network segments.

This design provides a foundation for separating workloads and applying network security controls.

## Step 2: Configure the Subnets

I defined two subnets inside the VNet:

- **Web subnet:** Intended for web-facing resources.
- **Application subnet:** Intended for application resources.

Subnet segmentation makes network architecture easier to organize and provides separate scopes for future security rules and routing configurations.

Creating separate subnets does not, by itself, guarantee isolation. Appropriate NSG rules and routing controls must also be configured.

## Step 3: Configure the Route Table

I created an Azure route table to demonstrate custom routing configuration.

A route table contains routes that determine where network traffic should be directed based on destination address prefixes and next-hop settings.

For this exercise, I configured the route table and associated it with the intended subnet.

**Azure networking note:** Azure provides system routes automatically, including the default Internet route where applicable. An Azure route table is not the same resource as an AWS Internet Gateway, and creating a route table alone does not create Internet connectivity.

## Step 4: Initialise and Validate Terraform

I initialised the Terraform working directory and checked the configuration.

```
terraform init
terraform fmt
terraform validate
```

- `terraform init` prepares the working directory and downloads provider dependencies.
- `terraform fmt` formats Terraform files consistently.
- `terraform validate` checks the configuration for syntax and configuration errors.

The Day 19 evidence includes a Terraform validation screenshot.

## Step 5: Review the Execution Plan

I generated the Terraform execution plan before deploying the network resources.

```
terraform plan
```

The plan provides a preview of the infrastructure changes Terraform intends to make.

Reviewing the plan helps identify unexpected additions, changes, or deletions before they affect the Azure environment.

## Step 6: Apply the Configuration

After reviewing the plan, I applied the Terraform configuration.

```
terraform apply
```

I captured the deployment output as evidence of the VNet configuration being applied.

## Step 7: Verify the Azure Resources

I inspected the deployed resources in Azure.

The evidence includes the VNet and subnet configuration, along with the route table configuration.

Verification helps confirm that the intended network components were created and that the route table was configured for the exercise.

## Security Considerations

- Use a clear IP addressing plan to prevent overlapping network ranges.
- Separate workloads into appropriate subnets.
- Apply NSGs to control allowed and denied network traffic.
- Review custom routes carefully before deploying them.
- Do not assume that subnet separation alone provides complete security isolation.
- Verify routing and connectivity requirements before exposing workloads.

## Screenshot Evidence

The Day 19 evidence ZIP contains:

1. `Screenshot Terraform-Day19-01-Project-Folder.png` — Project folder.
2. `Screenshot Terraform-Day19-02-Terraform-Validate.png` — Terraform validation.
3. `Screenshot Terraform-Day19-03-Terraform-Plan.png` — Terraform execution plan.
4. `Screenshot Terraform-Day19-04-Terraform-Apply-VNet.png` — Applying the VNet configuration.
5. `Screenshot Terraform-Day19-05-Azure-VNet-Subnets.png` — Azure VNet and subnet configuration.
6. `Screenshot Terraform-Day19-06-Azure-Route-Table.png` — Azure route table configuration.

## Key Learnings

1. Azure VNets provide private network address spaces for cloud resources.
2. CIDR notation defines network and subnet address ranges.
3. Subnets divide a virtual network into smaller logical segments.
4. Azure route tables configure custom routes for network traffic.
5. Terraform supports repeatable network infrastructure deployment.
6. Validation and planning should be completed before applying changes.
7. Azure and AWS have different networking resources and routing behaviour.

## Challenge: 23-Day Terraform Learning Journey

**Day:** 19 of 23

**Project:** Project 4 — Azure Virtual Network and Routing

**Status:** Completed
