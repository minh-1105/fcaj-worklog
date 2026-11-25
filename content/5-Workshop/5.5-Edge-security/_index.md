---
title : "Edge security with AWS WAF"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

So far the API is called directly on API Gateway. In this step CloudFront becomes the only public entry point: AWS WAF filters requests, CloudFront forwards `/api/*` to API Gateway with a secret `X-Origin-Verify` header, and the API rejects calls that do not carry it.

#### 1. Create the WAF web ACL (us-east-1)

WAF web ACLs for CloudFront must be created in **us-east-1**.

```powershell
aws cloudformation deploy --template-file .\aws\iac\edge-waf.yaml `
  --stack-name student-market-dev-edge --profile student-market --region us-east-1 `
  --parameter-overrides Environment=dev SubscribeFreePlan=false IncludeSqliRule=false ApiRateLimitPer5Min=500

$webAcl = (aws cloudformation describe-stacks --stack-name student-market-dev-edge --profile student-market --region us-east-1 `
  --query "Stacks[0].Outputs[?OutputKey=='WebAclArn'].OutputValue" --output text)
```

The web ACL has four rules: a per-IP rate limit of 500 `/api` requests per 5 minutes, the Amazon IP reputation list, the common rule set, and known bad inputs.

`SubscribeFreePlan=false` keeps usage-based WAF pricing (about $9/month for four rules). The CloudFront flat-rate Free plan cannot be subscribed from an account on the AWS Free account plan.

#### 2. Attach WAF and route /api/* through CloudFront

Redeploy the media stack with the web ACL, the API domain, and the origin secret from step 5.4.1:

```powershell
$apiDomain = "<ApiOriginDomain>"
sam deploy --template-file .\.aws-sam\build-media-pipeline\template.yaml `
  --stack-name student-market-dev-media `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev EnableAuditLogging=true AlertEmail=<alert-email> "FrontendOrigins=http://localhost:3000" `
    WebAclArn=$webAcl ApiOriginDomain=$apiDomain OriginVerifySecret=$originSecret
```

#### 3. Enforce origin verification on the API

```powershell
sam deploy --template-file .\.aws-sam\build-api\template.yaml `
  --stack-name student-market-dev `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev MediaStackName=student-market-dev-media `
    SsmParameterPath=/student-market/dev/api SkipDatabaseSeed=true EnforceOriginVerify=true `
    "FrontendOrigins=http://localhost:3000"
```

#### 4. Test

```powershell
$cdn = "<CloudFrontBaseUrl>"; $api = "<ApiBaseUrl>"

curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/api/Items/search"          # 200 through CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$api/api/Items/search"          # 403 ORIGIN_NOT_ALLOWED
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/api/Items/search?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E"   # XSS payload blocked by WAF (403)
```

Finally point the frontend to CloudFront (`NEXT_PUBLIC_API_URL=<CloudFrontBaseUrl>/api`) and, once the real HTTPS frontend domain is available, redeploy **both** stacks with `FrontendOrigins=<that domain>`.
