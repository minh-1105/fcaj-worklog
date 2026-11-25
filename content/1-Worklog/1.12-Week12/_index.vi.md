---
title: "Worklog Tuần 12"
date: 2026-11-24
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Tuần 12 — Capstone: Student Market

**Thời gian:** 24/11/2026 – 29/11/2026

## Mục tiêu tuần

Tổng hợp kiến thức bằng cách phân tích, giải thích và phản biện kiến trúc AWS của **Student Market** — nền tảng chợ đồ cũ cho sinh viên (giáo trình, tài liệu học tập, đồ dùng đã qua sử dụng), gồm frontend Next.js và REST API ASP.NET Core (.NET 8).

## Nội dung học tập

| Buổi | Ngày | Nội dung |
|---:|---|---|
| 1 | Thứ Ba, 24/11/2026 | Xác định yêu cầu chức năng và phi chức năng; xác định mỗi thành phần chạy ở đâu (AWS, Vercel, Supabase) và lý do lựa chọn. |
| 2 | Thứ Tư, 25/11/2026 | Luồng API: Client → CloudFront (AWS WAF) → API Gateway HTTP API → Lambda (.NET 8) → PostgreSQL; secret lấy từ SSM Parameter Store (SecureString/KMS). |
| 3 | Thứ Năm, 26/11/2026 | Pipeline ảnh: upload bằng presigned URL vào S3 `raw/` (private) → S3 event → Lambda ImageProcessor (WebP) → `processed/` → CloudFront qua OAC; lỗi đẩy vào SQS. |
| 4 | Thứ Sáu, 27/11/2026 | Bảo mật và vận hành: IAM role least privilege, rule WAF, CloudWatch alarm → SNS, CloudTrail, IAM Access Analyzer; chat realtime tùy chọn trên ECS Fargate + ALB, image build bằng CodeBuild lưu vào ECR. |
| 5 | Chủ Nhật, 29/11/2026 | IaC bằng AWS SAM/CloudFormation (4 stack), xem chi phí bằng Budgets/Cost Explorer, kế hoạch dọn tài nguyên; hoàn thiện sơ đồ, ADR và câu hỏi phản biện. |

## Các dịch vụ AWS mà Student Market sử dụng

| Nhóm | Dịch vụ | Vai trò trong Student Market |
|---|---|---|
| Compute | AWS Lambda | Lambda API chạy toàn bộ backend ASP.NET Core (đăng nhập/JWT, tin đăng, danh mục, chat REST, cấp link upload); Lambda ImageProcessor tạo `large.webp` (1600px) và `thumbnail.webp` (480px). |
| API | Amazon API Gateway (HTTP API) | Địa chỉ HTTPS công khai của API, cấu hình CORS cho domain frontend. |
| Storage | Amazon S3 | Bucket ảnh private, bật versioning và mã hóa: `raw/` chứa ảnh gốc, `processed/` chứa ảnh WebP. |
| Phân phối | Amazon CloudFront | CDN phân phối ảnh `processed/` qua Origin Access Control; đồng thời chuyển `/api/*` tới API Gateway kèm header `X-Origin-Verify`. |
| Bảo mật | AWS WAF | Web ACL gắn vào CloudFront: giới hạn tần suất `/api` theo IP, IP reputation, common rule set, known bad inputs. |
| Bảo mật | AWS Systems Manager Parameter Store + AWS KMS | Lưu secret dạng SecureString (connection string DB, JWT key, mật khẩu SMTP, secret origin-verify), nạp khi Lambda cold start. |
| Bảo mật | AWS IAM, IAM Access Analyzer | Mỗi Lambda một role riêng đúng quyền cần dùng, một user/policy để deploy, Access Analyzer phát hiện truy cập từ bên ngoài. |
| Messaging | Amazon SQS, Amazon SNS | SQS lưu các lần xử lý ảnh lỗi; SNS gửi email cảnh báo từ alarm. |
| Giám sát | Amazon CloudWatch | Log có cấu trúc, giới hạn thời gian lưu; alarm cho lỗi, throttle, thời gian chạy và độ dài hàng đợi lỗi. |
| Audit | AWS CloudTrail | Ghi management events ở mọi Region vào một bucket S3 riêng. |
| Realtime (tùy chọn) | Amazon ECS (Fargate), Elastic Load Balancing (ALB) | Một task ARM64 phục vụ hub SignalR `/hubs/chat`; ALB chỉ nhận traffic từ CloudFront. Chỉ bật khi demo (~$1.3/ngày). |
| Build (tùy chọn) | AWS CodeBuild, Amazon ECR | Build image API `linux/arm64` trên AWS và lưu vào ECR. |
| IaC | AWS CloudFormation, AWS SAM | Các stack `media-pipeline`, `api`, `edge-waf` (us-east-1), `realtime` + `build`. |
| Chi phí | AWS Budgets, Cost Explorer | Theo dõi chi tiêu trên $170 credit. |

