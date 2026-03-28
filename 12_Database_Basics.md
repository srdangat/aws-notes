# AWS Database Basics

## Introduction

- Storing data on disk (EFS, EBS, EC2 Instance Store, S3) can have its limits
- Sometimes, you want to store data in a **database**
- You can **structure the data**
- You build **indexes** to efficiently query/search through the data
- You define **relationships** between your datasets
- Databases are optimized for a purpose and come with different features, shapes, and constraints

---

## Relational Databases

- Looks just like Excel spreadsheets, with **links between them**
- Can use the **SQL language** to perform queries/lookups
- Tables, rows, and columns with defined schema

---

## NoSQL Databases

- **NoSQL = non-SQL = non-relational databases**
- Purpose built for specific data models and have **flexible schemas** for building modern applications

### Benefits:
- **Flexibility**: easy to evolve data model
- **Scalability**: designed to scale-out by using distributed clusters
- **High-performance**: optimized for a specific data model
- **Highly functional**: types optimized for the data model

### Examples:
Key-value, document, graph, in-memory, search databases

---

## NoSQL Data Example: JSON

- **JSON = JavaScript Object Notation**
- Common form of data that fits into a NoSQL model
- Data can be **nested**
- Fields can **change over time**
- Support for new types: **arrays**, etc.

---

## Database Types

| Type | AWS Services |
|------|-------------|
| **RDBMS (SQL / OLTP)** | RDS, Aurora — great for joins |
| **NoSQL** | DynamoDB (~JSON), ElastiCache (key/value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for Apache Cassandra) |
| **Object Store** | S3 (for big objects) / Glacier (for backups/archives) |
| **Data Warehouse (SQL Analytics / BI)** | Redshift (OLAP), Athena, EMR |
| **Search** | OpenSearch (JSON) — free text, unstructured searches |
| **Graphs** | Amazon Neptune — displays relationships between data |
| **Ledger** | Amazon Quantum Ledger Database |
| **Time Series** | Amazon Timestream |
