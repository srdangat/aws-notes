# VPC Basics Networking Fundamentals

## IP Addressing

* An **IP (Internet Protocol) address** is the unique identity of each host in a network.
* Two versions of IP addresses are used:

  * **IPv4** – 32-bit address (4 octets × 8 bits)
  * **IPv6** – 128-bit address

Each IPv4 octet ranges from **0–255**.

### Example

**IP Address:** `192.168.56.212`

| Octet | Decimal | Binary Breakdown                      |
| ----- | ------- | ------------------------------------- |
| 1     | 192     | 128 + 64 = 2⁷ + 2⁶                    |
| 2     | 168     | 128 + 32 + 8 = 2⁷ + 2⁵ + 2³           |
| 3     | 56      | 32 + 16 + 8 = 2⁵ + 2⁴ + 2³            |
| 4     | 212     | 128 + 64 + 16 + 4 = 2⁷ + 2⁶ + 2⁴ + 2² |

---

# CIDR (Classless Inter-Domain Routing)

CIDR is a method of allocating IP addresses and defining network ranges.

A CIDR block is represented as:

```text
IP Address / Prefix Length
```

Examples:

```text
10.0.0.0/16
10.0.100.0/24
192.168.0.0/32
```

> **AWS Note:** When creating an **IPv4 VPC**, the **primary IPv4 CIDR block** must have a prefix length between **`/16`** (largest network) and **`/28`** (smallest network). In contrast, **CIDR notation** itself supports prefix lengths from **`/0`** to **`/32`**.

---

## CIDR Components

Every CIDR block has two parts.

### 1. Base IP Address

Represents the **network address (base address)** of the CIDR block. It is the first IP address in the CIDR range and identifies the network itself.

Examples:

```text
10.0.0.0
172.16.0.0
192.168.0.0
```

---

### 2. Prefix Length (Subnet Mask)

The prefix length determines:

- Number of Network Bits
- Number of Host Bits

```text
Host Bits = 32 - Prefix Length
```

The host bits identify individual IP addresses within the network.

---

# Understanding CIDR

## Example 1: `10.0.0.0/16`

```text
Prefix Length = 16

Host Bits
= 32 - 16
= 16

Total IP Addresses
= 2^16
= 65,536
```

Since the first **16 bits** are fixed:

```text
10.0.x.x
```

the third and fourth octets can vary.

```text
10.0.0.0
10.0.0.1
10.0.0.2
...
10.0.0.255

10.0.1.0
10.0.1.1
...
10.0.1.255

...

10.0.255.255
```

---

## Example 2: `10.0.100.0/24`

```text
Prefix Length = 24

Host Bits
= 32 - 24
= 8

Total IP Addresses
= 2^8
= 256
```

Since the first **24 bits** are fixed:

```text
10.0.100.x
```

Only the last octet changes.

```text
10.0.100.0
10.0.100.1
10.0.100.2
...
10.0.100.255
```

---

# CIDR Reference

| CIDR | Host Bits | Subnet Mask     |           Total IPs | Example IP Range              |
| ---- | --------: | --------------- | ------------------: | ----------------------------- |
| /32  |         0 | 255.255.255.255 |              1 (2⁰) | 192.168.0.0                   |
| /31  |         1 | 255.255.255.254 |              2 (2¹) | 192.168.0.0 – 192.168.0.1     |
| /30  |         2 | 255.255.255.252 |              4 (2²) | 192.168.0.0 – 192.168.0.3     |
| /29  |         3 | 255.255.255.248 |              8 (2³) | 192.168.0.0 – 192.168.0.7     |
| /28  |         4 | 255.255.255.240 |             16 (2⁴) | 192.168.0.0 – 192.168.0.15    |
| /27  |         5 | 255.255.255.224 |             32 (2⁵) | 192.168.0.0 – 192.168.0.31    |
| /26  |         6 | 255.255.255.192 |             64 (2⁶) | 192.168.0.0 – 192.168.0.63    |
| /25  |         7 | 255.255.255.128 |            128 (2⁷) | 192.168.0.0 – 192.168.0.127   |
| /24  |         8 | 255.255.255.0   |            256 (2⁸) | 192.168.0.0 – 192.168.0.255   |
| /16  |        16 | 255.255.0.0     |       65,536 (2¹⁶) | 192.168.0.0 – 192.168.255.255 |
| /8   |        24 | 255.0.0.0       |   16,777,216 (2²⁴) | 10.0.0.0 – 10.255.255.255     |
| /0   |        32 | 0.0.0.0         | 4,294,967,296 (2³²) | 0.0.0.0 – 255.255.255.255     |

---

# Quick Memory Trick

## `/16`

```text
10.0.x.x

✓ First 2 octets remain fixed
✓ Last 2 octets change
```

---

## `/24`

```text
10.0.100.x

✓ First 3 octets remain fixed
✓ Last octet changes
```

---

## `/32`

```text
192.168.0.0

✓ No host bits
✓ Only one IP address
```

---

# CIDR Formulas

