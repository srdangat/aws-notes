# AWS VPC — Virtual Private Cloud

## Overview

- **VPC = Virtual Private Cloud**: private network to deploy your resources *(regional resource)*
- **Subnets** allow you to partition your network inside your VPC *(Availability Zone resource)*
- A **public subnet** is a subnet that is accessible from the internet
- A **private subnet** is a subnet that is NOT accessible from the internet
- To define access to the internet and between subnets, we use **Route Tables**

---

## Internet Gateway (IGW)

- Allows resources (e.g., EC2 instances) in a VPC to connect to the Internet
- It **scales horizontally** and is highly available and redundant
- Must be created separately from a VPC
- **One VPC can only be attached to one IGW** and vice versa
- Internet Gateways on their own do **not allow Internet access**
- Route tables must also be edited!

---

## NAT Gateway & NAT Instances

- **NAT Gateways** (AWS-managed) & **NAT Instances** (self-managed) allow your instances in **Private Subnets** to access the internet while remaining private
- NATGW is created in a specific Availability Zone, uses an **Elastic IP**

---

## Network ACL & Security Groups

### NACL (Network ACL)

| Property | Details |
|----------|---------|
| **Type** | Firewall which controls traffic from and to subnet |
| **Rules** | Can have ALLOW and DENY rules |
| **Attachment** | Attached at the **Subnet level** |
| **Rule types** | Rules only include **IP addresses** |
| **Default** | New subnets assigned the Default NACL |

#### NACL Rule Behavior
- Rules have a number (1–32766); **higher precedence with a lower number**
- First rule match will drive the decision
- Example: if you define #100 ALLOW `10.0.0.10/32` and #200 DENY `10.0.0.10/32` → the IP will be **ALLOWED** (100 has higher precedence)
- The last rule is an asterisk `*` and **denies** a request in case of no rule match
- AWS recommends adding rules by **increment of 100**
- Newly created NACLs will **deny everything**
- NACLs are great for **blocking a specific IP address** at the subnet level

---

### Security Groups

| Property | Details |
|----------|---------|
| **Type** | Firewall that controls traffic to and from an EC2 Instance |
| **Rules** | Can have only **ALLOW** rules |
| **Rule types** | Rules include IP addresses and other security groups |

---

## VPC Peering

- **Privately connect two VPCs** using AWS's network
- Make them behave as if they were in the same network
- **Must NOT have overlapping CIDRs**
- VPC Peering connection is **NOT transitive**
  - Example: If VPC A ↔ VPC B, and VPC A ↔ VPC C, this does NOT provide connectivity between B and C
- You must **update route tables** in each VPC's subnets to ensure EC2 instances can communicate

---

## VPC Endpoints (AWS PrivateLink)

- Every AWS service is publicly exposed (public URL)
- **VPC Endpoints** (powered by AWS PrivateLink) allow you to connect to AWS services using a **private network** instead of using the public Internet
- They're **redundant** and scale horizontally
- They remove the need of IGW, NATGW to access AWS Services

### Types of VPC Endpoints

| Type | Details |
|------|---------|
| **Interface Endpoints** (powered by PrivateLink) | Provisions an ENI (private IP address) as an entry point (must attach a Security Group); supports most AWS services; $ per hour + $ per GB of data processed |
| **Gateway Endpoints** | Provisions a gateway and must be used as a target in a route table (does not use security groups); supports **S3 and DynamoDB**; **Free** |

---

## VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
  - **VPC** Flow Logs
  - **Subnet** Flow Logs
  - **Elastic Network Interface (ENI)** Flow Logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to **S3, CloudWatch Logs, and Kinesis Data Firehose**
- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway…

---

## Site-to-Site VPN & Direct Connect

### Site-to-Site VPN
- Connect an on-premises VPN to AWS
- The connection is automatically **encrypted**
- Goes over the **public internet**
- On-premises: must use a **Customer Gateway (CGW)**
- AWS side: must use a **Virtual Private Gateway (VGW)**

### AWS Direct Connect (DX)
- Establish a **physical connection** between on-premises and AWS
- The connection is **private**, secure, and fast
- Goes over a **private network**
- Takes at least a **month** to establish
