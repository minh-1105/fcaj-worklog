---
title: "Week 12 Worklog"
date: 2026-11-24
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Week 12 — Capstone: Student Market

**Timeline:** 24/11/2026 – 29/11/2026

## Weekly objective

Consolidate the course by analyzing, explaining, and reviewing the AWS architecture of **Student Market** — a second-hand marketplace for students (textbooks, study materials, used items) with a Next.js frontend and an ASP.NET Core (.NET 8) REST API.

## Learning plan

| Session | Date | Topic |
|---:|---|---|
| 1 | Tue, 24/11/2026 | Define functional and non-functional requirements; map each component to where it runs (AWS, Vercel, Supabase) and why. |
| 2 | Wed, 25/11/2026 | API flow: Client → CloudFront (AWS WAF) → API Gateway HTTP API → Lambda (.NET 8) → PostgreSQL; secrets from SSM Parameter Store (SecureString/KMS). |
| 3 | Thu, 26/11/2026 | Media pipeline: presigned upload to private S3 `raw/` → S3 event → ImageProcessor Lambda (WebP) → `processed/` → CloudFront with OAC; failures to SQS. |
| 4 | Fri, 27/11/2026 | Security and operations: least-privilege IAM roles, WAF rules, CloudWatch alarms → SNS, CloudTrail, IAM Access Analyzer; optional realtime chat on ECS Fargate + ALB with images built by CodeBuild into ECR. |
| 5 | Sun, 29/11/2026 | IaC with AWS SAM/CloudFormation (4 stacks), cost review with Budgets/Cost Explorer, cleanup plan; complete diagrams, ADRs, and review questions. |

## AWS services used by Student Market

| Group | Service | Role in Student Market |
|---|---|---|
| Compute | AWS Lambda | API Lambda runs the whole ASP.NET Core backend (auth/JWT, listings, categories, REST chat, presigned upload links); ImageProcessor Lambda creates `large.webp` (1600px) and `thumbnail.webp` (480px). |
| API | Amazon API Gateway (HTTP API) | Public HTTPS entry for the API, with CORS for the frontend origin. |
| Storage | Amazon S3 | Private, versioned, encrypted media bucket: `raw/` for original uploads, `processed/` for WebP images. |
| Delivery | Amazon CloudFront | CDN for `processed/` images through Origin Access Control; also routes `/api/*` to API Gateway with an `X-Origin-Verify` header. |
| Security | AWS WAF | Web ACL on CloudFront: per-IP `/api` rate limit, IP reputation, common rule set, known bad inputs. |
| Security | AWS Systems Manager Parameter Store + AWS KMS | SecureString secrets (DB connection string, JWT key, SMTP password, origin-verify secret) loaded at cold start. |
| Security | AWS IAM, IAM Access Analyzer | One least-privilege role per Lambda, a deployer user/policy, and an account analyzer for external access. |
| Messaging | Amazon SQS, Amazon SNS | SQS failure queue for image processing; SNS topic sending alarm emails. |
| Observability | Amazon CloudWatch | Structured logs with retention and alarms for errors, throttles, duration, and queue depth. |
| Audit | AWS CloudTrail | Management events in all Regions, logged to a dedicated S3 bucket. |
| Realtime (optional) | Amazon ECS (Fargate), Elastic Load Balancing (ALB) | One ARM64 task serving the SignalR hub `/hubs/chat`; the ALB only accepts traffic from CloudFront. Enabled only for demos (~$1.3/day). |
| Build (optional) | AWS CodeBuild, Amazon ECR | Build the `linux/arm64` API image on AWS and store it in ECR. |
| IaC | AWS CloudFormation, AWS SAM | Stacks `media-pipeline`, `api`, `edge-waf` (us-east-1), `realtime` + `build`. |
| Cost | AWS Budgets, Cost Explorer | Track spending against the $170 credit. |

Status: Student Market is deployed to production — the API, media pipeline, and WAF run on AWS, and the frontend is live on Vercel. The realtime stack is enabled only for demos.

Outside AWS: the frontend is hosted on **Vercel** (Hobby) and PostgreSQL runs on **Supabase** (Free, Singapore, through the transaction pooler). Because of that, the project does **not** use RDS, VPC/NAT Gateway, EC2, Cognito, or Amplify.

## Setup to understand

Read the project's SAM templates and the deployment order: database → media pipeline → API → WAF (us-east-1) → frontend → update CORS with the real domain. Main Region is `ap-southeast-1`. Use `sam validate --lint` and LocalStack to check things locally before touching a real account.

## Reference model

```text
               Users (browser)
                     |
                     | HTTPS
        +------------+--------------+
        v                           v
 Vercel (Next.js)        Amazon CloudFront  <--  AWS WAF
                            |            |
                  /api/*    |            |  processed/* (OAC)
                            v            v
                 API Gateway HTTP API   S3 media bucket (private)
                            |             raw/        processed/
                            v              |              ^
                Lambda API (.NET 8)        | ObjectCreated| write WebP
                 |    |      |             v              |
   SSM Parameter |    |      |       Lambda ImageProcessor+
   Store (KMS) <-+    |      |             |
                      |      |             +--> SQS (failures)
       presigned URL -+      v
                       Supabase PostgreSQL (pooler :6543)

 CloudWatch logs/alarms --> SNS (email)   CloudTrail + IAM Access Analyzer
 Optional: CloudFront /hubs/* --> ALB --> ECS Fargate (SignalR), image from CodeBuild -> ECR
```

## Learning outcomes

- Completed the Student Market architecture diagram and data-flow explanation (API flow and image upload flow).
- Prepared a table describing every AWS service and why it was selected over alternatives (Lambda vs. EC2/Fargate, Supabase vs. RDS, Vercel vs. Amplify).
- Analyzed risks: leaked secrets, public bucket exposure, API abuse, image-processing failures, Supabase pausing after 7 idle days, and cost of the realtime stack.
- Wrote a final reflection and next-learning plan.

## Self-review questions

- Why does the browser upload directly to S3 with a presigned URL instead of sending the image through the API?
- How do OAC and the bucket policy ensure only `processed/*` is publicly readable through CloudFront?
- Why must the Lambda API use the Supabase pooler (port 6543) instead of a direct connection?
- Why are secrets stored in SSM Parameter Store instead of CloudFormation parameters or `appsettings`?
- If ImageProcessor fails, how does the system observe it (CloudWatch alarm, SQS queue, SNS email) and recover?

## Learning resources

- [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/)
- [Uploading objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html)
- [Restricting access to an Amazon S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/)
- [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
