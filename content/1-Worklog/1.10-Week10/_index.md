---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Week 10 — AWS IoT Core, MQTT, and Data Pipelines

## Weekly objective

Understand how telemetry moves from devices into AWS storage and processing services.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | IoT concepts, MQTT, brokers, publish/subscribe, topics, and QoS. |
| 2 | AWS IoT Things, X.509 certificates, IoT policies, client IDs, and topic authorization. |
| 3 | IoT Rules, rule actions, IAM roles, and telemetry flows into S3 or Lambda. |
| 4 | Raw data in S3, object-key design, retries/idempotency, and the small-files problem. |
| 5 | Glue Data Catalog, crawlers, ETL, and how Glue differs from S3 storage. |

## Setup to understand

Review how to register a Thing, certificate, and policy and use the MQTT test client. A real certificate or device is not required.

## Reference model

```text
weather/station-01/telemetry

{
  "station_id": "station-01",
  "timestamp": "2026-11-05T07:00:00Z",
  "temperature_c": 29.4,
  "humidity_percent": 74
}
```

## Learning outcomes

- Drew the flow `Device → IoT Core → Rule → S3`.
- Drew the flow `Device → IoT Core → Rule → Lambda → DynamoDB`.
- Explained the roles of IoT policies, IAM roles, and Glue crawlers.
- Designed a topic and JSON payload for a weather station.

## Self-review questions

- How is an IoT policy different from an IAM policy?
- Why should publish/subscribe permissions be restricted by topic?
- Does a Glue crawler store data or metadata?

## Learning resources

- [AWS IoT Core Overview](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html)
- [IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html)
- [AWS IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)
- [AWS Glue Overview](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)
- [Glue Crawlers](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)
