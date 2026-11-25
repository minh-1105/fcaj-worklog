---
title : "Kiểm tra API"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

```powershell
$api = "<ApiBaseUrl>"

# Health check - mong đợi {"status":"ok"}
curl.exe -s "$api/health"

# Endpoint công khai - mong đợi 200 kèm JSON
curl.exe -s "$api/api/Items/categories"
curl.exe -s "$api/api/Items/search"

# Endpoint cần đăng nhập nhưng không gửi token - mong đợi 401
curl.exe -s -o NUL -w "%{http_code}`n" -X POST "$api/api/Items/sell/00000000-0000-0000-0000-000000000000/images/presign"
```

#### Cold start và log

Request đầu tiên sau khi deploy chậm hơn vì Lambda khởi động runtime .NET và nạp parameter từ SSM. Xem log:

```powershell
$fn = (aws cloudformation describe-stacks --stack-name student-market-dev --profile student-market --region ap-southeast-1 `
  --query "Stacks[0].Outputs[?OutputKey=='ApiFunctionName'].OutputValue" --output text)
aws logs tail "/aws/lambda/$fn" --since 10m --profile student-market --region ap-southeast-1
```

Kiểm tra log **không** chứa mật khẩu, JWT hay presigned URL đầy đủ.

#### Kết nối frontend

Đăng ký tài khoản từ frontend (chạy local `http://localhost:3000` với `NEXT_PUBLIC_API_URL=<ApiBaseUrl>/api`), tạo tin đăng và upload ảnh. Toàn bộ luồng là: presign → POST lên S3 → confirm → ảnh hiển thị qua CloudFront.
