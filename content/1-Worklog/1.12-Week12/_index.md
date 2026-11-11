---
title: "Week 12 Worklog"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Week 12 — Theory Capstone: IoT Weather Platform

## Weekly objective

Consolidate the course by designing, explaining, and reviewing an IoT architecture without deploying it to AWS.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Define functional and non-functional requirements for the weather platform. |
| 2 | Design the ingest flow: Device → IoT Core → S3 and Lambda → DynamoDB. |
| 3 | Design the read API: Client → API Gateway → Lambda → DynamoDB. |
| 4 | Analyze IAM, failure modes, CloudWatch, cost estimates, and a hypothetical cleanup plan. |
| 5 | Complete the diagrams, ADRs, architecture explanation, and review questions. |

## Setup to understand

Do not deploy resources. Use documentation, diagrams, and AWS Pricing Calculator to analyze the logical architecture.

## Reference model

```text
Device Simulator
      |
      | MQTT
      v
 AWS IoT Core
      |
      v
   IoT Rule
   /      \
  v        v
 S3     Lambda (validate)
Raw data    |
            v
         DynamoDB
            ^
            |
         Lambda
            ^
            |
       API Gateway
            ^
            |
          Client
```

## Learning outcomes

- Completed an architecture diagram and data-flow explanation.
- Prepared a table describing each service and why it was selected.
- Analyzed risks including network loss, duplicate data, permission errors, cost, and certificate exposure.
- Wrote a final reflection and next-learning plan.

## Self-review questions

- How should duplicate telemetry be handled?
- Which components need IAM roles and which use IoT policies?
- If Lambda fails to write to DynamoDB, how should the system observe and recover?

## Learning resources

- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/)
- [AWS IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
