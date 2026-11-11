---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Tuần 10 — AWS IoT Core, MQTT và Data Pipeline

## Mục tiêu tuần

Hiểu cách telemetry đi từ thiết bị vào các dịch vụ lưu trữ và xử lý trên AWS.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | IoT concepts, MQTT, broker, publish/subscribe, topic và QoS. |
| 2 | AWS IoT Thing, X.509 certificate, IoT policy, client ID và topic authorization. |
| 3 | IoT Rule, rule action, IAM role và luồng telemetry vào S3 hoặc Lambda. |
| 4 | S3 raw data, object key design, retry/idempotency và small-files problem. |
| 5 | Glue Data Catalog, crawler, ETL và sự khác biệt với S3 storage. |

## Setup cần biết

Đọc quy trình đăng ký Thing, certificate, policy và dùng MQTT test client. Không tạo certificate hoặc thiết bị thật trong phần bắt buộc.

## Mô hình tham khảo

```text
weather/station-01/telemetry

{
  "station_id": "station-01",
  "timestamp": "2026-11-05T07:00:00Z",
  "temperature_c": 29.4,
  "humidity_percent": 74
}
```

## Kết quả đạt được

- Vẽ luồng `Device → IoT Core → Rule → S3`.
- Vẽ luồng `Device → IoT Core → Rule → Lambda → DynamoDB`.
- Giải thích vai trò của IoT policy, IAM role và Glue crawler.
- Thiết kế topic và JSON payload cho trạm thời tiết.

## Câu hỏi tự kiểm tra

- IoT policy khác IAM policy như thế nào?
- Vì sao phải giới hạn quyền publish/subscribe theo topic?
- Glue crawler lưu dữ liệu hay metadata?

## Nguồn học

- [AWS IoT Core Overview](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html)
- [IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html)
- [AWS IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)
- [AWS Glue Overview](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)
- [Glue Crawlers](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)
