---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Tuần 5 — Networking: VPC, Subnet, Routing và Endpoints

## Mục tiêu tuần

Hiểu cách địa chỉ, routing và kiểm soát truy cập tạo nên luồng mạng trong AWS.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | IPv4/CIDR, VPC, subnet, phân chia địa chỉ và Availability Zone. |
| 2 | Route table, local route, Internet Gateway và NAT Gateway. |
| 3 | Public/private subnet: khác nhau ở routing, không chỉ ở tên. |
| 4 | Security Group stateful so với Network ACL stateless. |
| 5 | VPC Endpoint: S3 Gateway Endpoint và Interface Endpoint; DNS, connectivity và chi phí. |

## Setup cần biết

Mô phỏng quy trình chọn VPC CIDR, tạo public/private subnet ở hai AZ, route table, Internet Gateway, Security Group và endpoint. Không tạo NAT Gateway chỉ để thử.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-route-tables
aws ec2 describe-vpc-endpoints
```

## Kết quả đạt được

- Hoàn thành bảng IP plan và sơ đồ VPC hai AZ.
- Diễn giải luồng gói tin EC2 đến S3 qua Gateway Endpoint.
- So sánh Security Group và Network ACL.
- Giải thích yếu tố làm một subnet trở thành public.

## Câu hỏi tự kiểm tra

- Subnet nào được gọi là public?
- Security Group và Network ACL xử lý traffic khác nhau thế nào?
- Khi nào VPC Endpoint giúp tránh NAT Gateway?

## Nguồn học

- [Plan your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html)
- [VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [S3 Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)
