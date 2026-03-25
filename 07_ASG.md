# Auto Scaling Group (ASG)

## Overview

- In real life, the load on your websites and applications can change
- In the cloud, you can create and get rid of servers very quickly
- ASG are **free** (you only pay for the underlying EC2 instances)

### The Goal of an Auto Scaling Group (ASG):
- **Scale out** (add EC2 instances) to match an increased load
- **Scale in** (remove EC2 instances) to match a decreased load
- Ensure we have a **minimum and maximum** number of EC2 instances running
- **Automatically register** new instances to a load balancer
- **Re-create** an EC2 instance in case a previous one is terminated (e.g., if unhealthy)

---

## Auto Scaling Group Attributes

An ASG requires a **Launch Template** (older "Launch Configurations" are deprecated):

- AMI + Instance Type
- EC2 User Data
- EBS Volumes
- Security Groups
- SSH Key Pair
- IAM Roles for your EC2 Instances
- Network + Subnets Information
- Load Balancer Information
- **Min Size / Max Size / Initial Capacity**
- Scaling Policies

---

## Auto Scaling — CloudWatch Alarms & Scaling

- It is possible to **scale an ASG based on CloudWatch alarms**
- An alarm monitors a **metric** (such as Average CPU, or a custom metric)
- Metrics such as Average CPU are computed for the **overall ASG instances**

### Based on the alarm:
- We can create **scale-out policies** (increase the number of instances)
- We can create **scale-in policies** (decrease the number of instances)

---

## Auto Scaling Groups — Scaling Policies

### 1. Dynamic Scaling

#### Target Tracking Scaling
- Simple to set up
- Example: I want the average ASG CPU to stay at around **40%**

#### Simple / Step Scaling
- When a CloudWatch alarm is triggered (e.g., CPU > 70%), then **add 2 units**
- When a CloudWatch alarm is triggered (e.g., CPU < 30%), then **remove 1**

### 2. Scheduled Scaling
- Anticipate scaling based on **known usage patterns**
- Example: increase the min capacity to 10 at 5 pm on Fridays

### 3. Predictive Scaling
- **Continuously forecast load** and schedule scaling ahead

---

## Good Metrics to Scale On

| Metric | Description |
|--------|-------------|
| **CPUUtilization** | Average CPU utilization across your instances |
| **RequestCountPerTarget** | To make sure the number of requests per EC2 instance is stable |
| **Average Network In / Out** | If your application is network bound |
| **Any custom metric** | That you push using CloudWatch |

---

## ASG Soft Limits

| Resource | Limit |
|----------|-------|
| Auto Scaling Groups per region | 200 |
| Launch Configurations per region | 1,000 |
| Launch Templates per region | 1,000 |
| Scaling Policies per Auto Scaling Group | 50 |
