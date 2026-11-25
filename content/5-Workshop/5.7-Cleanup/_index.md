---
title : "Clean up resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Delete the resources in the reverse order of creation. The media bucket is **versioned**, so it must be emptied (including all versions) before its stack can be deleted.

#### 1. Delete the API stack

```powershell
aws cloudformation delete-stack --stack-name student-market-dev --profile student-market --region ap-southeast-1
aws cloudformation wait stack-delete-complete --stack-name student-market-dev --profile student-market --region ap-southeast-1
```

#### 2. Empty the media bucket and delete the media stack

Use the S3 console: select the bucket → **Empty** → type `permanently delete`. Do the same for the CloudTrail/audit log bucket if the stack created one. Then:

```powershell
aws cloudformation delete-stack --stack-name student-market-dev-media --profile student-market --region ap-southeast-1
aws cloudformation wait stack-delete-complete --stack-name student-market-dev-media --profile student-market --region ap-southeast-1
```

Deleting the CloudFront distribution takes several minutes because it must be disabled first.

#### 3. Delete the WAF stack (us-east-1)

```powershell
aws cloudformation delete-stack --stack-name student-market-dev-edge --profile student-market --region us-east-1
```

#### 4. Delete the SSM parameters

```powershell
aws ssm get-parameters-by-path --path /student-market/dev/api --recursive --profile student-market --region ap-southeast-1 `
  --query "Parameters[].Name" --output text |
  ForEach-Object { $_ -split "\s+" } | Where-Object { $_ } |
  ForEach-Object { aws ssm delete-parameter --name $_ --profile student-market --region ap-southeast-1 }
```

#### 5. Final checks

```powershell
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE --profile student-market --region ap-southeast-1 `
  --query "StackSummaries[?starts_with(StackName,'student-market')].StackName"
aws logs describe-log-groups --log-group-name-prefix /aws/lambda/student-market --profile student-market --region ap-southeast-1 `
  --query "logGroups[].logGroupName"
```

Delete any leftover log groups, and remove the SAM artifact stack `aws-sam-cli-managed-default` if SAM is no longer used. The Supabase database and the Vercel project are outside AWS and are not affected.
