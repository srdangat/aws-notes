# VPC Basics — Networking Fundamentals

## IP Addressing

- IP address is the **identity of each host** in the network
- Two types:
  - **IPv4** (32-bit) — Four Octets (4 x 8 bits), represented as decimal values (0–255)
  - **IPv6** (128-bit)

### Example: `192.168.56.212`
- `192` = 128 + 64 = 2⁷ + 2⁶
- `168` = 128 + 32 + 8 = 2⁷ + 2⁵ + 2³
- `56` = 32 + 16 + 8 = 2⁵ + 2⁴ + 2³
- `212` = 128 + 64 + 16 + 4 = 2⁷ + 2⁶ + 2⁴ + 2²

---

## CIDR — Classless Inter-Domain Routing

CIDR is a method for allocating IP addresses. They help define an IP address range and are represented as an **IP address + prefix** (e.g., `192.168.0.0/24`).

### Two components:
1. **Base IP**: Represents an IP contained in the range (e.g., `10.0.0.0`, `192.168.0.0`)
2. **Subnet Mask**: Defines how many bits can change in the IP

### Subnet Mask Reference

| CIDR | Subnet Mask | Hosts |
|------|-------------|-------|
| /32 | — | 1 IP (2⁰) |
| /31 | — | 2 IPs (2¹) |
| /30 | — | 4 IPs (2²) |
| /29 | — | 8 IPs (2³) |
| /28 | 255.255.255.240 | 16 IPs (2⁴) |
| /27 | — | 32 IPs (2⁵) |
| /26 | — | 64 IPs (2⁶) |
| /25 | — | 128 IPs (2⁷) |
| /24 | 255.255.255.0 | 256 IPs (2⁸) |
| /16 | 255.255.0.0 | 65,536 IPs (2¹⁶) |
| /8 | 255.0.0.0 | — |
| /0 | — | All IPs |

> **Formula**: Number of hosts = 2^(number of OFF bits)

---

## IP Address Classes

| Class | Address Range |
|-------|---------------|
| A | 1.0.0.1 – 126.255.255.254 |
| B | 128.1.0.1 – 191.255.255.254 |
| C | 192.0.1.1 – 223.255.254.254 |
| D | 224.0.0.0 – 239.255.255.255 |

---

## Private IP Ranges

Private IPs can only allow certain values:

| Range | Description |
|-------|-------------|
| `10.0.0.0/8` (10.0.0.0 – 10.255.255.255) | Big networks |
| `172.16.0.0/12` (172.16.0.0 – 172.31.255.255) | **AWS default VPC** in that range |
| `192.168.0.0/16` (192.168.0.0 – 192.168.255.255) | e.g., home networks |

All the rest of the IP addresses on the internet are **Public**.

---

## Public vs Private vs Elastic IP

| Feature | Private | Public | Elastic |
|---------|---------|--------|---------|
| **Communication** | Within VPC | Over Internet | Over Internet |
| **Address Range** | Gets IP from Subnet | Gets IP from Amazon pool within region | Gets IP from Amazon pool within region |
| **Instance Restart Behavior** | Once assigned, cannot be changed | Changes over instance restart | Does NOT change |
| **Releasing IP** | When instance is terminated | When instance is stopped or terminated | Not Released |

---

## Communication Types

### Unicast
- **One-to-one** communication
- Data sent from a single sender to a single receiver
- Usage: Web browsing, email, VoIP
- Efficient for individual communications; less efficient when same data needs to reach many

### Multicast
- **One-to-many** communication
- Data sent from a single sender to multiple specified receivers
- Usage: Streaming media, online gaming, video conferencing
- More efficient than unicast for group communications

### Broadcast
- **One-to-all** communication
- Data sent from a single sender to all possible receivers within a network
- Usage: ARP requests, DHCP offers in local networks
- Can lead to network congestion in larger networks

---

## AWS Reserved IP Addresses in Subnets

AWS reserves **5 IP addresses** (first 4 and last 1) in each Subnet. These are NOT available for use.

Example: if CIDR block is `10.0.0.0/24`:

| IP | Reserved For |
|----|-------------|
| `10.0.0.0` | Network Address |
| `10.0.0.1` | Reserved by AWS for the VPC router |
| `10.0.0.2` | Reserved by AWS for mapping to Amazon-provided DNS |
| `10.0.0.3` | Reserved by AWS for future use |
| `10.0.0.255` | Network broadcast address |

---

## VPC Scope

| Scope | Services |
|-------|---------|
| **Global Level** | R53, Billing, IAM |
| **Region Level** | VPC (mapped to Region), ELB service |
| **AZ Level** | Subnet (mapped to AZ), EC2, RDS (create in Subnet Level) |

---

## VPC Soft Limits

| Resource | Limit |
|----------|-------|
| VPCs per Region | 5 |
| Subnets per VPC | 200 |
| Internet Gateways per Region | 5 |
| Elastic IP Addresses per Region | 5 |
| Route Tables per VPC | 200 |
| Routes per Route Table | 50 (max 5 non-propagated) |
| Network ACLs per VPC | 200 |
| Rules per Network ACL | 20 inbound + 20 outbound |
| Security Groups per VPC | 2,500 |
| Inbound/Outbound Rules per Security Group | 60 each |
| Security Groups per Network Interface | 5 |
| Network Interfaces (ENIs) per VPC | 350 |
| DHCP Option Sets per VPC | 1 |
| VPN Connections per Region | 50 |
| Customer Gateways per Region | 50 |
| VPC Peering Connections per VPC | 50 |
| Flow Logs per VPC | 2 per network interface, subnet, or VPC |
