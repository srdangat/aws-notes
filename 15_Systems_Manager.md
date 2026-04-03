# AWS Systems Manager (SSM)

## Overview

- Helps you **manage your EC2 and On-Premises systems at scale**
- Works for **Linux, Windows, MacOS**

### Most Important Features:
- Patching automation for enhanced compliance
- Run commands across an entire fleet of servers
- Store parameter configuration with the **SSM Parameter Store**

---

## Managed Instances

- A **managed instance** is a machine that has been configured for use with Systems Manager
- If the server is managed, you can log into the server from the console itself **without using any third-party tool**
- Remote commands can be run over the instance from SSM if it is managed

---

## SSM Session Manager

- Allows you to start a **secure shell** on your EC2 and on-premises servers
- **No SSH access, bastion hosts, or SSH keys needed**
- **No port 22 needed** (better security)
- Supports **Linux, macOS, and Windows**
- Send session log data to **S3 or CloudWatch Logs**

---

## SSM — Run Command

- Execute a **document (= script)** or just run a command
- Run command across **multiple instances** (using resource groups)
- **No need for SSH**
- Command Output can be shown in the **AWS Console**, sent to **S3 bucket** or **CloudWatch Logs**
- Send notifications to **SNS** about command status (In progress, Success, Failed, …)
- Integrated with **IAM & CloudTrail**
- Can be invoked using **EventBridge**

---

## Systems Manager — Patch Manager

- Automates the process of **patching managed instances**
- OS updates, application updates, security updates
- Supports **EC2 instances and on-premises servers**
- Supports **Linux, macOS, and Windows**
- Patch **on-demand** or **on a schedule** using Maintenance Windows

---

## Systems Manager — Maintenance Windows

- Defines a **schedule** for when to perform actions on your instances
- Example: OS patching, updating drivers, installing software, …

### Maintenance Window contains:
- **Schedule**
- **Duration**
- Set of **registered instances**
- Set of **registered tasks**

---

## Systems Manager — Automation

- Simplifies common **maintenance and deployment tasks** of EC2 instances and other AWS resources
- Examples: restart instances, create an AMI, EBS snapshot

### Automation Runbook
- SSM Documents to define actions performed on your EC2 instances or AWS resources (pre-defined or custom)

### Can be triggered using:
- Manually using **AWS Console, AWS CLI or SDK**
- **Amazon EventBridge**
- On a schedule using **Maintenance Windows**

---

## Systems Manager — Parameter Store

- **Secure storage** for configuration and secrets
- API Keys, passwords, configurations…
- Serverless, scalable, durable, easy SDK
- Control access permissions using **IAM**
- Version tracking & encryption (optional) using **KMS**

### Parameter Store Hierarchy Example:
```
/my-department/
  my-app/
    dev/
      db-url
      db-password
    prod/
      db-url
      db-password
  other-app/
/other-department/
```

### Tiers:

| Feature | Standard | Advanced |
|---------|----------|----------|
| **Total parameters** | 10,000 | 100,000 |
| **Max size per parameter** | 4 KB | 8 KB |
| **Parameter policies** | No | Yes |
| **Cost** | Free | $0.05 per advanced parameter per month |
