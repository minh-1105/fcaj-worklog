---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Tuần 8 — Infrastructure as Code: CloudFormation và AWS SAM

## Mục tiêu tuần

Biết đọc template và hiểu vòng đời quản lý hạ tầng bằng mã.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Infrastructure as Code; declarative, imperative và idempotency. |
| 2 | CloudFormation template: Resources, Parameters, Outputs và Conditions. |
| 3 | Stack, change set, update, rollback và drift. |
| 4 | SAM template, transform, Lambda/API resources, build, package và deploy. |
| 5 | DeletionPolicy, Retain/Snapshot, stack cleanup và security review. |

## Setup cần biết

Đọc cách cài SAM CLI và cấu trúc dự án SAM. `sam build` có thể tạo artifact cục bộ; `sam deploy` và `sam delete` tác động tài khoản thật nên không thuộc phần bắt buộc.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
sam --version
sam validate
sam build
aws cloudformation list-stacks
aws cloudformation describe-stacks --stack-name STACK_NAME
```

## Kết quả đạt được

- Chú giải một CloudFormation/SAM template YAML.
- Vẽ vòng đời CloudFormation stack.
- Giải thích idempotency, rollback và drift.
- Nêu trường hợp xóa stack nhưng tài nguyên vẫn tồn tại.

## Câu hỏi tự kiểm tra

- Idempotency nghĩa là gì?
- Vì sao `DeletionPolicy: Retain` cần được chú ý?
- Change set giúp ích gì trước khi cập nhật stack?

## Nguồn học

- [CloudFormation Getting Started](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html)
- [CloudFormation Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)
- [AWS SAM Getting Started](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html)
- [DeletionPolicy attribute](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-attribute-deletionpolicy.html)