```text
Network Bits = Prefix Length

Host Bits = 32 - Prefix Length

Total IP Addresses = 2^(Host Bits)
```

Example:

```text
192.168.0.0/24

Network Bits = 24
Host Bits = 8

Total Addresses = 2^8 = 256
```

---

# IP Address Classes

| Class | Address Range               |
| ----- | --------------------------- |
| A     | 1.0.0.1 – 126.255.255.254   |
| B     | 128.1.0.1 – 191.255.255.254 |
| C     | 192.0.1.1 – 223.255.255.254 |
| D     | 224.0.0.0 – 239.255.255.255 |

> **Note:** Modern networks (including AWS VPCs) use **CIDR**, not classful addressing. IP classes are included only for historical understanding.

---

# Private IP Ranges

Private IP addresses are used within private networks and cannot be routed over the Internet.

| CIDR             | Address Range                 | Typical Usage                    |
| ---------------- | ----------------------------- | -------------------------------- |
| `10.0.0.0/8`     | 10.0.0.0 – 10.255.255.255     | Large Enterprise Networks        |
| `172.16.0.0/12`  | 172.16.0.0 – 172.31.255.255   | AWS VPCs commonly use this range |
| `192.168.0.0/16` | 192.168.0.0 – 192.168.255.255 | Home / Office Networks           |

All remaining IPv4 addresses are **Public IP addresses**.

---

# Public vs Private vs Elastic IP

| Feature                       | Private IP                                   | Public IP              | Elastic IP                    |
| ----------------------------- | -------------------------------------------- | ---------------------- | ----------------------------- |
| **Typical Use**               | Communication within the VPC/private network | Internet communication | Static Internet communication |
| **Address Source**            | Subnet CIDR                                  | AWS Public IPv4 Pool   | AWS Public IPv4 Pool          |
| **Retained After Stop/Start** | Yes                                          | No                     | Yes                           |
| **Released on Stop**          | No                                           | Yes                    | No                            |
| **Released on Termination**   | Yes                                          | Yes                    | Only if manually released     |


---

# AWS Reserved IP Addresses

AWS reserves the **first four IP addresses and the last IP address** in every subnet (5 IP addresses total).

Example subnet:

```text
10.0.0.0/24
```

| Reserved IP | Purpose                            |
| ----------- | ---------------------------------- |
| 10.0.0.0    | Network Address                    |
| 10.0.0.1    | VPC Router                         |
| 10.0.0.2    | Amazon-provided DNS                |
| 10.0.0.3    | Reserved for Future Use            |
| 10.0.0.255  | Reserved by AWS (last IP address)  |

> **Note:** Although the last IP address is traditionally used as the broadcast address in IPv4 networks, AWS VPC does **not** support broadcast traffic. AWS simply reserves this address.

Example:

```text
10.0.0.0/24

Total IP Addresses  : 256
Reserved by AWS     : 5 IP addresses
Usable IP Addresses : 251
```

---

# Traditional Networking vs AWS

```text
Traditional Networking

Usable Hosts
= Total IP Addresses - 2

(Network Address + Broadcast Address)

Example

/24

256 - 2 = 254 usable hosts
```

```text
AWS VPC

Usable IP Addresses
= Total IP Addresses - 5

(Network + Router + DNS + Future Use + Last Address)

Example

/24

256 - 5 = 251 usable IP addresses
```

---

# VPC Scope

| Scope | AWS Services |
|--------|--------------|
| Global | Route 53, IAM, Billing |
| Regional | VPC, Internet Gateway, NAT Gateway, Elastic Load Balancer (ELB) |
| Availability Zone | Subnets, EC2 Instances, RDS Instances |

---

# Default AWS VPC Service Quotas

| Resource                 | Default Limit |
| ------------------------ | ------------: |
| VPCs per Region          | 5 |
| Subnets per VPC          | 200 |
| Internet Gateways        | 5 |
| Elastic IPs              | 5 |
| Route Tables             | 200 |
| Routes per Route Table   | 50 |
| Network ACLs             | 200 |
| Rules per NACL           | 20 Inbound + 20 Outbound |
| Security Groups          | 2,500 |
| Rules per Security Group | 60 Inbound + 60 Outbound |
| Security Groups per ENI  | 5 |
| ENIs per Region          | 350 |
| DHCP Option Sets         | 1 |
| VPN Connections          | 50 |
| Customer Gateways        | 50 |
| VPC Peering Connections  | 50 |
| Flow Logs                | 2 per ENI / Subnet / VPC |

> **Note:** These are common default AWS service quotas. Actual quotas may vary by AWS account, Region, and service updates. Many quotas can be increased by requesting a quota increase.

---

# Important Formulas

```text
IPv4 Size = 32 Bits

Network Bits = Prefix Length

Host Bits = 32 - Prefix Length

Total IP Addresses = 2^(Host Bits)

Traditional Usable Hosts = Total IP Addresses - 2

AWS Usable IP Addresses = Total IP Addresses - 5
```
