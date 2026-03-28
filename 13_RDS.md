# AWS RDS — Relational Database Service

## Overview

- **RDS = Relational Database Service**
- A **managed DB service** for DBs that use SQL as a query language
- Allows you to create databases in the cloud that are managed by AWS

### Supported Database Engines:
- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- **Aurora** (AWS Proprietary database)

---

## Advantage of RDS over EC2-Hosted DB

**RDS is a managed service:**
- Automated provisioning, **OS patching**
- Continuous backups and restore to specific timestamp (**Point in Time Restore**)
- Monitoring dashboards
- **Read replicas** for improved read performance
- **Multi-AZ** setup for DR (Disaster Recovery)
- Maintenance windows for upgrades
- Scaling capability (vertical and horizontal)
- Storage backed by **EBS**
- **BUT you can't SSH into your instances**

---

## RDS Deployment Options

### Read Replicas
- Scale the **read workload** of your DB
- Can create up to **15 Read Replicas**
- Data is only **written to the main DB**
- Uses **asynchronous** replication

> With synchronous replication, data is written to the primary and the replica **simultaneously**. With asynchronous replication, data is first written to primary storage and **then** to the replica.

### Multi-AZ
- **Failover** in case of AZ outage (high availability)
- Data is only **read/written to the main database**
- Can only have **1 other AZ** as failover
- Uses **synchronous** replication

### Multi-Region (Read Replicas)
- **Disaster recovery** in case of region issue
- **Local performance** for global reads
- Replication cost

---

## RDS Components

| Component | Description |
|-----------|-------------|
| **Subnet Group** | Determines the network (subnets and availability zones) where your RDS instances are deployed |
| **Option Group** | Adds additional features and configurations to your RDS instances that are not part of the default setup |
| **Parameter Group** | Manages database engine configuration settings for your RDS instances |

---

## RDS Soft Limits

| Resource | Limit |
|----------|-------|
| DB Instances per Region | 40 (including all instance types and database engines) |
| DB Subnet Groups per Region | 50 |
| DB Parameter Groups per Region | 50 |
| DB Security Groups per Region | 25 |
| DB Option Groups per Region | 20 |
| Parameters per DB Parameter Group | Varies by engine |

---

## Amazon Aurora

- AWS Proprietary database (not open-sourced)
- PostgreSQL and MySQL are both supported as Aurora DB
- Aurora is **"AWS cloud optimized"** — claims 5x performance over MySQL on RDS, 3x over PostgreSQL on RDS
- Aurora storage automatically grows in increments of 10GB, up to **128TB**
- Aurora can have up to **15 replicas** (MySQL has 5)
- Replication process is faster (sub-10ms replica lag)
- Failover in Aurora is instantaneous — High Availability native
- Aurora costs more than RDS (~20% more) but is more efficient

### Aurora Serverless
- Automated database instantiation and **auto-scaling** based on actual usage
- Good for **infrequent, intermittent, or unpredictable** workloads
- No capacity planning needed
- **Pay per second**, can be more cost-effective
