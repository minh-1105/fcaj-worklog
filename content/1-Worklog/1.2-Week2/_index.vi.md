---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Tuần 2 — AWS Account, IAM, STS, CLI và Budget

## Mục tiêu tuần

Hiểu danh tính, xác thực, phân quyền và các công cụ cơ bản để thao tác với AWS.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Root user, MFA, IAM user/group/role và IAM Identity Center. |
| 2 | Cấu trúc JSON policy: Version, Statement, Effect, Action, Resource và Condition. |
| 3 | Allow, implicit deny, explicit deny; identity-based policy, resource-based policy, permissions boundary và SCP. |
| 4 | Temporary credentials, STS AssumeRole, AWS CLI profile và Region. |
| 5 | Least privilege; AWS Budgets, Cost Explorer và giới hạn của cảnh báo ngân sách. |

## Setup cần biết

Đọc các bước bảo vệ root bằng MFA, ưu tiên IAM Identity Center, cài AWS CLI v2, cấu hình SSO profile và tạo Budget. Budget không phải hard spending cap.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws --version
aws configure list
aws configure sso --profile learning
aws sso login --profile learning
aws sts get-caller-identity --profile learning
aws ec2 describe-regions --profile learning
```

## Kết quả đạt được

- Giải thích các trường chính trong IAM policy.
- Vẽ quan hệ giữa user, role và policy.
- Viết checklist bảo mật tài khoản.
- Nhận diện cách AWS CLI xác định profile, Region và danh tính gọi API.

## Câu hỏi tự kiểm tra

- Khi nào nên dùng IAM role?
- Explicit deny ảnh hưởng Allow như thế nào?
- Vì sao `iam:PassRole` khác `iam:CreateRole`?

## Nguồn học

- [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [IAM JSON policy elements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
- [AWS CLI v2 installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Configure IAM Identity Center for the CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
- [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)
