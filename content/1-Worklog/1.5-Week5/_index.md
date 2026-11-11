---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Week 5 — Networking: VPCs, Subnets, Routing, and Endpoints

## Weekly objective

Understand how addressing, routing, and access controls determine network flows in AWS.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | IPv4/CIDR, VPCs, subnets, address planning, and Availability Zones. |
| 2 | Route tables, local routes, Internet Gateways, and NAT Gateways. |
| 3 | Public and private subnets differ through routing, not merely by name. |
| 4 | Stateful Security Groups versus stateless Network ACLs. |
| 5 | VPC Endpoints: S3 Gateway and Interface Endpoints; DNS, connectivity, and cost. |

## Setup to understand

Model the process of selecting a VPC CIDR and defining public/private subnets in two AZs, route tables, an Internet Gateway, Security Groups, and endpoints. Do not create a NAT Gateway only for experimentation.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-route-tables
aws ec2 describe-vpc-endpoints
```

## Learning outcomes

- Completed an IP plan and a two-AZ VPC diagram.
- Explained the packet flow from EC2 to S3 through a Gateway Endpoint.
- Compared Security Groups and Network ACLs.
- Explained what makes a subnet public.

## Self-review questions

- What makes a subnet public?
- How do Security Groups and Network ACLs process traffic differently?
- When can a VPC Endpoint avoid the need for a NAT Gateway?

## Learning resources

- [Plan your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html)
- [VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [S3 Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)
