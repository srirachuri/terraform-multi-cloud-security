# Day 22: AWS Security Capstone — Checkov Scanning and Security Hardening

## Overview

This stage of the AWS Security Capstone focuses on infrastructure security scanning, remediation, and monitoring using Terraform and Checkov.

The configuration builds on the previous capstone stages and adds security controls for AWS networking, S3 storage, IAM, and CloudWatch logging. Checkov is used to identify potential infrastructure-as-code security issues, review findings, and guide remediation.

## Objectives

- Install and run Checkov against the Terraform configuration.
- Review security findings and identify practical remediations.
- Restrict the VPC's default security group.
- Add S3 lifecycle rules and server access logging.
- Configure VPC Flow Logs with CloudWatch Logs.
- Encrypt the CloudWatch log group using AWS KMS.
- Set log retention to 365 days.
- Re-run Terraform validation and plan checks after changes.
- Document remaining security findings instead of claiming unverified zero findings.

## Security Controls Implemented

### 1. Checkov security scanning

Checkov scans Terraform configurations for potential misconfigurations and security risks.

The evidence includes an initial scan, a failed-check summary, subsequent scans, and Terraform plan checks. The screenshots show an iterative process of investigating findings, changing configuration, and checking the result.

**Important:** A failed Checkov check is a finding to investigate, not automatically proof that the infrastructure is exploitable. Findings should be assessed by severity, applicability, and remediation options. Any remaining findings should be documented with a reason and a planned or accepted remediation.

### 2. EC2 hardening

The Terraform configuration includes these controls:

- IMDSv2 is required through `http_tokens = "required"`.
- The root EBS volume is encrypted.
- EBS uses the `gp3` volume type.
- EC2 detailed monitoring is enabled.
- EBS optimisation is enabled.
- SSH access is restricted to the configured trusted CIDR.
- The instance receives AWS permissions through an IAM instance profile rather than requiring embedded access keys.

The configuration also allows inbound HTTPS from the internet and restricts outbound traffic in the security group to HTTP and HTTPS. Review these rules against the workload's actual requirements.

### 3. Default security group restriction

The default VPC security group is explicitly configured with empty ingress and egress rule lists:

```
resource "aws_default_security_group" "default" {
  vpc_id = aws_vpc.main.id

  ingress = []
  egress  = []

  tags = {
    Name    = "aws-security-capstone-default-sg"
    Project = "AWS-Security-Capstone"
  }
}
```

This reduces the risk of workloads accidentally inheriting permissive rules from the default security group. Workloads should use deliberately configured security groups instead.

### 4. S3 lifecycle management

Lifecycle rules are configured for the main S3 bucket and the access-logs bucket to abort incomplete multipart uploads after seven days.

This helps clean up unfinished uploads and can reduce unnecessary storage costs. It does not automatically expire all stored objects or their versions.

### 5. S3 server access logging

The main S3 bucket sends server access logs to a separate destination bucket.

The destination bucket includes:

- Public access blocking.
- AWS KMS server-side encryption.
- Versioning.
- Bucket-owner-enforced object ownership.
- A bucket policy allowing the S3 logging service to deliver logs under the specified conditions.

Separating access logs from the main bucket helps preserve an audit trail for requests made to the primary bucket.

### 6. VPC Flow Logs

VPC Flow Logs are configured to capture all traffic (`traffic_type = "ALL"`) and deliver the records to CloudWatch Logs.

The configuration includes an IAM role and policy for the Flow Logs service, plus a CloudWatch log group with:

- Log group name: `/aws/vpc/aws-security-capstone`
- Retention: 365 days
- KMS encryption using the capstone KMS key

Flow Logs help with network troubleshooting and security investigations. They record network-flow metadata rather than the contents of network packets.

## Key Terraform Resources

| Resource | Purpose |
|---|---|
| `aws_default_security_group` | Removes ingress and egress rules from the VPC default security group |
| `aws_s3_bucket_lifecycle_configuration` | Aborts incomplete multipart uploads |
| `aws_s3_bucket_logging` | Enables S3 server access logging |
| `aws_s3_bucket_policy` | Authorizes S3 server access log delivery |
| `aws_kms_key` | Provides a KMS key for encryption |
| `aws_cloudwatch_log_group` | Stores VPC Flow Logs with configured retention and encryption |
| `aws_iam_role` | Defines the VPC Flow Logs service role |
| `aws_iam_role_policy` | Grants the permissions needed to deliver logs |
| `aws_flow_log` | Enables VPC Flow Logs for all traffic |

## Validation and Verification Workflow

The screenshots document the security-scanning and Terraform review process.

Typical commands for this workflow are:

```
terraform fmt
terraform validate
terraform plan

checkov -d . --framework terraform
```

After making changes, run the scan again and inspect the remaining findings. Review the Terraform plan before applying any changes, particularly if Terraform proposes replacing an EC2 instance or another resource.

A plan showing replacement should be investigated before approval because replacement may interrupt service or cause data loss if stateful resources are involved.

## Evidence Included

The uploaded ZIP contains these screenshots:

