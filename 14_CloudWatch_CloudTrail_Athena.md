# AWS CloudWatch, CloudTrail & Athena

## Amazon CloudWatch

### CloudWatch Metrics

- CloudWatch provides **metrics for every service in AWS**
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to **namespaces**
- **Dimension** is an attribute of a metric (instance id, environment, etc.)
- Up to **30 dimensions** per metric
- Metrics have **timestamps**
- Can create **CloudWatch dashboards** of metrics
- Can create **CloudWatch Custom Metrics** (e.g., RAM)

### Important Metrics

| Service | Metrics |
|---------|---------|
| **EC2 instances** | CPU Utilization, Status Checks, Network (not RAM); default every 5 min; Detailed Monitoring ($$$): every 1 min |
| **EBS volumes** | Disk Read/Writes |
| **S3 buckets** | BucketSizeBytes, NumberOfObjects, AllRequests |
| **Billing** | Total Estimated Charge (only in us-east-1) |
| **Service Limits** | How much you've been using a service API |
| **Custom metrics** | Push your own metrics |

---

## CloudWatch Logs

### Log Structure
- **Log groups**: arbitrary name, usually representing an application
- **Log stream**: instances within application / log files / containers
- Can define **log expiration policies** (never expire, 1 day to 10 years…)

### CloudWatch Logs — Sources
- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: collection of logs from application
- ECS: collection from containers
- AWS Lambda: collection from function logs
- VPC Flow Logs: VPC specific logs
- API Gateway
- CloudTrail based on filter
- Route53: Log DNS queries

### CloudWatch Logs — Destinations
- Amazon S3 (exports)
- Kinesis Data Streams
- Kinesis Data Firehose
- AWS Lambda
- OpenSearch
- Logs are **encrypted by default**

---

## CloudWatch Logs for EC2

- By default, **no logs** from your EC2 machine will go to CloudWatch
- You need to run a **CloudWatch agent** on EC2 to push the log files you want
- Make sure **IAM permissions are correct**
- The CloudWatch log agent can be **setup on-premises** too

---

## CloudWatch Logs Agent & Unified Agent

For virtual servers (EC2 instances, on-premises servers…):

### CloudWatch Logs Agent (Old Version)
- Old version of the agent
- Can only send to **CloudWatch Logs**

### CloudWatch Unified Agent (Recommended)
- Collect additional **system-level metrics** such as RAM, processes, etc.
- Collect logs to send to CloudWatch Logs
- **More capabilities** than the old agent

---

## CloudWatch Alarms

- Alarms are used to **trigger notifications** for any metric
- Various options (sampling, %, max, min, etc.)

### Alarm States:
- `OK`
- `INSUFFICIENT_DATA`
- `ALARM`

### Period:
- Length of time in **seconds** to evaluate the metric
- High resolution custom metrics: **10 sec, 30 sec or multiples of 60 sec**

### CloudWatch Alarm Targets:
- **Stop, Terminate, Reboot, or Recover** an EC2 Instance
- **Trigger Auto Scaling Action**
- **Send notification to SNS** (from which you can do pretty much anything)

---

## CloudWatch Monitoring Types

### Basic Monitoring
- **Frequency**: Data collected at **5-minute** intervals
- **Cost**: Typically included at no additional cost for many AWS services
- **Use Cases**: Suitable for applications where high granularity isn't critical; good for general monitoring and basic operational insights

### Detailed Monitoring
- **Frequency**: Data collected at **1-minute** intervals
- **Cost**: Generally incurs additional charges (more granular data)
- **Use Cases**: Ideal for applications that require real-time monitoring and quick responses to performance changes; useful when performance metrics need to be tracked closely

---

## AWS CloudTrail

- Provides **governance, compliance, and audit** for your AWS account
- CloudTrail is **enabled by default**
- Get a history of **events / API calls** made within your AWS account by:
  - Console
  - SDK
  - CLI
  - AWS Services
- Can put logs from CloudTrail into **CloudWatch Logs or S3**
- A trail can be applied to **All Regions (default) or a single Region**
- If a resource is deleted in AWS, investigate **CloudTrail first**

### CloudTrail Events:
| Type | Description |
|------|-------------|
| **Management Events** | Operations performed on resources in your AWS account (e.g., configuring security, configuring rules for routing data) |
| **Data Events** | Resource-level operations (e.g., S3 object-level activity, Lambda function execution) |
| **CloudTrail Insights Events** | Detect unusual activity in your account |

---

## AWS Athena

- **Serverless** query service to analyze data stored in **Amazon S3**
- Uses standard **SQL language** to query the files (built on Presto)
- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing: ~**$5 per TB of data scanned**
- Commonly used with **Amazon Quicksight** for reporting/dashboards

### Use Cases:
- Business intelligence / analytics / reporting
- Analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc.

### Performance Improvement:
- Use **columnar data** for cost-savings (less scan)
- **Compress** data for smaller retrievals
- **Partition** datasets in S3 for easy querying
- Use **larger files** (> 128 MB) to minimize overhead
