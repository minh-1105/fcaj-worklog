---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# Tuần 7 — Serverless: Lambda, API Gateway và Event-Driven Architecture

## Mục tiêu tuần

Hiểu kiến trúc HTTP API và xử lý sự kiện mà không phải quản trị server.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Serverless, event-driven và Lambda execution environment. |
| 2 | Lambda handler, event/context, timeout, memory, cold start và concurrency. |
| 3 | API Gateway HTTP API, route, integration, authentication/authorization và CORS. |
| 4 | Lambda execution role, least privilege và AWS SDK truy vấn DynamoDB. |
| 5 | Error handling, retry, idempotency; synchronous và asynchronous invocation. |

## Setup cần biết

Đọc trình tự tạo Lambda, execution role, HTTP API, route/integration và xem CloudWatch Logs. Không bắt buộc deploy.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws lambda list-functions
aws lambda get-function --function-name FUNCTION_NAME
aws apigatewayv2 get-apis
```

## Kết quả đạt được

- Vẽ sequence diagram `Client → API Gateway → Lambda → DynamoDB`.
- Phân biệt lỗi 4xx và 5xx.
- Giải thích quyền `dynamodb:PutItem` và `dynamodb:Query`.
- Phân biệt Lambda gọi DynamoDB với DynamoDB Streams kích hoạt Lambda.

## Câu hỏi tự kiểm tra

- Lambda cần execution role để làm gì?
- CORS giải quyết vấn đề nào?
- Retry có thể tạo duplicate record không?

## Nguồn học

- [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
- [API Gateway HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html)