1. `Screenshot Capstone-Day3-01-Checkov-Installation-and-Finding.png` — Checkov installation and an initial finding.
2. `Screenshot Capstone-Day3-02-Checkov-Baseline-Results.png` — Baseline scan results.
3. `Screenshot Capstone-Day3-03-Checkov-Failed-Checks.png` — Failed-check review.
4. `Screenshot Capstone-Day3-04-EC2-Hardening-Terraform-Plan.png` — EC2 hardening plan.
5. `Screenshot Capstone-Day3-05-EC2-Replacement-Investigation.png` — Investigation of a proposed EC2 replacement.
6. `Screenshot Capstone-Day3-06-IAM-Permission-Denied.png` — IAM permission-denied troubleshooting evidence.
7. `Screenshot Capstone-Day3-07-Terraform-No-Changes.png` — Terraform no-change plan.
8. `Screenshot Capstone-Day3-08-Checkov-Current-Results.png` — Updated Checkov results.
9. `Screenshot Capstone-Day3-09-Default-Security-Group-Fix.png` — Default security group remediation.
10. `Screenshot Capstone-Day3-10-Default-Security-Group-Applied.png` — Default security group apply evidence.
11. `Screenshot Capstone-Day3-11-Default-SG-Plan-Check.png` — Follow-up plan check.
12. `Screenshot Capstone-Day3-12-Terraform-Plan-No-Changes.png` — Terraform no-change result.
13. `Screenshot Capstone-Day3-13-Checkov-Final-Scan.png` — Final Checkov scan evidence.
14. `Screenshot Capstone-Day3-14-Replacement-Plan-Warning.png` — Replacement-plan warning.
15. `Screenshot Capstone-Day3-15-S3-Lifecycle-Applied.png` — S3 lifecycle configuration apply evidence.
16. `Screenshot Capstone-Day3-16-Terraform-No-Changes.png` — Terraform no-change result.
17. `Screenshot Capstone-Day3-17-Access-Logging-Plan.png` — S3 access-logging plan.
18. `Screenshot Capstone-Day3-18-S3-Logging-Policy-Applied.png` — S3 logging policy apply evidence.
19. `Screenshot Capstone-Day3-19-Terraform-Plan-No-Changes.png` — Terraform no-change result.
20. `Screenshot Capstone-Day3-20-Checkov-Failure-Summary.png` — Checkov failure summary.
21. `Screenshot Capstone-Day3-21-Terraform-Plan-No-Changes.png` — Terraform no-change result.
22. `Screenshot Capstone-Day3-22-VPC-Flow-Logs-Applied.png` — VPC Flow Logs applied evidence.
23. `Screenshot Capstone-Day3-23-Terraform-Plan-No-Changes.png` — Terraform no-change result.
24. `Screenshot Capstone-Day3-24-CloudWatch-Retention-Applied.png` — CloudWatch retention applied evidence.
25. `Screenshot Capstone-Day3-25.png` — Additional project evidence; describe its exact contents after reviewing the image.

The ZIP also contains `main.tf`.

## Lessons Learned

- Infrastructure scanning helps identify security risks before deployment.
- A scan result needs interpretation and remediation prioritisation, not just a pass/fail response.
- Terraform plans can reveal potentially disruptive replacements before changes are applied.
- Default security groups should not be left permissive without a deliberate reason.
- S3 access logging and VPC Flow Logs improve visibility for security analysis and troubleshooting.
- CloudWatch retention and KMS encryption should be configured according to logging and compliance needs.

## Improvements to Consider

- Review the EC2 AMI selection: the configuration includes an Amazon Linux AMI data source but the EC2 resource currently uses a hard-coded AMI ID. Use a suitable data-source reference or a deliberately managed AMI ID.
- Confirm that the KMS key policy, CloudWatch Logs service principal, and region match the deployment environment.
- Check the final Checkov output and document any remaining findings with their severity and rationale.
- Verify the deployed settings in AWS instead of relying only on Terraform configuration and plan output.

## GitHub and Security Hygiene

Before publishing:

- Do not commit AWS credentials, Terraform state files, saved plan files, or sensitive variable values.
- Keep real `terraform.tfvars` files out of the repository if they contain environment-specific or sensitive information.
- Use a sanitised `terraform.tfvars.example` with placeholder values.
- Avoid publishing public IP addresses or account-specific details unnecessarily.
- Review AWS resources and charges when the lab is complete.

## Completion Status

**Day 22 evidence:** The uploaded screenshots document Checkov scanning, remediation attempts, Terraform plan checks, S3 lifecycle and access logging, default security group restriction, VPC Flow Logs, and CloudWatch retention configuration.

The screenshots include a Checkov failure summary as well as a final scan. Do not claim that every Checkov finding was eliminated unless the final scan output confirms it. The Terraform no-change plans show that Terraform detected no outstanding changes at those points; they do not independently prove that every security control passed a runtime test.

## Next Step

Review the final Checkov results, document any accepted residual findings, verify the deployed AWS controls, and prepare the overall capstone summary for the GitHub portfolio.
