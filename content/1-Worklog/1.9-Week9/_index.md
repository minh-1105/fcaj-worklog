---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Week 9 — Monitoring, Logging, and Auditing

## Weekly objective

Distinguish workload observability from auditing API activity in an account.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Metrics, logs, traces, events, and audit records. |
| 2 | CloudWatch metrics, logs, dashboards, alarms, and log retention. |
| 3 | Lambda/API Gateway metrics: invocations, errors, duration, latency, and throttles. |
| 4 | CloudTrail Event history, management events, data events, and trails. |
| 5 | A triage runbook for 5xx errors, authorization failures, timeouts, and root-cause analysis. |

## Setup to understand

Review where to find CloudWatch Logs/Alarms and CloudTrail Event history. No alarm or trail needs to be created.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws logs describe-log-groups
aws cloudwatch describe-alarms
aws cloudtrail lookup-events --max-results 5
```

## Learning outcomes

- Compared CloudWatch and CloudTrail.
- Wrote a runbook for triaging 5xx errors.
- Drew the observability flow for a request through API Gateway and Lambda.
- Explained management events and data events.

## Self-review questions

- Does CloudTrail replace application logs?
- Does Event history include S3 object-level events by default?
- How do metrics, logs, and traces answer different questions?

## Learning resources

- [CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [What is CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
