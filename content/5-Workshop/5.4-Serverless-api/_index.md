---
title : "Serverless API: SSM, Lambda, and API Gateway"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

The whole ASP.NET Core API runs as one Lambda function (`AddAWSLambdaHosting(LambdaEventSource.HttpApi)`) behind an API Gateway HTTP API. At cold start it loads its secrets from SSM Parameter Store, so the template contains no secret values.

#### Why this design

- **HTTP API** is enough: REST/JSON routing, CORS, and throttling (20 requests/s, burst 50), without API keys or usage plans.
- **Lambda on demand** costs nothing when there is no traffic.
- **SSM SecureString** keeps secrets encrypted with the AWS managed key `aws/ssm`; the Lambda role can only read its own path and decrypt through SSM.

#### Contents

1. [Store secrets in SSM Parameter Store](5.4.1-store-secrets/)
2. [Deploy the API stack](5.4.2-deploy-api/)
3. [Test the API](5.4.3-test-api/)
