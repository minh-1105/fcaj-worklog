---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Tuần 4 — Amazon S3 và Object Storage

## Mục tiêu tuần

Hiểu cấu trúc, cơ chế truy cập, bảo vệ và vòng đời của S3 objects.

## Nội dung học tập

| Buổi | Nội dung lý thuyết |
|---:|---|
| 1 | Bucket, object, key, namespace, consistency; object storage so với block/file storage. |
| 2 | S3 storage classes và chi phí storage, request, retrieval. |
| 3 | IAM policy, bucket policy, S3 Block Public Access và presigned URL. |
| 4 | Versioning, delete marker, noncurrent versions, SSE-S3 và SSE-KMS. |
| 5 | Lifecycle transitions/expiration, replication và kiểm soát chi phí. |

## Setup cần biết

Đọc cách tạo private bucket, bật Block Public Access và versioning, sau đó review encryption và lifecycle. Không cần tạo bucket thật.

## Lệnh cơ bản để đọc hiểu

Các lệnh dưới đây dùng để nhận diện cú pháp và tác dụng; không cần chạy nếu chưa có môi trường hoặc quyền phù hợp.

```bash
aws s3 ls
aws s3 ls s3://BUCKET_NAME
aws s3 cp sample.txt s3://BUCKET_NAME/sample.txt
aws s3 sync ./data s3://BUCKET_NAME/data/
aws s3api get-bucket-versioning --bucket BUCKET_NAME
```

## Kết quả đạt được

- Lập bảng so sánh các S3 storage class.
- Vẽ luồng request đến private bucket.
- Giải thích versioning, delete marker và lifecycle.
- Phân biệt IAM policy và bucket policy.

## Câu hỏi tự kiểm tra

- Vì sao xóa object không nhất thiết xóa mọi version?
- Bucket policy khác IAM policy như thế nào?
- Khi nào nên dùng presigned URL?

## Nguồn học

- [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)
- [S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
- [S3 Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)
