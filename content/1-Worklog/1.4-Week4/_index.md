---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Week 4 — Amazon S3 and Object Storage

## Weekly objective

Understand the structure, access controls, protection, and lifecycle of S3 objects.

## Learning plan

| Session | Theory topic |
|---:|---|
| 1 | Buckets, objects, keys, namespaces, consistency; object versus block/file storage. |
| 2 | S3 storage classes and storage, request, and retrieval charges. |
| 3 | IAM policies, bucket policies, S3 Block Public Access, and presigned URLs. |
| 4 | Versioning, delete markers, noncurrent versions, SSE-S3, and SSE-KMS. |
| 5 | Lifecycle transitions/expiration, replication, and cost control. |

## Setup to understand

Review how to create a private bucket, enable Block Public Access and versioning, and inspect encryption and lifecycle settings. No real bucket is required.

## Basic commands to understand

The commands below are for recognizing syntax and purpose; they do not need to be run without a suitable environment and permissions.

```bash
aws s3 ls
aws s3 ls s3://BUCKET_NAME
aws s3 cp sample.txt s3://BUCKET_NAME/sample.txt
aws s3 sync ./data s3://BUCKET_NAME/data/
aws s3api get-bucket-versioning --bucket BUCKET_NAME
```

## Learning outcomes

- Compared S3 storage classes.
- Drew the request flow to a private bucket.
- Explained versioning, delete markers, and lifecycle rules.
- Distinguished IAM policies from bucket policies.

## Self-review questions

- Why does deleting an object not necessarily delete every version?
- How is a bucket policy different from an IAM policy?
- When should a presigned URL be used?

## Learning resources

- [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)
- [S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
- [S3 Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)
