---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# Tuần 3 — Compute: EC2, EBS và Systems Manager

## Mục tiêu tuần

Hiểu vòng đời máy ảo, lưu trữ block và cách quản trị EC2 an toàn.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | EC2, AMI, instance family/type, vCPU/RAM và shared tenancy. |
| 2 | On-Demand, Savings Plans, Spot Instances và các yếu tố ảnh hưởng chi phí. |
| 3 | EBS volume, snapshot, instance store và persistence. |
| 4 | Security Group, instance profile, SSH và Systems Manager Session Manager. |
| 5 | Vòng đời EC2: launch, reboot, stop, start, terminate; cleanup và public IPv4 charges. |

## Setup cần biết

Đọc quy trình chọn Region, AMI, instance type, VPC/subnet, security group, IAM instance profile, storage và tags. Session Manager cần SSM Agent, IAM permissions và network connectivity phù hợp.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws ec2 describe-instances --region ap-southeast-1
aws ec2 describe-volumes --region ap-southeast-1
aws ec2 describe-security-groups --region ap-southeast-1
aws ssm describe-instance-information --region ap-southeast-1
```

## Kết quả đạt được

- Vẽ sơ đồ quan hệ EC2, EBS, Security Group và IAM role.
- So sánh Stop, Reboot và Terminate.
- Giải thích các nguyên nhân phổ biến khiến EC2 không kết nối được.
- Mô tả khác biệt giữa SSH và Session Manager.

## Câu hỏi tự kiểm tra

- Dừng EC2 thì EBS có còn tính phí không?
- Security Group stateful nghĩa là gì?
- Session Manager cần những điều kiện nào?

## Nguồn học

- [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
- [EC2 Getting Started](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
- [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [Session Manager setup](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)
