---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Week 6 — Databases: Amazon RDS and DynamoDB

## Weekly objective

Choose a relational or NoSQL database based on access patterns and system requirements.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Relational and NoSQL databases; schemas, transactions, ACID, and indexing. |
| 2 | RDS instances, storage, backups, DB subnet groups, and connectivity. |
| 3 | Multi-AZ versus read replicas; high availability versus read scaling. |
| 4 | DynamoDB tables, items, partition keys, sort keys, Query, Scan, and GSIs. |
| 5 | On-demand/provisioned capacity, TTL, backups, cost, and a decision matrix. |

## Setup to understand

Review the configuration fields used to create RDS and DynamoDB resources. Documentation or Console observation is sufficient; no database must be created.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws rds describe-db-instances
aws dynamodb list-tables
aws dynamodb describe-table --table-name TABLE_NAME
```

## Learning outcomes

- Built a decision matrix for RDS and DynamoDB.
- Drew a sample ERD.
- Designed a DynamoDB key using `(station_id, timestamp)`.
- Distinguished Query from Scan and Multi-AZ from read replicas.

## Self-review questions

- Why is DynamoDB Query generally preferable to Scan?
- Is Multi-AZ a read replica for read scaling?
- How do access patterns affect partition-key design?

## Learning resources

- [Amazon RDS Getting Started](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html)
- [RDS High Availability](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
- [DynamoDB Core Components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html)
