---
title : "Prerequisites"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Tools

Install the following on the local machine (Windows PowerShell is used in all commands):

| Tool | Purpose | Check |
|---|---|---|
| AWS CLI v2 | Call AWS APIs | `aws --version` |
| AWS SAM CLI | Build and deploy the templates | `sam --version` |
| .NET 8 SDK | Build the Lambda functions | `dotnet --version` |
| Git | Get the source code | `git --version` |
| curl | Test HTTP endpoints | `curl.exe --version` |

#### AWS account and permissions

1. Use an IAM user (not the root user) dedicated to deployment. It needs permissions for CloudFormation, S3, Lambda, API Gateway, IAM roles, SQS, SNS, CloudWatch, CloudFront, WAF, SSM, KMS (through SSM), CloudTrail, and IAM Access Analyzer.
2. Configure a named profile and use the **ap-southeast-1** Region:

   ```powershell
   aws configure --profile student-market
   aws sts get-caller-identity --profile student-market
   ```

3. Create an **AWS Budget** (for example $10/month with an email alert) before deploying anything.

#### Database

Create a free **Supabase** project in the Singapore Region and run the EF Core migrations from the local machine with the **session** pooler connection string:

```powershell
cd student-flea-market-be
$env:ConnectionStrings__DefaultConnection = "<session pooler connection string>"
dotnet ef database update --project .\StudentFleaMarket.Infrastructure --startup-project .\StudentFleaMarket.API
```

Keep the **transaction** pooler connection string (port `6543`) for the API in step 5.4. Lambda runs outside a VPC, so it must use the pooler (the direct connection is IPv6-only).

#### Source code

Open a terminal at the root of the Student Market repository. All later commands are run from there.

```powershell
sam validate --lint --template-file .\aws\iac\media-pipeline.yaml
sam validate --lint --template-file .\aws\iac\api.yaml
```
