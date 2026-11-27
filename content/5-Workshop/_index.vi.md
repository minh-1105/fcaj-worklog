---
title : "Workshop"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

# Triển khai backend serverless cho marketplace trên AWS: Student Market

## Tổng quan

Workshop này triển khai phần backend trên AWS của **Student Market** — chợ đồ cũ dành cho sinh viên — bằng AWS SAM và CloudFormation. Các bước lần lượt dựng:

+ **Pipeline ảnh** — trình duyệt upload ảnh thẳng lên bucket **Amazon S3** private bằng presigned URL; một hàm **AWS Lambda** chuyển ảnh sang WebP; **Amazon CloudFront** chỉ phân phối ảnh đã xử lý.
+ **API serverless** — API ASP.NET Core 8 chạy trên **AWS Lambda** phía sau **Amazon API Gateway** (HTTP API), đọc secret từ **AWS Systems Manager Parameter Store**.
+ **Bảo mật ở edge** — **AWS WAF** đứng trước CloudFront, API chỉ được gọi qua CloudFront.
+ **Giám sát** — alarm **Amazon CloudWatch**, email cảnh báo qua **Amazon SNS**, hàng đợi lỗi **Amazon SQS** và **AWS CloudTrail**.

Frontend (Vercel) và database PostgreSQL (Supabase) chạy ngoài AWS, chỉ được kết nối với các stack triển khai ở đây.

## Nội dung

1. [Tổng quan về workshop](5.1-workshop-overview/)
2. [Chuẩn bị](5.2-prerequisites/)
3. [Pipeline ảnh: S3, Lambda và CloudFront](5.3-media-pipeline/)
4. [API serverless: SSM, Lambda và API Gateway](5.4-serverless-api/)
5. [Bảo mật ở edge với AWS WAF](5.5-edge-security/)
6. [Giám sát và audit](5.6-monitoring/)
7. [Dọn dẹp tài nguyên](5.7-cleanup/)
