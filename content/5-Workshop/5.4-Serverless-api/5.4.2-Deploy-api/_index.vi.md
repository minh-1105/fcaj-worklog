---
title : "Deploy stack API"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Build và deploy

```powershell
sam build --template-file .\aws\iac\api.yaml --build-dir .\.aws-sam\build-api

sam deploy --template-file .\.aws-sam\build-api\template.yaml `
  --stack-name student-market-dev `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 `
  --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev MediaStackName=student-market-dev-media `
    SsmParameterPath=/student-market/dev/api SkipDatabaseSeed=true EnforceOriginVerify=false `
    "FrontendOrigins=http://localhost:3000"
```

| Tham số | Giá trị | Ý nghĩa |
|---|---|---|
| `MediaStackName` | `student-market-dev-media` | Stack có các export được import bằng `Fn::ImportValue`. |
| `SsmParameterPath` | `/student-market/dev/api` | Path được nạp khi cold start (`SSM_CONFIG_PATH`). |
| `SkipDatabaseSeed` | `true` | Migration đã chạy tay qua session pooler. |
| `EnforceOriginVerify` | `false` | Bật ở bước 5.5, sau khi CloudFront đã gửi header. |
| `SubnetIds`, `SecurityGroupIds` | để trống | Lambda chạy ngoài VPC — không cần NAT Gateway. |
| `GoogleClientId`, `EmailUsername` | tùy chọn | Bật đăng nhập Google và gửi email OTP thật. Không có `EmailUsername` thì mã OTP chỉ được ghi vào CloudWatch Logs. |

#### Rà soát IAM role

Mở execution role của hàm API trong IAM console. Role chỉ nên cho phép:

- `ssm:GetParametersByPath` trên `/student-market/dev/api` và `kms:Decrypt` chỉ khi đi qua `ssm.ap-southeast-1.amazonaws.com`;
- các thao tác S3 trên media bucket cần cho presign và xác nhận upload;
- ghi vào CloudWatch log group của chính nó.

Không được gắn managed policy dạng `*FullAccess`.

Ghi lại output `ApiBaseUrl` và `ApiOriginDomain`.
