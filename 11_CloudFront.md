# AWS CloudFront — Content Delivery Network

## Overview

- **Content Delivery Network (CDN)**
- Improves **read performance**, content is cached at the edge
- Improves **user experience**
- **410+ Points of Presence** globally (edge locations)

---

## How CloudFront Works

**Step-by-step flow:**

1. **User goes to the closest edge location**
2. **At the edge**: Check if content is cached
3. **If not cached**: Get from origin, cache it, then send it to user

---

## CloudFront — Origins

### S3 Bucket
- For distributing files and **caching them at the edge**
- Enhanced security with **CloudFront Origin Access Control (OAC)**
  - OAC is replacing Origin Access Identity (OAI)
- CloudFront can be used as an **ingress** (to upload files to S3)

### Custom Origin (HTTP)
- Application Load Balancer
- EC2 instance
- S3 website (must first enable the bucket as a static S3 website)
- Any HTTP backend you want

---

## CloudFront Geo Restriction

- You can **restrict who can access your distribution**

| Mode | Description |
|------|-------------|
| **Allowlist** | Allow users to access your content ONLY if they're in one of the countries on a list of approved countries |
| **Blocklist** | Prevent users from accessing your content if they're in one of the countries on a list of banned countries |

### Control access from EC2 or ALB to CloudFront:
Go to VPC Managed Prefix List → search prefix name `com.amazonaws.global.cloudfront.origin-facing` → copy prefix list ID → security group → paste in source.

---

## CloudFront — Cache Invalidations

- In case you update the back-end origin, **CloudFront doesn't know** about it
- CloudFront will only get the refreshed content after the **TTL has expired**
- However, you can force an entire or partial **cache refresh** (bypassing the TTL) by performing a **CloudFront Invalidation**
- You can invalidate all files `(*)` or a special path `(/images/*)`

---

## Path-Based Routing with Multiple Origins

You can route different paths to different origins:

| Path Pattern | Origin |
|-------------|--------|
| `/images/*` | S3 Bucket |
| `/api/*` | EC2 Instance |

This is configured using **Behaviors** in CloudFront.

---

## CloudFront vs S3 Cross-Region Replication

| Feature | CloudFront | S3 Cross-Region Replication |
|---------|------------|---------------------------|
| **Edge Network** | Global (410+ PoPs) | Must be set up for each region |
| **Files** | Cached for a TTL | Updated in near real-time |
| **Read/Write** | Read only | Read only |
| **Use Case** | Static content that must be available everywhere | Dynamic content that needs to be available at low-latency in few regions |

---

## CloudFront — Custom Error Pages

- You can configure **custom error pages** for specific HTTP error codes (e.g., 404, 500)
- CloudFront returns the custom error page to viewers when the origin returns an error

---

## CloudFront Pricing

- CloudFront Edge locations are all around the world
- The cost of data out per edge location varies
- You can **reduce the number of edge locations** for cost reduction

### Price Classes:
| Class | Coverage |
|-------|---------|
| **Price Class All** | All regions — best performance |
| **Price Class 200** | Most regions, but excludes the most expensive |
| **Price Class 100** | Only the least expensive regions |
