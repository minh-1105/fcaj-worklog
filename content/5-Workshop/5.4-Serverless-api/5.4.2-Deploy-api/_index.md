---
title : "Deploy the API stack"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Build and deploy

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

| Parameter | Value | Meaning |
|---|---|---|
| `MediaStackName` | `student-market-dev-media` | The stack whose exports are imported with `Fn::ImportValue`. |
| `SsmParameterPath` | `/student-market/dev/api` | Path loaded at cold start (`SSM_CONFIG_PATH`). |
| `SkipDatabaseSeed` | `true` | Migrations were already run manually through the session pooler. |
| `EnforceOriginVerify` | `false` | Turned on in step 5.5, after CloudFront sends the header. |
| `SubnetIds`, `SecurityGroupIds` | empty | Lambda runs outside a VPC — no NAT Gateway needed. |
| `GoogleClientId`, `EmailUsername` | optional | Enable Google sign-in and real OTP emails. Without `EmailUsername`, OTP codes are only written to CloudWatch Logs. |

#### Review the IAM role

Open the API function's execution role in the IAM console. It should only allow:

- `ssm:GetParametersByPath` on `/student-market/dev/api` and `kms:Decrypt` only via `ssm.ap-southeast-1.amazonaws.com`;
- S3 actions on the media bucket needed to presign and confirm uploads;
- writing its own CloudWatch log group.

No `*FullAccess` managed policy should be attached.

Write down the `ApiBaseUrl` and `ApiOriginDomain` outputs.
