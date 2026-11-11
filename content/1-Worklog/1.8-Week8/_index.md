---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Week 8 — Infrastructure as Code: CloudFormation and AWS SAM

## Weekly objective

Learn to read templates and understand the lifecycle of infrastructure managed as code.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Infrastructure as Code; declarative, imperative, and idempotent approaches. |
| 2 | CloudFormation templates: Resources, Parameters, Outputs, and Conditions. |
| 3 | Stacks, change sets, updates, rollback, and drift. |
| 4 | SAM templates, transforms, Lambda/API resources, build, package, and deploy. |
| 5 | DeletionPolicy, Retain/Snapshot, stack cleanup, and security review. |

## Setup to understand

Review SAM CLI installation and a SAM project structure. `sam build` can create local artifacts; `sam deploy` and `sam delete` affect a real account and are not required.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
sam --version
sam validate
sam build
aws cloudformation list-stacks
aws cloudformation describe-stacks --stack-name STACK_NAME
```

## Learning outcomes

- Annotated a CloudFormation/SAM YAML template.
- Drew the CloudFormation stack lifecycle.
- Explained idempotency, rollback, and drift.
- Identified cases where resources remain after a stack is deleted.

## Self-review questions

- What does idempotency mean?
- Why does `DeletionPolicy: Retain` require attention?
- How does a change set help before a stack update?

## Learning resources

- [CloudFormation Getting Started](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html)
- [CloudFormation Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)
- [AWS SAM Getting Started](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html)
- [DeletionPolicy attribute](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-attribute-deletionpolicy.html)
