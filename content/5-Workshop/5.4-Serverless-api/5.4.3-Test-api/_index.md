---
title : "Test the API"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

```powershell
$api = "<ApiBaseUrl>"

# Health check - expect {"status":"ok"}
curl.exe -s "$api/health"

# Public endpoints - expect 200 with JSON
curl.exe -s "$api/api/Items/categories"
curl.exe -s "$api/api/Items/search"

# Protected endpoint without a token - expect 401
curl.exe -s -o NUL -w "%{http_code}`n" -X POST "$api/api/Items/sell/00000000-0000-0000-0000-000000000000/images/presign"
```

#### Cold start and logs

The first request after a deployment is slower because Lambda starts the .NET runtime and loads the SSM parameters. Look at the logs:

```powershell
$fn = (aws cloudformation describe-stacks --stack-name student-market-dev --profile student-market --region ap-southeast-1 `
  --query "Stacks[0].Outputs[?OutputKey=='ApiFunctionName'].OutputValue" --output text)
aws logs tail "/aws/lambda/$fn" --since 10m --profile student-market --region ap-southeast-1
```

Check that the logs contain **no** passwords, JWTs, or full presigned URLs.

#### Connect the frontend

Register a user from the frontend (locally `http://localhost:3000`, with `NEXT_PUBLIC_API_URL=<ApiBaseUrl>/api`), create a listing, and upload an image. The whole flow is: presign → POST to S3 → confirm → image displayed from CloudFront.
