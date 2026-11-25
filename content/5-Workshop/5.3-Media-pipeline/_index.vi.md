---
title : "Pipeline ảnh: S3, Lambda và CloudFront"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Stack ảnh được deploy trước vì stack API import output của nó (tên bucket, URL CloudFront, SNS topic).

#### Pipeline hoạt động thế nào

1. API kiểm tra người bán và trả về **presigned POST** có thời hạn ngắn cho một key cố định trong `raw/`.
2. Trình duyệt upload thẳng file lên S3 — ảnh không đi qua API.
3. Sự kiện `s3:ObjectCreated` trong `raw/` kích hoạt Lambda **ImageProcessor**.
4. ImageProcessor ghi `large.webp` (1600px) và `thumbnail.webp` (480px) vào `processed/`.
5. CloudFront **chỉ** đọc `processed/*` qua Origin Access Control; `raw/` luôn private và tự xóa sau 7 ngày.
6. Sau hai lần retry thất bại, event được đẩy vào hàng đợi lỗi **SQS** có mã hóa.

#### Nội dung

1. [Deploy stack ảnh](5.3.1-deploy-media-stack/)
2. [Kiểm tra pipeline ảnh](5.3.2-test-pipeline/)
