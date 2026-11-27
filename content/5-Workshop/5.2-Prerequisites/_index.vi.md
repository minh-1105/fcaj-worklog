---
title : "Chuẩn bị"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Công cụ

Cài các công cụ sau trên máy (mọi lệnh trong workshop dùng Windows PowerShell):

| Công cụ | Mục đích | Kiểm tra |
|---|---|---|
| AWS CLI v2 | Gọi AWS API | `aws --version` |
| AWS SAM CLI | Build và deploy template | `sam --version` |
| .NET 8 SDK | Build các hàm Lambda | `dotnet --version` |
| Git | Lấy source code | `git --version` |
| curl | Kiểm tra HTTP endpoint | `curl.exe --version` |

#### Tài khoản AWS và quyền

1. Dùng một IAM user riêng để deploy (không dùng root user). User cần quyền với CloudFormation, S3, Lambda, API Gateway, IAM role, SQS, SNS, CloudWatch, CloudFront, WAF, SSM, KMS (thông qua SSM), CloudTrail và IAM Access Analyzer.
2. Cấu hình profile và dùng Region **ap-southeast-1**:

   ```powershell
   aws configure --profile student-market
   aws sts get-caller-identity --profile student-market
   ```

3. Tạo **AWS Budget** (ví dụ $10/tháng, có email cảnh báo) trước khi deploy bất cứ thứ gì.

#### Database

Tạo project **Supabase** miễn phí ở region Singapore và chạy migration EF Core từ máy bằng connection string **session** pooler:

```powershell
cd student-flea-market-be
$env:ConnectionStrings__DefaultConnection = "<connection string session pooler>"
dotnet ef database update --project .\StudentFleaMarket.Infrastructure --startup-project .\StudentFleaMarket.API
```

Giữ lại connection string **transaction** pooler (cổng `6543`) cho API ở bước 5.4. Lambda chạy ngoài VPC nên phải dùng pooler (direct connection chỉ có IPv6).

#### Source code

Mở terminal ở thư mục gốc của repository Student Market. Mọi lệnh phía sau đều chạy từ đây.

```powershell
sam validate --lint --template-file .\aws\iac\media-pipeline.yaml
sam validate --lint --template-file .\aws\iac\api.yaml
```
