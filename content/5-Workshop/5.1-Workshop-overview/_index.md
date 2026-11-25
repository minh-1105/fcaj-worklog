---
title : "Workshop overview"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Architecture

```text
Browser ──HTTPS──▶ Vercel (Next.js static site)
   │
   ├──HTTPS──▶ CloudFront ◀── AWS WAF (us-east-1)
   │             ├── /api/*  ──(X-Origin-Verify)──▶ API Gateway HTTP API ──▶ Lambda API (.NET 8) ──▶ Supabase PostgreSQL
   │             └── images  ──(OAC, processed/* only)──▶ S3 media bucket                 │
   │                                                                                      └──▶ SSM Parameter Store (KMS)
   └──presigned POST──▶ S3 raw/ ──ObjectCreated──▶ Lambda ImageProcessor ──▶ S3 processed/
                                                        └── on failure ──▶ SQS
CloudWatch alarms ──▶ SNS (email)        CloudTrail + IAM Access Analyzer
```

#### Stacks to deploy

| Stack | Template | Region | Main resources |
|---|---|---|---|
| `student-market-dev-media` | `media-pipeline.yaml` | ap-southeast-1 | S3 bucket, ImageProcessor Lambda, SQS, CloudFront + OAC, alarms, SNS, CloudTrail + audit bucket, IAM Access Analyzer |
| `student-market-dev` | `api.yaml` | ap-southeast-1 | API Lambda, HTTP API, log group, alarms |
| `student-market-dev-edge` | `edge-waf.yaml` | **us-east-1** | AWS WAF web ACL (CLOUDFRONT scope) |

The optional realtime chat stacks (`realtime.yaml` with ECS Fargate + ALB, and `build.yaml` with CodeBuild + ECR) are not part of this workshop because they cost about $1.3/day.

#### What to learn

- Why browsers should upload directly to S3 instead of sending files through the API.
- How S3 event notifications trigger Lambda and how failed events reach an SQS queue.
- How Origin Access Control keeps a bucket private while CloudFront serves part of it.
- How to keep secrets out of templates and code with SSM Parameter Store SecureString.
- How to protect an API with AWS WAF and an origin-verification header.
- How to observe a serverless system with CloudWatch, SNS, and CloudTrail, and how to remove everything afterwards.

#### Estimated time and cost

- Time: about 2–3 hours.
- Cost: Lambda, API Gateway, S3, CloudFront, SQS, SNS, and CloudWatch stay within free allowances for workshop traffic. AWS WAF costs about $5 per web ACL plus $1 per rule per month (prorated hourly), so complete the cleanup step (5.7) at the end.
