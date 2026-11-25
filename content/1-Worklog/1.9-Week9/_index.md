---
title: "Week 9 Worklog"
date: 2026-11-05
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Week 9 — Monitoring, Logging, and Auditing

**Timeline:** 05/11/2026 – 10/11/2026

## Weekly objective

Distinguish workload observability from auditing API activity in an account.

## Learning plan

| Session | Date | Topic |
|---:|---|---|
| 1 | Thu, 05/11/2026 | Metrics, logs, traces, events, and audit records. |
| 2 | Fri, 06/11/2026 | CloudWatch metrics, logs, dashboards, alarms, and log retention. |
| 3 | Sat, 07/11/2026 | Lambda/API Gateway metrics: invocations, errors, duration, latency, and throttles. |
| 4 | Mon, 09/11/2026 | CloudTrail Event history, management events, data events, and trails. |
| 5 | Tue, 10/11/2026 | A triage runbook for 5xx errors, authorization failures, timeouts, and root-cause analysis. |

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
