---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Tuần 9 — Monitoring, Logging và Auditing

## Mục tiêu tuần

Phân biệt quan sát sức khỏe workload với theo dõi các hành động API trong tài khoản.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Metrics, logs, traces, events và audit records. |
| 2 | CloudWatch metrics, logs, dashboard, alarm và log retention. |
| 3 | Lambda/API Gateway metrics: invocations, errors, duration, latency và throttles. |
| 4 | CloudTrail Event history, management events, data events và trail. |
| 5 | Runbook triage cho lỗi 5xx, auth failure, timeout và phân tích nguyên nhân. |

## Setup cần biết

Đọc vị trí xem CloudWatch Logs/Alarms và CloudTrail Event history. Không cần tạo alarm hay trail.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws logs describe-log-groups
aws cloudwatch describe-alarms
aws cloudtrail lookup-events --max-results 5
```

## Kết quả đạt được

- Lập bảng so sánh CloudWatch và CloudTrail.
- Viết runbook xử lý lỗi 5xx.
- Vẽ luồng quan sát request qua API Gateway và Lambda.
- Giải thích management event và data event.

## Câu hỏi tự kiểm tra

- CloudTrail có thay thế application log không?
- Event history có mặc định chứa S3 object-level events không?
- Metric, log và trace trả lời các câu hỏi khác nhau thế nào?

## Nguồn học

- [CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [What is CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
