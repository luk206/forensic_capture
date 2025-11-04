# EC2 Forensics Capture

This GitHub Actions workflow automates the collection of forensic metadata from an Amazon EC2 instance.  
It assumes a role in the target AWS account, gathers key evidence (instance metadata, ENIs, volumes, IAM role info, CloudTrail events, and more), and stores everything as a downloadable artifact.

---

## Features

- Automatically assumes a forensic IAM role in any AWS account.
- Collects:
  - EC2 instance description
  - Summarized metadata
  - ENIs (network interfaces)
  - Attached EBS volumes
  - EC2 console output
  - CloudTrail events related to the instance
  - Security group details
  - IAM instance profile information
- Saves all results under a structured directory inside the final artifact.
- No local AWS CLI required — everything runs inside GitHub Actions.

---

## Usage

### Trigger manually

1. Go to **GitHub → Actions → EC2 Forensics Capture → Run workflow**.
2. Provide the required inputs:

| Input | Description |
|-------|-------------|
| `aws_account_id` | AWS Account ID where the instance exists |
| `role_name` | IAM role to assume for forensic access |
| `instance_id` | EC2 instance to analyze |

---

## Prerequisites

An IAM role must exist in the target AWS account with:

- Permissions for:
  - `ec2:Describe*`
  - `cloudtrail:LookupEvents`
  - `iam:GetRole`
  - `ec2:GetConsoleOutput`
- A trust policy that allows GitHub Actions OIDC to assume it.

Example minimal trust policy (replace with your actual GitHub OIDC provider):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "AWS": "*" },
      "Action": "sts:AssumeRole"
    }
  ]
}
