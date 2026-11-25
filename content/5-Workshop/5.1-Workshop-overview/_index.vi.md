---
title : "Tổng quan về workshop"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Kiến trúc

```text
Trình duyệt ──HTTPS──▶ Vercel (Next.js static site)
   │
   ├──HTTPS──▶ CloudFront ◀── AWS WAF (us-east-1)
   │             ├── /api/*  ──(X-Origin-Verify)──▶ API Gateway HTTP API ──▶ Lambda API (.NET 8) ──▶ Supabase PostgreSQL
   │             └── ảnh     ──(OAC, chỉ processed/*)──▶ S3 media bucket                 │
   │                                                                                     └──▶ SSM Parameter Store (KMS)
   └──presigned POST──▶ S3 raw/ ──ObjectCreated──▶ Lambda ImageProcessor ──▶ S3 processed/
                                                        └── khi lỗi ──▶ SQS
CloudWatch alarm ──▶ SNS (email)         CloudTrail + IAM Access Analyzer
```

#### Các stack sẽ triển khai

| Stack | Template | Region | Tài nguyên chính |
|---|---|---|---|
| `student-market-dev-media` | `media-pipeline.yaml` | ap-southeast-1 | S3 bucket, Lambda ImageProcessor, SQS, CloudFront + OAC, alarm, SNS, CloudTrail + bucket audit, IAM Access Analyzer |
| `student-market-dev` | `api.yaml` | ap-southeast-1 | Lambda API, HTTP API, log group, alarm |
| `student-market-dev-edge` | `edge-waf.yaml` | **us-east-1** | Web ACL AWS WAF (scope CLOUDFRONT) |

Các stack chat realtime tùy chọn (`realtime.yaml` với ECS Fargate + ALB và `build.yaml` với CodeBuild + ECR) không nằm trong workshop này vì tốn khoảng $1.3/ngày.

#### Những gì sẽ học

- Vì sao trình duyệt nên upload thẳng lên S3 thay vì gửi file qua API.
- S3 event notification kích hoạt Lambda thế nào và event lỗi được đưa vào SQS ra sao.
- Origin Access Control giữ bucket private trong khi CloudFront vẫn phân phối được một phần bucket.
- Cách đưa secret ra khỏi template và code bằng SSM Parameter Store SecureString.
- Cách bảo vệ API bằng AWS WAF và header xác minh origin.
- Cách quan sát hệ thống serverless bằng CloudWatch, SNS, CloudTrail và cách dọn sạch tài nguyên sau khi làm xong.

#### Thời gian và chi phí ước tính

- Thời gian: khoảng 2–3 giờ.
- Chi phí: Lambda, API Gateway, S3, CloudFront, SQS, SNS và CloudWatch nằm trong mức miễn phí với lưu lượng của workshop. AWS WAF tốn khoảng $5 mỗi web ACL cộng $1 mỗi rule mỗi tháng (tính theo giờ), vì vậy hãy làm bước dọn dẹp (5.7) khi xong.
