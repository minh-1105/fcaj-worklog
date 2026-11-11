---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Week 2 — AWS Account, IAM, STS, CLI, and Budgets

## Weekly objective

Understand identities, authentication, authorization, and the basic tools used to interact with AWS.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Root user, MFA, IAM users/groups/roles, and IAM Identity Center. |
| 2 | JSON policy structure: Version, Statement, Effect, Action, Resource, and Condition. |
| 3 | Allow, implicit deny, explicit deny; identity-based and resource-based policies, permissions boundaries, and SCPs. |
| 4 | Temporary credentials, STS AssumeRole, AWS CLI profiles, and Regions. |
| 5 | Least privilege; AWS Budgets, Cost Explorer, and the limits of budget alerts. |

## Setup to understand

Review how to protect the root user with MFA, prefer IAM Identity Center, install AWS CLI v2, configure an SSO profile, and create a budget. A budget is not a hard spending cap.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws --version
aws configure list
aws configure sso --profile learning
aws sso login --profile learning
aws sts get-caller-identity --profile learning
aws ec2 describe-regions --profile learning
```

## Learning outcomes

- Explained the main fields in an IAM policy.
- Mapped the relationships among users, roles, and policies.
- Prepared an account-security checklist.
- Identified how the AWS CLI selects a profile, Region, and calling identity.

## Self-review questions

- When should an IAM role be used?
- How does an explicit deny affect an allow?
- Why is `iam:PassRole` different from `iam:CreateRole`?

## Learning resources

- [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [IAM JSON policy elements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
- [AWS CLI v2 installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Configure IAM Identity Center for the CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
- [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)
