# Scalability, High Availability & Load Balancing (LB)

## Scalability

- **Scalability** means that an application/system can handle greater loads by adapting
- There are two kinds of scalability:
  - **Vertical Scalability**
  - **Horizontal Scalability** (= elasticity)
- Scalability is linked but different from **High Availability**

---

## Vertical Scalability

- Means **increasing the size** of the instance
- Example: your application runs on a `t2.micro` → scale vertically to `t2.large`
- Very common for **non-distributed systems**, such as a database
- There's usually a **limit** to how much you can vertically scale (hardware limit)

**EC2 Vertical Scaling Range:**
- From: `t2.nano` — 0.5G of RAM, 1 vCPU
- To: `u-12tb1.metal` — 12.3 TB of RAM, 448 vCPUs

---

## Horizontal Scalability

- Means **increasing the number of instances/systems** for your application
- Implies **distributed systems**
- Very common for **web applications**

---

## High Availability

- High Availability usually goes **hand in hand with horizontal scaling**
- Means running your application/system in **at least 2 Availability Zones**
- The goal of HA is to **survive a data center loss (disaster)**

### High Availability & Scalability for EC2

| Approach | Details |
|----------|---------|
| **Vertical Scaling** | Increase instance size (scale up / down) |
| **Horizontal Scaling** | Increase number of instances (scale out / in) via ASG or Load Balancer |
| **High Availability** | Run instances for the same application across **multi AZ** |

---

## What is Load Balancing?

**Load Balancers** are servers that forward traffic to multiple servers (e.g., EC2 instances) downstream.

### Why Use a Load Balancer?
- Spread load across multiple downstream instances
- Expose a **single point of access (DNS)** to your application
- Seamlessly handle **failures** of downstream instances
- Do regular **health checks** to your instances
- Provide **SSL termination (HTTPS)** for your websites
- **High availability** across zones

### Health Checks
- Health Checks are crucial for Load Balancers
- They enable the LB to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (`/health` is common)
- If the response is **not 200 (OK)**, then the instance is **unhealthy**

---

## Why Use an Elastic Load Balancer?

- An **Elastic Load Balancer** is a managed load balancer
- AWS guarantees that it will be working
- AWS takes care of upgrades, maintenance, high availability
- It costs less to setup your own load balancer but it will be a lot more effort
- Integrated with many AWS offerings: EC2, EC2 Auto Scaling Groups, Amazon ECS, AWS Certificate Manager (ACM), CloudWatch, Route 53, AWS WAF, AWS Global Accelerator

---

## Types of Load Balancers

### Classic Load Balancer (v1) — Legacy
- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7)
- Health checks are TCP or HTTP based
- Fixed hostname: `XXX.region.elb.amazonaws.com`

---

### Application Load Balancer (ALB) — Layer 7

- **Layer 7 (HTTP)**
- Load balancing to multiple HTTP applications across machines (**target groups**)
- Load balancing to multiple applications on the **same machine** (e.g., containers)
- Support for HTTP/2 and WebSocket
- Supports redirects (e.g., from HTTP to HTTPS)

#### ALB Routing Rules (based on):
- Path in URL (e.g., `/users` vs `/posts`)
- Hostname in URL
- Query string, Headers

#### ALB Target Groups:
- EC2 instances
- ECS tasks
- Lambda functions
- IP Addresses (must be private IPs)

#### ALB Soft Limits

| Resource | Limit |
|----------|-------|
| Load Balancers per region | 50 |
| Listeners per Load Balancer | 50 |
| Rules per Listener | 100 |
| Target Groups per region | 300 |
| Targets per Target Group | 1,000 |

---

### Network Load Balancer (NLB) — Layer 4

- **Layer 4** — forward TCP & UDP traffic to your instances
- Handle **millions of requests per second**
- **Less latency** ~100 ms (vs 400 ms for ALB)
- **NLB has one static IP per AZ**, and supports assigning Elastic IP (helpful for whitelisting specific IP)
- Used for **extreme performance**, TCP or UDP traffic
- **Not included in the AWS free tier**

#### NLB Target Groups:
- EC2 instances
- IP Addresses (must be private IPs)
- Application Load Balancer
- Health Checks support the **TCP, HTTP, and HTTPS Protocols**

---

### Gateway Load Balancer (GWLB)

- Deploy, scale, and manage a fleet of **3rd party network virtual appliances** in AWS
- Example: Firewalls, Intrusion Detection and Prevention Systems, Deep Packet Inspection Systems
- Operates at **Layer 3 (Network Layer)** — IP Packets
- Functions: Transparent Network Gateway + Load Balancer
- Uses the **GENEVE protocol on port 6081**

---

## Listeners, Rules, and Target Groups

- A **listener** is a process that checks for connection requests, using the protocol and port you configure
- The **rules** you define for your listeners determine how the load balancer routes requests to the **targets** you register
- **Target groups** are a logical grouping of targets behind a load balancer