Trạng thái: Student Market đã được deploy lên production — API, pipeline ảnh và WAF chạy trên AWS, frontend chạy trên Vercel. Stack realtime chỉ bật khi demo.

Ngoài AWS: frontend host trên **Vercel** (Hobby), PostgreSQL chạy trên **Supabase** (Free, Singapore, qua transaction pooler). Vì vậy dự án **không** dùng RDS, VPC/NAT Gateway, EC2, Cognito hay Amplify.

## Setup cần biết

Đọc các template SAM của dự án và thứ tự deploy: database → pipeline ảnh → API → WAF (us-east-1) → frontend → cập nhật CORS bằng domain thật. Region chính là `ap-southeast-1`. Dùng `sam validate --lint` và LocalStack để kiểm tra trên máy trước khi đụng tới tài khoản thật.

## Mô hình tham khảo

```text
               Người dùng (trình duyệt)
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
                Lambda API (.NET 8)        | ObjectCreated| ghi WebP
                 |    |      |             v              |
   SSM Parameter |    |      |       Lambda ImageProcessor+
   Store (KMS) <-+    |      |             |
                      |      |             +--> SQS (lỗi)
       presigned URL -+      v
                       Supabase PostgreSQL (pooler :6543)

 CloudWatch log/alarm --> SNS (email)     CloudTrail + IAM Access Analyzer
 Tùy chọn: CloudFront /hubs/* --> ALB --> ECS Fargate (SignalR), image từ CodeBuild -> ECR
```

## Kết quả đạt được

- Hoàn thành sơ đồ kiến trúc Student Market và giải thích luồng dữ liệu (luồng API và luồng upload ảnh).
- Lập bảng chức năng và lý do chọn từng dịch vụ AWS so với phương án khác (Lambda so với EC2/Fargate, Supabase so với RDS, Vercel so với Amplify).
- Lập bảng rủi ro: lộ secret, bucket bị public, API bị lạm dụng, xử lý ảnh lỗi, Supabase tự tạm dừng sau 7 ngày không truy cập, chi phí của stack realtime.
- Viết bài tổng kết kiến thức đã hiểu và hướng học tiếp theo.

## Câu hỏi tự kiểm tra

- Vì sao trình duyệt upload thẳng lên S3 bằng presigned URL thay vì gửi ảnh qua API?
- OAC và bucket policy đảm bảo thế nào để chỉ `processed/*` được đọc công khai qua CloudFront?
- Vì sao Lambda API phải dùng Supabase pooler (cổng 6543) thay vì direct connection?
- Vì sao secret được lưu trong SSM Parameter Store thay vì tham số CloudFormation hay `appsettings`?
- Nếu ImageProcessor lỗi, hệ thống quan sát (CloudWatch alarm, hàng đợi SQS, email SNS) và phục hồi thế nào?

## Nguồn học

- [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/)
- [Upload object bằng presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html)
- [Giới hạn truy cập S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/)
- [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
