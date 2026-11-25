---
title : "Giám sát và audit"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### CloudWatch alarm

Cả hai stack tạo alarm gửi chung vào một SNS topic:

| Nguồn alarm | Theo dõi gì |
|---|---|
| Lambda API | Lỗi |
| Lambda ImageProcessor | Lỗi, throttle, thời gian chạy p99 |
| Hàng đợi lỗi SQS | Số message đang chờ (event xử lý ảnh bị lỗi) |

```powershell
aws cloudwatch describe-alarms --profile student-market --region ap-southeast-1 `
  --query "MetricAlarms[].{Name:AlarmName,State:StateValue}" --output table
```

Để kiểm tra đường gửi cảnh báo, tạm thời đặt một alarm sang `ALARM` rồi kiểm tra hộp thư:

```powershell
aws cloudwatch set-alarm-state --alarm-name "<ten alarm>" --state-value ALARM `
  --state-reason "workshop test" --profile student-market --region ap-southeast-1
```

#### Log

Log group có thời gian lưu giới hạn (mặc định 14 ngày). Dùng CloudWatch Logs Insights trên log group của API để tìm lỗi:

```text
fields @timestamp, @message
| filter @message like /error|Error|ERROR/
| sort @timestamp desc
| limit 20
```

#### CloudTrail và IAM Access Analyzer

- **CloudTrail** (được tạo vì `EnableAuditLogging=true`) ghi management events ở mọi Region (ai đã thay đổi IAM, stack, bucket). Mở *CloudTrail → Event history*, lọc `Event source = cloudformation.amazonaws.com` để xem các lần deploy.
- **IAM Access Analyzer** (`student-market-dev-account`) báo các tài nguyên được chia sẻ ra ngoài tài khoản. Mở *IAM → Access analyzer* và xác nhận không có finding bất thường — media bucket không được xuất hiện ở dạng public.

#### Chi phí

Mở *Billing → Cost Explorer*, nhóm theo **Service** và so với AWS Budget. WAF nên là khoản duy nhất đáng kể.
