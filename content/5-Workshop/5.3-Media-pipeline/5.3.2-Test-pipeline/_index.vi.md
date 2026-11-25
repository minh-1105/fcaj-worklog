---
title : "Kiểm tra pipeline ảnh"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

Trên production, trình duyệt upload qua presigned POST do API cấp. Ở đây việc upload được mô phỏng bằng CLI với đúng định dạng key: `raw/users/{userId}/items/{itemId}/{uploadId}.{ext}`.

#### Upload ảnh thử

```powershell
$bucket   = "<MediaBucketName>"
$cdn      = "<CloudFrontBaseUrl>"
$userId   = [guid]::NewGuid(); $itemId = [guid]::NewGuid(); $uploadId = [guid]::NewGuid()

aws s3 cp .\test.jpg "s3://$bucket/raw/users/$userId/items/$itemId/$uploadId.jpg" `
  --content-type image/jpeg --profile student-market --region ap-southeast-1
```

#### Kiểm tra kết quả

Sau vài giây, các bản ảnh đã xử lý phải xuất hiện:

```powershell
aws s3 ls "s3://$bucket/processed/items/$itemId/$uploadId/" --profile student-market --region ap-southeast-1
```

Kết quả mong đợi: `large.webp` và `thumbnail.webp`. Sau đó kiểm tra quyền truy cập qua CloudFront:

```powershell
# 200 - ảnh processed đọc được qua CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/processed/items/$itemId/$uploadId/thumbnail.webp"

# 403 - raw/ không bao giờ đọc được qua CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/raw/users/$userId/items/$itemId/$uploadId.jpg"
```

#### Xem log và hàng đợi lỗi

```powershell
$fn = (aws cloudformation describe-stacks --stack-name student-market-dev-media --profile student-market --region ap-southeast-1 `
  --query "Stacks[0].Outputs[?OutputKey=='ImageProcessorFunctionName'].OutputValue" --output text)
aws logs tail "/aws/lambda/$fn" --since 10m --profile student-market --region ap-southeast-1
```

Upload một file text đổi đuôi thành `.jpg` và kiểm tra: lần chạy bị lỗi, được retry, cuối cùng vào hàng đợi lỗi SQS (CloudWatch alarm theo độ dài hàng đợi → email SNS).
