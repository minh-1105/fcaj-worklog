---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# Week 3 — Compute: EC2, EBS, and Systems Manager

## Weekly objective

Understand virtual-machine lifecycles, block storage, and secure EC2 administration.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | EC2, AMIs, instance families/types, vCPU/RAM, and shared tenancy. |
| 2 | On-Demand, Savings Plans, Spot Instances, and cost drivers. |
| 3 | EBS volumes, snapshots, instance store, and persistence. |
| 4 | Security Groups, instance profiles, SSH, and Systems Manager Session Manager. |
| 5 | The EC2 lifecycle: launch, reboot, stop, start, terminate; cleanup and public IPv4 charges. |

## Setup to understand

Review how to select a Region, AMI, instance type, VPC/subnet, security group, IAM instance profile, storage, and tags. Session Manager requires SSM Agent, IAM permissions, and suitable network connectivity.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws ec2 describe-instances --region ap-southeast-1
aws ec2 describe-volumes --region ap-southeast-1
aws ec2 describe-security-groups --region ap-southeast-1
aws ssm describe-instance-information --region ap-southeast-1
```

## Learning outcomes

- Drew the relationships among EC2, EBS, Security Groups, and IAM roles.
- Compared Stop, Reboot, and Terminate.
- Explained common reasons why an EC2 instance cannot be reached.
- Described the difference between SSH and Session Manager.

## Self-review questions

- Does EBS continue to incur cost when an EC2 instance is stopped?
- What does stateful mean for a Security Group?
- What prerequisites does Session Manager require?

## Learning resources

- [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
- [EC2 Getting Started](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
- [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [Session Manager setup](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)
