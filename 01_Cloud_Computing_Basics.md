# Cloud Computing Basics

## What is Cloud Computing?

- On-demand delivery of compute power, database storage, applications, and other IT resources
- Through a cloud service platform with **pay-as-you-go** pricing
- You can provision exactly the right type & right size of computing resources you need
- You can access as many resources as you need, almost instantly

---

## What is a Server Composed of?

| Component | Description |
|-----------|-------------|
| **Compute** | CPU |
| **Memory** | RAM |
| **Storage** | Data |
| **Database** | Store data in a structured way |
| **Network** | Routers, Switch, DNS server |

---

## IT Terminology

- **Network**: cables, routers, and servers connected to each other
- **Router**: a networking device that forwards data packets between computer networks. They know where to send your packets on the internet.
- **Switch**: Takes a packet and sends it to the correct server/client on your network.

---

## Problems with Traditional IT

- Pay for the rent for the Data Center
- Pay for power supply, cooling, maintenance
- Adding and replacing hardware takes time
- Scaling is limited
- Hire 24/7 team to monitor the infrastructure
- How to deal with disasters? (earthquake, power shutdown, fire)

---

## Characteristics of Cloud Computing

1. **On-demand self service**: Users can provision resources and use them without human interaction from the service provider
2. **Multi-tenancy and resource pooling**: Multiple customers can share the same infrastructure & applications with security and privacy. Multiple customers are serviced from the same physical resources.
3. **Rapid elasticity and scalability**: Automatically and quickly acquire and dispose resources when needed. Quickly and easily scale based on demand.
4. **Measured service**: Usage is measured; users pay correctly for what they used.

---

## Advantages of Cloud Computing

1. Pay on-demand: don't own hardware
2. Stop guessing capacity: scale based on actual measured usage
3. Increased speed & Agility
4. Stop spending money running and maintaining data centers
5. Go global in minutes

---

## Problems Solved by Cloud

| Benefit | Description |
|---------|-------------|
| **Flexibility** | Change resource types when needed |
| **Cost-Effectiveness** | Pay as you go, for what you use |
| **Scalability** | Accommodate larger loads by making hardware stronger or adding additional nodes |
| **Elasticity** | Ability to scale out and scale-in when needed |
| **High Availability & Fault Tolerance** | Build across data centers |
| **Agility** | Rapidly develop, test, and launch software applications |

---

## Cloud Service Models (Cloud Stack)

| Layer | Components |
|-------|------------|
| **Infrastructure Layer** | Networking, Storage, Servers, Virtualization |
| **Platform Layer** | Operating System, Middleware, Runtime |
| **Application/Software Layer** | Data, Applications |

### 1. IaaS (Infrastructure as a Service)
- You manage: OS, middleware, runtime, data, applications
- Provider manages: virtualization, servers, storage, networking

### 2. PaaS (Platform as a Service)
- You manage: data, applications
- Provider manages: everything below

### 3. SaaS (Software as a Service)
- Provider manages everything
- You just use the software

---

## Deployment Models

### 1. Public Cloud
- Owned and operated by third-party cloud service providers
- Resources are available to the general public over the internet on a pay-per-usage basis
- Multi-tenancy: resources are shared among multiple users
- Examples: **AWS, Azure, GCP, IBM Cloud, DigitalOcean, Oracle**

### 2. Private Cloud
- Dedicated to a single organization; can be hosted on-premises or by a third party
- Organization has full control over infra, allowing for customization, security, and compliance enforcement
- Enhanced security and data privacy compared to public cloud
- Examples: **OpenStack, Rackspace, VMware Cloud Foundation**

### 3. Hybrid Cloud
- Combination of public and private cloud
- Keep some servers on-premise and extend some capabilities to the cloud
- Control over sensitive assets in your private cloud
- Examples: **AWS Outposts, Azure Arc, Google Anthos**

---

## AWS Pricing Model (Overview)

AWS has **3 pricing fundamentals** — pay-as-you-go model:

1. **Compute**: Pay for compute time
2. **Storage**: Pay for data stored in the cloud
3. **Data Transfer OUT** of the cloud (data transfer IN is free)

Solves the expensive issues of traditional IT.
