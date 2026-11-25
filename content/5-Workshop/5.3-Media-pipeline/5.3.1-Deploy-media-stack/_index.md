---
title : "Deploy the media stack"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Build and deploy

```powershell
sam build --template-file .\aws\iac\media-pipeline.yaml --build-dir .\.aws-sam\build-media-pipeline

sam deploy --template-file .\.aws-sam\build-media-pipeline\template.yaml `
  --stack-name student-market-dev-media `
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM --resolve-s3 `
  --profile student-market --region ap-southeast-1 `
  --parameter-overrides Environment=dev EnableAuditLogging=true AlertEmail=<alert-email> `
    "FrontendOrigins=http://localhost:3000"
```

Main parameters:

| Parameter | Value in this step | Meaning |
|---|---|---|
| `FrontendOrigins` | `http://localhost:3000` | Origins allowed to POST to the bucket (CORS). Updated later with the real frontend domain. |
| `AlertEmail` | Alert email address | Subscribed to the SNS alert topic. |
| `EnableAuditLogging` | `true` | Creates the CloudTrail trail (management events + S3 data events) and S3 access logs in a separate audit bucket. Adds a small storage cost. |
| `RawRetentionDays` | `7` (default) | Days to keep original uploads after the WebP variants exist. |
| `WebAclArn`, `ApiOriginDomain`, `OriginVerifySecret` | Empty for now | Filled in at step 5.5. |

CloudFront takes several minutes to create. Then **confirm the SNS subscription** from the email AWS sends; otherwise alarm notifications are not delivered.

#### Read the outputs

```powershell
aws cloudformation describe-stacks --stack-name student-market-dev-media `
  --profile student-market --region ap-southeast-1 --query "Stacks[0].Outputs" --output table
```

Write down `MediaBucketName`, `CloudFrontBaseUrl`, `CloudFrontDistributionArn`, and `AlertTopicArn`.

#### Check the security settings

```powershell
$bucket = "<MediaBucketName>"
aws s3api get-public-access-block --bucket $bucket --profile student-market --region ap-southeast-1
aws s3api get-bucket-versioning --bucket $bucket --profile student-market --region ap-southeast-1
aws s3api get-bucket-encryption --bucket $bucket --profile student-market --region ap-southeast-1
```

All four Block Public Access settings must be `true`, versioning `Enabled`, and default encryption configured.
