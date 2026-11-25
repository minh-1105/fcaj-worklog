---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Xóa tài nguyên theo thứ tự ngược với lúc tạo. Media bucket có bật **versioning**, nên phải làm rỗng (kể cả mọi version) trước khi xóa stack.

#### 1. Xóa stack API

```powershell
aws cloudformation delete-stack --stack-name student-market-dev --profile student-market --region ap-southeast-1
aws cloudformation wait stack-delete-complete --stack-name student-market-dev --profile student-market --region ap-southeast-1
```

#### 2. Làm rỗng media bucket và xóa stack ảnh

Cách đơn giản nhất là dùng S3 console: chọn bucket → **Empty** → gõ `permanently delete`. Làm tương tự với bucket log CloudTrail/audit nếu stack có tạo. Sau đó:

```powershell
aws cloudformation delete-stack --stack-name student-market-dev-media --profile student-market --region ap-southeast-1
aws cloudformation wait stack-delete-complete --stack-name student-market-dev-media --profile student-market --region ap-southeast-1
```

Việc xóa CloudFront distribution mất vài phút vì distribution phải được disable trước.

#### 3. Xóa stack WAF (us-east-1)

```powershell
aws cloudformation delete-stack --stack-name student-market-dev-edge --profile student-market --region us-east-1
```

#### 4. Xóa parameter trong SSM

```powershell
aws ssm get-parameters-by-path --path /student-market/dev/api --recursive --profile student-market --region ap-southeast-1 `
  --query "Parameters[].Name" --output text |
  ForEach-Object { $_ -split "\s+" } | Where-Object { $_ } |
  ForEach-Object { aws ssm delete-parameter --name $_ --profile student-market --region ap-southeast-1 }
```

#### 5. Kiểm tra lần cuối

```powershell
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE --profile student-market --region ap-southeast-1 `
  --query "StackSummaries[?starts_with(StackName,'student-market')].StackName"
aws logs describe-log-groups --log-group-name-prefix /aws/lambda/student-market --profile student-market --region ap-southeast-1 `
  --query "logGroups[].logGroupName"
```

Xóa các log group còn sót và xóa stack lưu artifact của SAM `aws-sam-cli-managed-default` nếu không còn dùng SAM. Database Supabase và project Vercel nằm ngoài AWS nên không bị ảnh hưởng.
