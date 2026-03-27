# DNS Basics & Amazon Route 53

## What is DNS?

- **DNS (Domain Name System)** translates human-readable domain names to machine-readable IP addresses
- Example: `www.amazon.com` → `192.0.2.44`
- DNS is the **backbone of the Internet**
- DNS uses a **hierarchical naming structure**

### Hierarchy Example
```
.com
  └── example.com
        ├── www.example.com
        └── api.example.com
```

---

## DNS Terminologies

| Term | Description |
|------|-------------|
| **Domain Registrar** | Amazon Route 53, GoDaddy, … |
| **DNS Records** | A, AAAA, CNAME, NS, … |
| **Zone File** | Contains DNS records |
| **Name Server** | Resolves DNS queries (Authoritative or Non-Authoritative) |
| **Top Level Domain (TLD)** | `.com`, `.us`, `.in`, `.gov`, `.org`, … |
| **Second Level Domain (SLD)** | `amazon.com`, `google.com`, … |

---

## Amazon Route 53

- **Route 53** is a Managed DNS (Domain Name System)
- A **highly available, scalable, fully managed and Authoritative DNS**
  - Authoritative = the customer (you) can update the DNS records
- Route 53 is also a **Domain Registrar**
- Ability to check the **health of your resources**
- The only AWS service which provides **100% availability SLA**
- Why Route 53? **53** is a reference to the traditional DNS port

---

## Route 53 — Records

Each record contains:
- **Domain/subdomain Name** — e.g., `example.com`
- **Record Type** — e.g., A or AAAA
- **Value** — e.g., `12.34.56.78`
- **Routing Policy** — how Route 53 responds to queries
- **TTL** — amount of time the record is cached at DNS Resolvers

### DNS Record Types

#### Must Know:
| Type | Description |
|------|-------------|
| **A** | Maps a hostname to IPv4 |
| **AAAA** | Maps a hostname to IPv6 |
| **CNAME** | Maps a hostname to another hostname |
| **NS** | Name Servers for the Hosted Zone / indicates the authoritative name server for the domain |

#### Advanced:
| Type | Description |
|------|-------------|
| **MX** | Specifies a mail server responsible for accepting mail on behalf of a domain |
| **TXT** | Store text data, often used for verification or authentication |
| **PTR** | Used for reverse DNS lookup — map an IP address to a domain name |
| **SRV** | Specifies the location of a server for specified services |
| **CAA / DS / NAPTR / SOA / SPF** | Advanced use cases |

---

## Route 53 — Hosted Zones

- A **container for records** that define how to route traffic to a domain and its subdomains
- You pay **$0.50 per month** per hosted zone

### Public Hosted Zones
- Contains records that specify how to route traffic on the **Internet** (public domain names)
- Example: `application1.mypublicdomain.com`

### Private Hosted Zones
- Contain records that specify how you route traffic **within one or more VPCs** (private domain names)
- Example: `application1.company.internal`

---

## Route 53 — Records TTL (Time To Live)

| TTL | Impact |
|-----|--------|
| **High TTL** (e.g., 24 hr) | Less traffic on Route 53; possibly outdated records |
| **Low TTL** (e.g., 60 sec) | More traffic on Route 53 ($$); records outdated for less time; easy to change records |

> **Except for Alias records**, TTL is mandatory for each DNS record.

---

## CNAME vs Alias

### CNAME
- Points a hostname to **any other hostname** (e.g., `app.mydomain.com` → `blabla.anything.com`)
- **ONLY FOR NON ROOT DOMAIN** (e.g., `something.mydomain.com`)

### Alias Records
- Points a hostname to an **AWS Resource** (e.g., `app.mydomain.com` → `blabla.amazonaws.com`)
- Works for **ROOT DOMAIN and NON ROOT DOMAIN** (e.g., `mydomain.com`)
- **Free of charge**
- Native health check
- **Maps a hostname to an AWS resource**
- An extension to DNS functionality
- **Automatically recognizes** changes in the resource's IP addresses
- Unlike CNAME, it **can be used for the top node** of a DNS namespace (Zone Apex), e.g.: `example.com`
- Alias Record is always of type **A/AAAA** for AWS resources (IPv4 / IPv6)
- **You can't set the TTL**
- **You cannot set an ALIAS record for an EC2 DNS name**

### Alias Records Targets
- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone

---

## Route 53 Routing Policies

Route 53 supports several routing policies to control how it responds to DNS queries:

| Policy | Use Case |
|--------|----------|
| **Simple** | Route to a single resource; no health checks |
| **Weighted** | Route traffic across multiple resources with defined weights |
| **Latency-based** | Route to the resource with the lowest latency |
| **Failover** | Active-passive failover with health checks |
| **Geolocation** | Route based on user's location |
| **Geoproximity** | Route based on geographic location + bias |
| **Multi-Value Answer** | Route to multiple resources, return multiple values |
| **IP-based** | Route based on client's IP address |
