---
title : "Lưu secret vào SSM Parameter Store"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Mỗi SecureString nằm dưới path `/student-market/dev/api` sẽ trở thành một khóa cấu hình của API (dấu `/` tương ứng với `:`).

| Tên parameter | Khóa cấu hình | Giá trị |
|---|---|---|
| `/student-market/dev/api/ConnectionStrings/DefaultConnection` | `ConnectionStrings:DefaultConnection` | Connection string **transaction** pooler của Supabase (cổng 6543, `No Reset On Close=true`) |
| `/student-market/dev/api/Jwt/SecretKey` | `Jwt:SecretKey` | Chuỗi ngẫu nhiên, tối thiểu 32 ký tự |
| `/student-market/dev/api/OriginVerify/Secret` | `OriginVerify:Secret` | Chuỗi ngẫu nhiên dùng chung với CloudFront (bước 5.5) |
| `/student-market/dev/api/Email/Password` | `Email:Password` | Tùy chọn: Gmail App Password để gửi email OTP |

#### Tạo parameter

```powershell
function New-Secret { -join ((48..57 + 65..90 + 97..122) | Get-Random -Count 48 | ForEach-Object { [char]$_ }) }
$path = "/student-market/dev/api"

aws ssm put-parameter --name "$path/ConnectionStrings/DefaultConnection" --type SecureString `
  --value "<connection string transaction pooler>" --overwrite --profile student-market --region ap-southeast-1
aws ssm put-parameter --name "$path/Jwt/SecretKey" --type SecureString --value (New-Secret) --overwrite --profile student-market --region ap-southeast-1
$originSecret = New-Secret
aws ssm put-parameter --name "$path/OriginVerify/Secret" --type SecureString --value $originSecret --overwrite --profile student-market --region ap-southeast-1
```

Giữ biến `$originSecret` trong phiên terminal này; biến này sẽ được truyền cho stack ảnh ở bước 5.5.

#### Kiểm tra mà không làm lộ giá trị

```powershell
aws ssm get-parameters-by-path --path $path --recursive --profile student-market --region ap-southeast-1 `
  --query "Parameters[].{Name:Name,Type:Type}" --output table
```

Mọi parameter phải có kiểu `SecureString`. Tránh dùng `--with-decryption` nếu không thật sự cần đọc giá trị.
