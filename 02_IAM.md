# AWS IAM — Identity and Access Management

## Overview

- **IAM = Identity and Access Management** — Global service
- IAM basically allows us to control **who can do what** in the AWS environment
- Always stick to **"Minimum Required Access Policy"**

---

## IAM: Users & Groups

- **Root account** created by default — shouldn't be used or shared
- **Users** are people within your organization, and can be grouped
- **Groups** only contain users, not other groups
- A **user** can belong to multiple groups

---

## IAM: Permissions

- Users or Groups can be assigned **JSON documents called Policies**
- These policies define the **permissions** of the users
- In AWS, apply the **least privilege principle**: don't give more permission than a user needs

---

## IAM Policies Structure

A policy consists of:

- **Version**: policy language version — always include `"2012-10-17"`
- **Id**: an identifier for the policy *(optional)*
- **Statement**: one or more individual statements *(required)*

Each **Statement** consists of:

| Field | Description |
|-------|-------------|
| **Sid** | Identifier for the statement *(optional)* |
| **Effect** | Whether the statement allows or denies access (`Allow` / `Deny`) |
| **Principal** | Account/user/role to which this policy is applied |
| **Action** | List of actions this policy allows or denies |
| **Resource** | List of resources to which the actions apply |
| **Condition** | Conditions for when this policy is in effect *(optional)* |

---

## How Can Users Access AWS?

Three options to access AWS:

1. **AWS Management Console** — protected by password + MFA
2. **AWS Command Line Interface (CLI)** — protected by access keys
3. **AWS Software Developer Kit (SDK)** — for code, protected by access keys

### Access Keys

- Generated through the AWS Console
- Users manage their own access keys
- Access Keys are **secret**, just like a password — **don't share them**
- `Access Key ID` ≈ username
- `Secret Access Key` ≈ password

---

## IAM Roles for Services

- Some AWS services will need to perform actions on your behalf
- To do so, we assign permissions to AWS services with **IAM Roles**

### Common Roles:
- EC2 Instance Roles
- Lambda Function Roles
- Roles for CloudFormation

---

## IAM Security Tools

### IAM Credentials Report *(account-level)*
- A report that lists all your account's users and the status of their various credentials

### IAM Access Advisor *(user-level)*
- Shows the service permissions granted to a user and when those services were last accessed
- You can use this information to **revise your policies**

---

## IAM Guidelines & Best Practices

- Don't use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce **MFA (Multi-Factor Authentication)**
- Create and use **Roles** for giving permissions to AWS services
- Use Access Keys for programmatic access (CLI/SDK)
- Audit permissions using IAM Credentials Report & IAM Access Advisor
- **Never share IAM users & Access Keys**

---

## IAM Soft Limits

| Resource | Limit |
|----------|-------|
| Users per AWS account | 5,000 |
| Groups per AWS account | 300 |
| Roles per AWS account | 1,000 |
| Instance Profiles | 1,000 |
| Managed Policies | 1,500 |
| Inline Policies per user/group/role | 10 |
| Groups per user | 10 |
| Roles per instance profile | 1 |
| Access keys per user | 2 |
| Managed policies attached per IAM user | 20 |
| Managed policies attached per IAM group | 10 |
| Managed policies attached per IAM role | 20 |

### Policy Character Limits

| Policy Type | Limit |
|-------------|-------|
| Customer Managed Policies | 6,144 characters |
| Inline Policies | 2,048 characters |
| S3 Bucket Policies | 20,480 characters |
| SNS Topic Policies | 8,192 characters |
| SQS Queue Policies | 8,192 characters |
| Permissions Boundaries | 6,144 characters |
| Service Control Policies (SCPs) | 5,120 characters |
| Session Policies | 2,048 chars (inline); 12,288 total for all inline session policies |
