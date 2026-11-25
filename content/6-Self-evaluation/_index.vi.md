---
title: "Tự đánh giá"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong suốt thời gian thực tập tại **Công ty TNHH Amazon Web Services Việt Nam** (chương trình **Workforce Bootcamp – First Cloud AI Journey**) từ **14/09/2026** đến **14/12/2026**, tôi đã có cơ hội học hỏi, rèn luyện và áp dụng kiến thức đã được trang bị tại trường vào môi trường làm việc thực tế.

Tôi đã tham gia xây dựng **Student Flea Market (Student Market)** — nền tảng chợ đồ cũ dành cho sinh viên, gồm frontend Next.js, backend ASP.NET Core và hạ tầng serverless trên AWS — từ khâu thiết kế kiến trúc, viết Infrastructure as Code, triển khai lên production đến bảo mật, giám sát và tối ưu chi phí. Qua dự án, tôi cải thiện kỹ năng **lập trình full-stack, thiết kế kiến trúc cloud, bảo mật AWS, vận hành hệ thống, viết tài liệu kỹ thuật và làm việc nhóm**.

Về tác phong, tôi luôn cố gắng hoàn thành tốt nhiệm vụ, tuân thủ nội quy, và tích cực trao đổi với đồng nghiệp để nâng cao hiệu quả công việc.

### Công nghệ đã áp dụng trong dự án

| Mảng | Công nghệ | Những gì tôi đã làm được |
|---|---|---|
| Frontend | Next.js 16, React 19, TypeScript, Tailwind CSS, shadcn/ui, TanStack Query, Zustand, React Hook Form, Zod | Xây dựng giao diện marketplace responsive, quản lý server/client state, validate form, static export và deploy trên Vercel. |
| Backend | ASP.NET Core 8, Entity Framework Core, Npgsql, JWT, FluentValidation, Mapster, Serilog, SignalR | Tổ chức Clean Architecture (API, Application, Domain, Infrastructure), xác thực JWT + refresh token, OTP email, đăng nhập Google, chat realtime và automated test. |
| Database | PostgreSQL trên Supabase | Thiết kế data model, migration bằng EF Core, kết nối qua transaction pooler cho Lambda, sao lưu bằng `pg_dump`. |
| Compute & API | AWS Lambda, Amazon API Gateway (HTTP API) | Chạy toàn bộ API .NET trên Lambda, cấu hình CORS và throttling. |
| Media | Amazon S3, Lambda ImageProcessor (Magick.NET), Amazon CloudFront (OAC) | Upload trực tiếp bằng presigned URL, xử lý ảnh sang WebP theo sự kiện S3, phân phối ảnh qua CDN với bucket private. |
| Bảo mật | AWS WAF, SSM Parameter Store + KMS, IAM, IAM Access Analyzer, AWS CloudTrail | Đưa API ra sau CloudFront + WAF với header origin-verify, chuyển secret sang SecureString, phân quyền least privilege, bật audit trail. |
| Giám sát | Amazon CloudWatch, Amazon SNS, Amazon SQS | Structured log, alarm cho lỗi/throttle/duration, failure queue cho xử lý ảnh, gửi cảnh báo qua email. |
| Container | Amazon ECS Fargate, Application Load Balancer, Amazon ECR, AWS CodeBuild, Docker | Chạy hub SignalR trên Fargate ARM64, build image trên CodeBuild, chỉ cho ALB nhận traffic từ CloudFront. |
| IaC & DevOps | AWS SAM, AWS CloudFormation, LocalStack, PowerShell, Git/GitHub | Viết các stack media, API, WAF, realtime và build; giả lập S3/Lambda trên máy bằng LocalStack; script deploy/teardown. |
| Chi phí | AWS Budgets, Cost Explorer | Theo dõi $170 credit, chọn Vercel/Supabase thay cho Amplify/RDS và chỉ bật stack realtime khi demo. |

Để phản ánh một cách khách quan quá trình thực tập, tôi xin tự đánh giá bản thân dựa trên các tiêu chí dưới đây:

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
| --- | --- | --- | --- | --- | --- |
| 1 | **Kiến thức và kỹ năng chuyên môn** | Hiểu biết về ngành, áp dụng kiến thức vào thực tế, kỹ năng sử dụng công cụ, chất lượng công việc | ✅ | ☐ | ☐ |
| 2 | **Khả năng học hỏi** | Tiếp thu kiến thức mới, học hỏi nhanh | ☐ | ✅ | ☐ |
| 3 | **Chủ động** | Tự tìm hiểu, nhận nhiệm vụ mà không chờ chỉ dẫn | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Hoàn thành công việc đúng hạn, đảm bảo chất lượng | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Tuân thủ giờ giấc, nội quy, quy trình làm việc | ☐ | ☐ | ✅ |
| 6 | **Tính cầu tiến** | Sẵn sàng nhận feedback và cải thiện bản thân | ☐ | ✅ | ☐ |
| 7 | **Giao tiếp** | Trình bày ý tưởng, báo cáo công việc rõ ràng | ☐ | ✅ | ☐ |
| 8 | **Hợp tác nhóm** | Làm việc hiệu quả với đồng nghiệp, tham gia nhóm | ✅ | ☐ | ☐ |
| 9 | **Ứng xử chuyên nghiệp** | Tôn trọng đồng nghiệp, đối tác, môi trường làm việc | ✅ | ☐ | ☐ |
| 10 | **Tư duy giải quyết vấn đề** | Nhận diện vấn đề, đề xuất giải pháp, sáng tạo | ☐ | ✅ | ☐ |
| 11 | **Đóng góp vào dự án/tổ chức** | Hiệu quả công việc, sáng kiến cải tiến, ghi nhận từ team | ✅ | ☐ | ☐ |
| 12 | **Tổng thể** | Đánh giá chung về toàn bộ quá trình thực tập | ✅ | ☐ | ☐ |

### Điểm mạnh

* Nắm được cách thiết kế một hệ thống serverless hoàn chỉnh trên AWS và triển khai được lên production bằng Infrastructure as Code.
* Biết cân nhắc chi phí khi chọn kiến trúc (Vercel, Supabase, Lambda thay cho Amplify, RDS, EC2) để vừa với ngân sách credit.
* Chú trọng bảo mật: bucket private, secret trong SSM, WAF, least privilege, audit trail.
* Ghi chép worklog và tài liệu deploy, vận hành đầy đủ để nhóm có thể làm theo.

### Cần cải thiện

* Nâng cao tính kỷ luật, chấp hành nghiêm chỉnh nội quy và giờ giấc của công ty cũng như của bất kỳ tổ chức nào.
* Cải thiện tư duy giải quyết vấn đề: phân tích giới hạn kỹ thuật sớm hơn (ví dụ SignalR không chạy được trên API Gateway + Lambda) thay vì phát hiện sau khi deploy.
* Bổ sung kiểm thử end-to-end và integration test cho luồng thật (đăng nhập Google, upload ảnh, chat hai tài khoản) trước khi release.
* Theo dõi vòng đời công nghệ (runtime `dotnet8` hết hỗ trợ, lỗ hổng trong container image) và cập nhật kịp thời.
* Học cách giao tiếp tốt hơn trong công việc hằng ngày: báo cáo tiến độ, thống nhất thay đổi với nhóm trước khi merge.
