---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# Week 7 — Serverless: Lambda, API Gateway, and Event-Driven Architecture

## Weekly objective

Understand HTTP API architectures and event processing without managing servers.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Serverless, event-driven systems, and the Lambda execution environment. |
| 2 | Lambda handlers, event/context, timeout, memory, cold starts, and concurrency. |
| 3 | API Gateway HTTP APIs, routes, integrations, authentication/authorization, and CORS. |
| 4 | Lambda execution roles, least privilege, and using an AWS SDK with DynamoDB. |
| 5 | Error handling, retries, idempotency, and synchronous versus asynchronous invocation. |

## Setup to understand

Review the sequence for creating a Lambda function, execution role, HTTP API, route/integration, and viewing CloudWatch Logs. Deployment is not required.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws lambda list-functions
aws lambda get-function --function-name FUNCTION_NAME
aws apigatewayv2 get-apis
```

## Learning outcomes

- Drew the sequence `Client → API Gateway → Lambda → DynamoDB`.
- Distinguished 4xx and 5xx errors.
- Explained `dynamodb:PutItem` and `dynamodb:Query` permissions.
- Distinguished Lambda calling DynamoDB from DynamoDB Streams invoking Lambda.

## Self-review questions

- Why does Lambda need an execution role?
- What problem does CORS address?
- Can retries create duplicate records?

## Learning resources

- [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
- [API Gateway HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html)
