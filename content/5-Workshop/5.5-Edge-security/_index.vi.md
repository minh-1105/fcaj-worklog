---
title : "Bảo mật ở edge với AWS WAF"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

Đến giờ API vẫn được gọi thẳng vào API Gateway. Ở bước này CloudFront trở thành cổng công khai duy nhất: AWS WAF lọc request, CloudFront chuyển `/api/*` tới API Gateway kèm header bí mật `X-Origin-Verify`, và API từ chối mọi request không có header này.

#### 1. Tạo web ACL WAF (us-east-1)

Web ACL của WAF dành cho CloudFront bắt buộc phải tạo ở **us-east-1**.

```powershell
aws cloudformation deploy --template-file .\aws\iac\edge-waf.yaml `
  --stack-name student-market-dev-edge --profile student-market --region us-east-1 `
  --parameter-overrides Environment=dev SubscribeFreePlan=false IncludeSqliRule=false ApiRateLimitPer5Min=500

$webAcl = (aws cloudformation describe-stacks --stack-name student-market-dev-edge --profile student-market --region us-east-1 `
  --query "Stacks[0].Outputs[?OutputKey=='WebAclArn'].OutputValue" --output text)
```

Web ACL gồm bốn rule: giới hạn 500 request `/api` mỗi 5 phút cho mỗi IP, danh sách IP xấu (Amazon IP reputation), common rule set và known bad inputs.

`SubscribeFreePlan=false` dùng WAF trả theo mức sử dụng (khoảng $9/tháng cho bốn rule). Tài khoản đang ở gói AWS Free không đăng ký được gói CloudFront Free.

#### 2. Gắn WAF và chuyển /api/* qua CloudFront

Deploy lại stack ảnh với web ACL, domain của API và secret origin ở bước 5.4.1:

```powershell
$apiDomain = "<ApiOriginDomain>"
sam deploy --template-file .\.aws-sam\build-media-pipeline\template.yaml `
  --stack-name student-market-dev-media `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev EnableAuditLogging=true AlertEmail=<email-nhan-canh-bao> "FrontendOrigins=http://localhost:3000" `
    WebAclArn=$webAcl ApiOriginDomain=$apiDomain OriginVerifySecret=$originSecret
```

#### 3. Bắt buộc xác minh origin ở API

```powershell
sam deploy --template-file .\.aws-sam\build-api\template.yaml `
  --stack-name student-market-dev `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev MediaStackName=student-market-dev-media `
    SsmParameterPath=/student-market/dev/api SkipDatabaseSeed=true EnforceOriginVerify=true `
    "FrontendOrigins=http://localhost:3000"
```

#### 4. Kiểm tra

```powershell
$cdn = "<CloudFrontBaseUrl>"; $api = "<ApiBaseUrl>"

curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/api/Items/search"          # 200 qua CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$api/api/Items/search"          # 403 ORIGIN_NOT_ALLOWED
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/api/Items/search?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E"   # payload XSS bị WAF chặn (403)
```

Cuối cùng trỏ frontend sang CloudFront (`NEXT_PUBLIC_API_URL=<CloudFrontBaseUrl>/api`) và, khi đã có domain HTTPS thật của frontend, deploy lại **cả hai** stack với `FrontendOrigins=<domain đó>`.
