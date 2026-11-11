---
title: "Worklog Tuần 12"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Tuần 12 — Capstone lý thuyết: IoT Weather Platform

## Mục tiêu tuần

Tổng hợp kiến thức bằng cách thiết kế, giải thích và phản biện một kiến trúc IoT mà không triển khai lên AWS.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Xác định yêu cầu chức năng và phi chức năng cho nền tảng thời tiết. |
| 2 | Thiết kế ingest flow: Device → IoT Core → S3 và Lambda → DynamoDB. |
| 3 | Thiết kế read API: Client → API Gateway → Lambda → DynamoDB. |
| 4 | Phân tích IAM, failure modes, CloudWatch, cost estimate và cleanup plan giả định. |
| 5 | Hoàn thiện sơ đồ, ADR, bản giải thích và câu hỏi phản biện. |

## Setup cần biết

Không triển khai tài nguyên. Chỉ sử dụng tài liệu, sơ đồ và AWS Pricing Calculator để phân tích kiến trúc logic.

## Mô hình tham khảo

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

## Kết quả đạt được

- Hoàn thành architecture diagram và data-flow explanation.
- Lập bảng chức năng và lý do chọn từng dịch vụ.
- Lập bảng rủi ro: mất mạng, dữ liệu trùng, lỗi quyền, chi phí và lộ certificate.
- Viết bài tổng kết kiến thức đã hiểu và hướng học tiếp theo.

## Câu hỏi tự kiểm tra

- Làm sao xử lý telemetry bị gửi trùng?
- Thành phần nào cần IAM role và thành phần nào dùng IoT policy?
- Nếu Lambda ghi DynamoDB thất bại, hệ thống cần quan sát và phục hồi thế nào?

## Nguồn học

- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/)
- [AWS IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
