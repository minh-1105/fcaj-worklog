---
title : "Monitoring and auditing"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### CloudWatch alarms

Both stacks create alarms that publish to the same SNS topic:

| Alarm source | What it watches |
|---|---|
| API Lambda | Errors |
| ImageProcessor Lambda | Errors, throttles, p99 duration |
| SQS failure queue | Messages waiting (failed image events) |

```powershell
aws cloudwatch describe-alarms --profile student-market --region ap-southeast-1 `
  --query "MetricAlarms[].{Name:AlarmName,State:StateValue}" --output table
```

To test the notification path, temporarily force one alarm into `ALARM` and check the alert inbox:

```powershell
aws cloudwatch set-alarm-state --alarm-name "<alarm name>" --state-value ALARM `
  --state-reason "workshop test" --profile student-market --region ap-southeast-1
```

#### Logs

Log groups have a bounded retention (14 days by default). Use CloudWatch Logs Insights on the API log group to find errors:

```text
fields @timestamp, @message
| filter @message like /error|Error|ERROR/
| sort @timestamp desc
| limit 20
```

#### CloudTrail and IAM Access Analyzer

- **CloudTrail** (created because `EnableAuditLogging=true`) records management events in all Regions (who changed IAM, stacks, buckets). Open *CloudTrail → Event history* and filter by `Event source = cloudformation.amazonaws.com` to see the deployments.
- **IAM Access Analyzer** (`student-market-dev-account`) reports resources shared outside the account. Open *IAM → Access analyzer* and confirm there are no unexpected active findings — the media bucket must not appear as public.

#### Cost

Open *Billing → Cost Explorer*, group by **Service**, and compare with the AWS Budget. WAF should be the only noticeable line item.
