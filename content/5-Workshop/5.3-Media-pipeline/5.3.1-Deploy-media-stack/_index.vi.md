---
title : "Deploy stack ảnh"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Build và deploy

```powershell
sam build --template-file .\aws\iac\media-pipeline.yaml --build-dir .\.aws-sam\build-media-pipeline

sam deploy --template-file .\.aws-sam\build-media-pipeline\template.yaml `
  --stack-name student-market-dev-media `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 `
  --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev EnableAuditLogging=true AlertEmail=<email-nhan-canh-bao> `
    "FrontendOrigins=http://localhost:3000"
```

Các tham số chính:

| Tham số | Giá trị ở bước này | Ý nghĩa |
|---|---|---|
| `FrontendOrigins` | `http://localhost:3000` | Origin được phép POST lên bucket (CORS). Cập nhật sau bằng domain frontend thật. |
| `AlertEmail` | Email nhận cảnh báo | Đăng ký nhận cảnh báo từ SNS topic. |
| `EnableAuditLogging` | `true` | Tạo CloudTrail trail (management events + S3 data events) và S3 access log trong một bucket audit riêng. Tốn thêm một ít chi phí lưu trữ. |
| `RawRetentionDays` | `7` (mặc định) | Số ngày giữ ảnh gốc sau khi đã có bản WebP. |
| `WebAclArn`, `ApiOriginDomain`, `OriginVerifySecret` | Để trống | Sẽ điền ở bước 5.5. |

CloudFront mất vài phút để tạo. Sau đó **xác nhận đăng ký SNS** trong email AWS gửi tới, nếu không cảnh báo sẽ không được gửi đi.

#### Đọc output

```powershell
aws cloudformation describe-stacks --stack-name student-market-dev-media `
  --profile student-market --region ap-southeast-1 --query "Stacks[0].Outputs" --output table
```

Ghi lại `MediaBucketName`, `CloudFrontBaseUrl`, `CloudFrontDistributionArn` và `AlertTopicArn`.

#### Kiểm tra cấu hình bảo mật

```powershell
$bucket = "<MediaBucketName>"
aws s3api get-public-access-block --bucket $bucket --profile student-market --region ap-southeast-1
aws s3api get-bucket-versioning --bucket $bucket --profile student-market --region ap-southeast-1
aws s3api get-bucket-encryption --bucket $bucket --profile student-market --region ap-southeast-1
```

Cả bốn thiết lập Block Public Access phải là `true`, versioning là `Enabled` và có default encryption.
