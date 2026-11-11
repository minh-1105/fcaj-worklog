---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Tuần 6 — Databases: Amazon RDS và DynamoDB

## Mục tiêu tuần

Lựa chọn relational hoặc NoSQL database dựa trên access pattern và yêu cầu hệ thống.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Relational và NoSQL; schema, transaction, ACID và indexing. |
| 2 | RDS instances, storage, backups, DB subnet group và connectivity. |
| 3 | Multi-AZ so với read replica; high availability so với read scaling. |
| 4 | DynamoDB table, item, partition key, sort key, Query, Scan và GSI. |
| 5 | On-demand/provisioned capacity, TTL, backup, chi phí và decision matrix. |

## Setup cần biết

Đọc các trường cấu hình khi tạo RDS hoặc DynamoDB. Chỉ quan sát tài liệu/Console; không bắt buộc tạo database.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws rds describe-db-instances
aws dynamodb list-tables
aws dynamodb describe-table --table-name TABLE_NAME
```

## Kết quả đạt được

- Lập decision matrix cho RDS và DynamoDB.
- Vẽ ERD dữ liệu mẫu.
- Thiết kế DynamoDB key với `(station_id, timestamp)`.
- Phân biệt Query và Scan, Multi-AZ và read replica.

## Câu hỏi tự kiểm tra

- Vì sao DynamoDB Query thường phù hợp hơn Scan?
- Multi-AZ có phải là read replica để scale đọc không?
- Access pattern ảnh hưởng thiết kế partition key thế nào?

## Nguồn học

- [Amazon RDS Getting Started](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html)
- [RDS High Availability](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
- [DynamoDB Core Components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html)
