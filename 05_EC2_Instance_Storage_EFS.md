# EC2 Instance Storage && EFS

## EBS — Elastic Block Store Volume

### Overview
- An **EBS (Elastic Block Store) Volume** is a **network drive** you can attach to your instances while they run
- It allows your instances to **persist data**, even after their termination
- They can only be **mounted to one instance at a time**
- They are **bound to a specific availability zone**
- Analogy: Think of them as a **"network USB stick"**

### Key Properties
- It's a **network drive** (i.e., not a physical drive)
  - Uses the network to communicate with the instance → may have a bit of latency
  - Can be detached from an EC2 instance and attached to another quickly
- **Locked to an Availability Zone (AZ)**
  - An EBS Volume in `us-east-1a` **cannot** be attached to `us-east-1b`
  - To move a volume across AZs, you first need to **snapshot** it
- Have a **provisioned capacity** (size in GBs, and IOPS)
  - You get billed for all the provisioned capacity
  - You can increase the capacity of the drive over time

---

## Delete On Termination Attribute

- Controls the EBS behavior when an EC2 instance terminates
- By default, the **root EBS volume is deleted** (attribute enabled)
- By default, any other attached EBS volume is **not deleted** (attribute disabled)
- This can be controlled by the AWS Console / AWS CLI
- **Use case**: preserve root volume when instance is terminated

---

## EBS Snapshots

- Make a **backup (snapshot)** of your EBS volume at a point in time
- Not necessary to detach volume to do snapshot, but **recommended**
- Can **copy snapshots across AZ or Region**

### EBS Snapshot Features

| Feature | Details |
|---------|---------|
| **EBS Snapshot Archive** | Move a snapshot to an "archive tier" that is 75% cheaper. Takes 24–72 hours for restoring. |
| **Recycle Bin for EBS Snapshots** | Setup rules to retain deleted snapshots so you can recover them after accidental deletion. Specify retention (1 day to 1 year). |

---

## EBS Volume Types

### General Purpose SSD
- **Cost effective** storage, **low-latency**
- System boot volumes, virtual desktops, development and test environments
- 1 GiB – 16 TiB

| Type | Details |
|------|---------|
| **gp3** | Baseline 3,000 IOPS and throughput of 125 MiB/s. Can increase IOPS up to 16,000 and throughput up to 1,000 MiB/s independently. |
| **gp2** | Small gp2 volumes can burst IOPS to 3,000. Volume size and IOPS are linked; max IOPS is 16,000. 3 IOPS per GB. |

### Provisioned IOPS (SSD)
- Critical business applications with **sustained IOPS performance**
- Or applications that need more than **16,000 IOPS**
- Great for **database workloads**

| Type | Details |
|------|---------|
| **io1** (4 GiB – 16 TiB) | Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other |
| **io2 Block Express** (4 GiB – 64 TiB) | Sub-millisecond latency; Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1; Supports EBS Multi-attach |

### Hard Disk Drives (HDD)
- **Cannot be a boot volume**
- 125 GiB to 16 TiB

| Type | Details |
|------|---------|
| **Throughput Optimized HDD (st1)** | Big Data, Data Warehouses, Log Processing. Max throughput 500 MiB/s – max IOPS 500 |
| **Cold HDD (sc1)** | For data that is infrequently accessed; lowest cost important; Max throughput 250 MiB/s – max IOPS 250 |

---

## EC2 Instance Store

- EBS volumes are network drives with good but **"limited"** performance
- If you need a **high-performance hardware disk**, use **EC2 Instance Store**
- Better **I/O performance**
- EC2 Instance Store **lose their storage** if they're stopped (**ephemeral**)
- Good for **buffer / cache / scratch data / temporary content**
- **Risk of data loss** if hardware fails
- Backups and Replication are **your responsibility**

---

## AMI — Amazon Machine Image

- **AMI are a customization of an EC2 instance**
- You add your own software, configuration, operating system, monitoring…
- **Faster boot / configuration time** because all your software is pre-packaged
- AMI are built for a **specific region** (and can be copied across regions)

### You can launch EC2 instances from:
- **A Public AMI**: AWS provided
- **Your own AMI**: you make and maintain them yourself
- **An AWS Marketplace AMI**: an AMI someone else made (and potentially sells)

---

## EFS — Elastic File System

- **Managed NFS (Network File System)** that can be mounted on **100s of EC2**
- EFS works with **Linux EC2 instances** in **multi-AZ**
- **Highly available, scalable, expensive** (3x gp2), **pay per use**, no capacity planning
- Use cases: content management, web serving, data sharing, WordPress
- Uses **NFSv4.1 protocol**
- Uses **security groups** to control access to EFS
- Compatible with **Linux-based AMI** (not Windows)

### Security Group Configuration
- **EFS Security Group Inbound**: Allow NFS traffic (TCP port **2049**) from the security group of your EC2 instances
- **EC2 Security Group Outbound**: Allow NFS traffic (TCP port **2049**) to the security group of your EFS file system

### EFS Storage Classes

| Tier | Details |
|------|---------|
| **Standard** | For frequently accessed files |
| **Infrequent Access (EFS-IA)** | Cost to retrieve files, lower price to store |
| **Archive** | Rarely accessed data (few times each year), 50% cheaper |

### EFS Availability & Durability

| Mode | Details |
|------|---------|
| **Standard** | Multi-AZ, great for prod |
| **One Zone** | One AZ, great for dev; backup enabled by default; compatible with IA (EFS One Zone-IA) |

---

## EBS vs EFS Comparison

| Feature | Amazon EBS | Amazon EFS |
|---------|-----------|-----------|
| **Type** | Block storage | File storage |
| **Use Case** | High performance, low-latency workloads | Shared access, scalable file storage |
| **Instance Attachment** | Single EC2 instance | Multiple EC2 instances |
| **Performance** | High IOPS and throughput | Scales with workload, performance modes |
| **Scalability** | Fixed size, resizable manually | Automatically scalable |
| **Data Persistence** | Independent of EC2 lifecycle | Independent of EC2 lifecycle |
| **Pricing** | Provisioned size and IOPS | Storage size and data transfer |
