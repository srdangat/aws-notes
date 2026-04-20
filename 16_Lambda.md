# AWS Lambda — Serverless Computing

## What is Serverless?

- **Serverless** is a new paradigm in which the developers **don't have to manage servers** anymore
- They just **deploy code / functions**
- Initially, Serverless == **FaaS (Function as a Service)**
- Serverless was pioneered by **AWS Lambda** but now also includes anything that's managed: databases, messaging, storage, etc.
- **Serverless does NOT mean there are no servers** — it means you just don't manage/provision/see them

---

## Serverless Services in AWS

- **AWS Lambda**
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon S3
- AWS SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

---

## Why AWS Lambda?

### Traditional EC2 Approach:
- Virtual Servers in the Cloud
- Limited by RAM and CPU
- Continuously running
- Scaling means intervention to add/remove servers

### AWS Lambda Approach:
- **Virtual functions** — no servers to manage
- Limited by time — **short executions**
- **Run on-demand**
- **Scaling is automated**

---

## Benefits of AWS Lambda

### Easy Pricing:
- **Pay per request and compute time**
- **Free tier**: 1,000,000 AWS Lambda requests and 400,000 GBs of compute time per month

### Other Benefits:
- Integrated with the **whole AWS suite of services**
- Integrated with many **programming languages**
- Easy monitoring through **AWS CloudWatch**
- Easy to get more resources per function (up to **10GB of RAM**)
- Increasing RAM will also improve **CPU and network**

---

## Lambda Language Support

| Language | Support |
|----------|---------|
| Node.js | ✅ |
| Python | ✅ |
| Java | ✅ |
| C# / .NET | ✅ |
| Golang | ✅ |
| C# / Powershell | ✅ |
| Ruby | ✅ |
| Custom Runtime API | ✅ (community supported, e.g., Rust) |
| Lambda Container Image | ✅ (must implement Lambda Runtime API; ECS/Fargate preferred for arbitrary Docker images) |

---

## Lambda Limits (Per Region)

### Execution:
- **Memory allocation**: 128 MB – 10 GB (1 MB increments)
- **Maximum execution time**: 900 seconds (**15 minutes**)
- **Environment variables**: 4 KB
- **Disk capacity in the "function container"** (`/tmp`): 512 MB to 10 GB
- **Concurrency executions**: 1,000 (can be increased)

### Deployment:
- **Compressed deployment size** (zip): 50 MB
- **Uncompressed deployment size** (code + dependencies): 250 MB
- Can use the `/tmp` directory to load other files at startup
- **Environment variables**: 4 KB

---

## Lambda Use Cases

### Serverless Thumbnail Creation
1. New image in S3
2. S3 triggers Lambda function
3. Lambda creates a thumbnail
4. Lambda pushes thumbnail back to S3 and metadata to DynamoDB

### Serverless CRON Job
1. CloudWatch Events EventBridge Rule triggers Lambda function every hour
2. Lambda performs a task

---

## Lambda@Edge & CloudFront Functions

Used to run code **closer to the user** at edge locations.

| Feature | CloudFront Functions | Lambda@Edge |
|---------|---------------------|-------------|
| **Runtime** | JavaScript | Node.js, Python |
| **# of Requests** | Millions per second | Thousands per second |
| **CloudFront Triggers** | Viewer Request/Response | Viewer/Origin Request/Response |
| **Max Execution Time** | < 1 ms | 5–10 seconds |
| **Memory** | 2 MB | 128 MB – 10 GB |
| **Use Case** | Simple request manipulation | More complex logic |
