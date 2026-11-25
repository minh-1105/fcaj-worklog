---
title : "Test the image pipeline"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

In production the browser uploads through a presigned POST from the API. Here the upload is simulated with the CLI, using the same key format: `raw/users/{userId}/items/{itemId}/{uploadId}.{ext}`.

#### Upload a test image

```powershell
$bucket   = "<MediaBucketName>"
$cdn      = "<CloudFrontBaseUrl>"
$userId   = [guid]::NewGuid(); $itemId = [guid]::NewGuid(); $uploadId = [guid]::NewGuid()

aws s3 cp .\test.jpg "s3://$bucket/raw/users/$userId/items/$itemId/$uploadId.jpg" `
  --content-type image/jpeg --profile student-market --region ap-southeast-1
```

#### Check the result

After a few seconds the processed variants should exist:

```powershell
aws s3 ls "s3://$bucket/processed/items/$itemId/$uploadId/" --profile student-market --region ap-southeast-1
```

Expected: `large.webp` and `thumbnail.webp`. Then verify access through CloudFront and S3:

```powershell
# 200 - processed image through CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/processed/items/$itemId/$uploadId/thumbnail.webp"

# 403 - raw/ is never readable through CloudFront
curl.exe -s -o NUL -w "%{http_code}`n" "$cdn/raw/users/$userId/items/$itemId/$uploadId.jpg"
```

#### Look at the logs and the failure queue

```powershell
$fn = (aws cloudformation describe-stacks --stack-name student-market-dev-media --profile student-market --region ap-southeast-1 `
  --query "Stacks[0].Outputs[?OutputKey=='ImageProcessorFunctionName'].OutputValue" --output text)
aws logs tail "/aws/lambda/$fn" --since 10m --profile student-market --region ap-southeast-1
```

Upload a text file renamed to `.jpg` and check that the invocation fails, is retried, and finally lands in the SQS failure queue (CloudWatch alarm on queue depth → SNS email).
