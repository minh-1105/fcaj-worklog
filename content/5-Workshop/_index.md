---
title : "Workshop"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

# Deploying a Serverless Marketplace Backend on AWS: Student Market

#### Overview

This workshop deploys the AWS backend of **Student Market** — a secondhand marketplace for students — using AWS SAM and CloudFormation. The steps build:

+ **Media pipeline** — browsers upload images directly to a private **Amazon S3** bucket with presigned URLs; an **AWS Lambda** function converts them to WebP; **Amazon CloudFront** delivers only the processed images.
+ **Serverless API** — an ASP.NET Core 8 API running on **AWS Lambda** behind an **Amazon API Gateway** HTTP API, reading its secrets from **AWS Systems Manager Parameter Store**.
+ **Edge security** — **AWS WAF** in front of CloudFront, with the API reachable only through CloudFront.
+ **Monitoring** — **Amazon CloudWatch** alarms, **Amazon SNS** email alerts, an **Amazon SQS** failure queue, and **AWS CloudTrail**.

The frontend (Vercel) and the PostgreSQL database (Supabase) run outside AWS and are only connected to the stacks deployed here.

#### Contents

1. [Workshop overview](5.1-workshop-overview/)
2. [Prerequisites](5.2-prerequisites/)
3. [Media pipeline: S3, Lambda, and CloudFront](5.3-media-pipeline/)
4. [Serverless API: SSM, Lambda, and API Gateway](5.4-serverless-api/)
5. [Edge security with AWS WAF](5.5-edge-security/)
6. [Monitoring and auditing](5.6-monitoring/)
7. [Clean up resources](5.7-cleanup/)
