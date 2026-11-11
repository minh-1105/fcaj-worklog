---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Student Flea Market
## Nền tảng chợ đồ cũ Cloud-Native dành cho sinh viên

### 1. Tóm tắt điều hành

Student Flea Market là nền tảng chợ trực tuyến dành cho sinh viên đại học mua, bán hoặc trao đổi giáo trình, tài liệu học tập, thiết bị điện tử, đồ dùng ký túc xá và các vật dụng đã qua sử dụng. Dự án giải quyết một vấn đề phổ biến: sản phẩm thường được đăng rải rác trên mạng xã hội và nhóm chat, khiến người dùng khó tìm kiếm, so sánh, quản lý và xác minh độ tin cậy.

Giải pháp kết hợp giao diện Next.js responsive, backend ASP.NET Core và các dịch vụ AWS được quản lý. Frontend hỗ trợ tìm sản phẩm, xác thực, quản lý tin đăng và giao diện quản trị. Backend cung cấp REST API có bảo vệ, thực thi quy tắc nghiệp vụ, lưu dữ liệu giao dịch trong PostgreSQL và cấp presigned request có thời hạn ngắn để trình duyệt upload ảnh trực tiếp lên Amazon S3. Một Lambda event-driven xử lý ảnh thành các phiên bản WebP tối ưu, còn Amazon CloudFront phân phối ảnh đã xử lý.

Phiên bản đầu tập trung vào marketplace MVP: đăng ký và đăng nhập, duyệt danh mục, tìm kiếm và lọc, tạo/cập nhật tin đăng thuộc sở hữu người bán và xử lý ảnh an toàn. Exchange request, giao dịch, chat, đánh giá, báo cáo và thanh toán đã được dự kiến trong domain model nhưng thuộc các giai đoạn mở rộng.

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Sinh viên thường dùng mạng xã hội, nhóm tin nhắn hoặc liên hệ cá nhân để trao đổi đồ cũ. Cách làm này có nhiều hạn chế:

- Bài đăng nhanh chóng bị trôi và không thể lọc theo cấu trúc thống nhất.
- Mô tả, tình trạng, giá và thông tin liên hệ không theo tiêu chuẩn chung.
- Người mua khó so sánh hoặc xác định sản phẩm còn khả dụng hay không.
- Người bán không có quy trình riêng để quản lý tin đăng và hình ảnh.
- Khả năng xác minh danh tính, quyền sở hữu, kiểm duyệt và báo cáo còn hạn chế.
- Upload ảnh lớn qua API truyền thống làm tăng tải và băng thông application server.

#### Giải pháp đề xuất

Student Flea Market cung cấp nền tảng tập trung để sinh viên đăng ký, đăng nhập, duyệt và lọc sản phẩm, quản lý tin đăng, upload ảnh an toàn và liên hệ người bán. Các giai đoạn sau bổ sung trao đổi, giao dịch, nhắn tin, đánh giá, báo cáo và thanh toán.

Nền tảng áp dụng thiết kế cloud-native. Thành phần theo request dùng dịch vụ managed và tính phí theo mức sử dụng, trong khi PostgreSQL bảo đảm tính nhất quán cho người dùng, sản phẩm, trao đổi, giao dịch, hội thoại, đánh giá và báo cáo.

#### Lợi ích

- **Sinh viên:** tìm sản phẩm phù hợp, tiết kiệm nhanh hơn trong cộng đồng quen thuộc.
- **Người bán:** quy trình đăng bán và quản lý ảnh rõ ràng.
- **Cộng đồng trường:** tăng tái sử dụng giáo trình và thiết bị, giảm lãng phí.
- **Quản trị viên:** nền tảng cho kiểm duyệt, báo cáo và thực thi chính sách.
- **Đội phát triển:** hệ thống module hóa, kiểm thử được bằng PostgreSQL và LocalStack trước khi triển khai AWS.

### 3. Phạm vi dự án

#### Phạm vi MVP

1. Đăng ký tài khoản và đăng nhập bằng JWT.
2. Danh mục sản phẩm và danh sách có phân trang.
3. Tìm kiếm và lọc theo danh mục.
4. Tạo, xem, cập nhật và xóa tin đăng.
5. Kiểm tra quyền sở hữu để chỉ người bán được thay đổi tin.
6. Upload ảnh trực tiếp bằng presigned S3 POST.
7. Tự động kiểm tra và chuyển ảnh sang các phiên bản WebP tối ưu.
8. Health check, structured logging, validation và phản hồi lỗi thống nhất.
9. Giao diện responsive hỗ trợ tiếng Việt và tiếng Anh.
10. Giao diện quản trị và trang chính sách ban đầu.

#### Phạm vi mở rộng

Data model đã dự kiến favorite, exchange request, hội thoại, giao dịch, thanh toán, đánh giá, báo cáo vi phạm, khôi phục tài khoản, đề xuất sản phẩm và analytics. Các chức năng này chưa thuộc production MVP đầu tiên.

### 4. Kiến trúc giải pháp

{{<mermaid align="left">}}
graph TD
    U[Sinh viên hoặc quản trị viên] -->|HTTPS| A[AWS Amplify Hosting<br/>Next.js static frontend]
    A -->|REST và JWT| G[Amazon API Gateway<br/>HTTP API]
    G --> B[AWS Lambda<br/>ASP.NET Core API]
    B --> R[(Amazon RDS<br/>PostgreSQL)]
    B --> S[AWS Secrets Manager]
    B -->|Presigned POST| U
    U -->|Upload trực tiếp| RAW[Amazon S3<br/>Ảnh raw]
    RAW -->|ObjectCreated event| P[AWS Lambda<br/>Image Processor]
    P -->|WebP variants| PROCESSED[Amazon S3<br/>Ảnh processed]
    PROCESSED --> C[Amazon CloudFront<br/>Origin Access Control]
    C -->|Ảnh được cache| U
    P --> Q[Amazon SQS<br/>Failure queue]
    G --> CW[Amazon CloudWatch]
    B --> CW
    P --> CW
{{< /mermaid >}}

#### Luồng gọi API

1. Người dùng mở ứng dụng Next.js static được host trên AWS Amplify.
2. Frontend gọi API Gateway qua HTTPS.
3. API Gateway kích hoạt ASP.NET Core API dưới dạng Lambda event function on-demand.
4. API validate request, xác minh JWT, kiểm tra quyền sở hữu và đọc/ghi PostgreSQL qua Entity Framework Core.
5. Log và operational metric được gửi tới CloudWatch.

HTTP API phù hợp vì MVP cần REST/JSON routing và request mang JWT nhưng chưa cần usage plan, API key, response transformation hoặc API caching. Lambda On-Demand phù hợp với traffic giai đoạn đầu không đều và không phải duy trì application server khi không có request.

#### Luồng upload và xử lý ảnh

1. Người bán đã đăng nhập yêu cầu quyền upload ảnh cho tin đăng.
2. API kiểm tra người bán, tin đăng, MIME type, kích thước và số lượng ảnh.
3. API trả presigned S3 POST policy có thời hạn ngắn và object key cố định.
4. Trình duyệt upload trực tiếp vào prefix **raw/** của S3 private.
5. S3 ObjectCreated event kích hoạt Lambda xử lý ảnh.
6. Lambda tạo **thumbnail.webp** và **large.webp** trong prefix **processed/**.
7. CloudFront đọc duy nhất ảnh processed qua Origin Access Control và cache gần người dùng.
8. Event thất bại được chuyển vào SQS failure queue có mã hóa.

### 5. Công nghệ và dịch vụ AWS

| Thành phần | Công nghệ hoặc dịch vụ | Trách nhiệm |
|---|---|---|
| Giao diện web | Next.js 16, React 19, TypeScript | Giao diện marketplace responsive |
| UI và state | Tailwind CSS, shadcn/ui, TanStack Query, Zustand | UI, server state và client state |
| Host frontend | AWS Amplify Hosting | Static hosting, HTTPS, CDN và deploy theo branch |
| Public API | Amazon API Gateway HTTP API | HTTPS entry point, routing, CORS và throttling |
| Backend | ASP.NET Core 8 trên AWS Lambda | Xác thực, validation, catalog, ownership và presigning |
| Dữ liệu quan hệ | Amazon RDS for PostgreSQL | Người dùng, sản phẩm, giao dịch, tin nhắn và báo cáo |
| Truy cập dữ liệu | Entity Framework Core và Npgsql | Migration, constraint và query |
| Lưu trữ media | Amazon S3 | Ảnh raw và processed dạng private |
| Xử lý media | AWS Lambda với Magick.NET | Xác minh ảnh và tạo WebP |
| Phân phối media | Amazon CloudFront với OAC | Phân phối ảnh an toàn và có cache |
| Lưu bí mật | AWS Secrets Manager | Database credentials và application secrets |
| Xử lý lỗi | Amazon SQS | Dead-letter destination cho lỗi xử lý ảnh |
| Quan sát | Amazon CloudWatch | Log, metric, retention và alarm |
| Infrastructure as Code | AWS SAM và CloudFormation | Triển khai AWS có thể lặp lại |
| Giả lập AWS local | LocalStack | Kiểm thử S3 và image pipeline với chi phí thấp |

### 6. Thiết kế dữ liệu

Relational model gồm ApplicationUser, Category, Item, ItemImage, Favorite, ExchangeRequest, Transaction, Payment, Conversation, Message, Review và Report. Index và constraint bảo đảm tính duy nhất, giá không âm, rating hợp lệ, quan hệ sở hữu và hiệu quả truy vấn. Ảnh nằm trong object storage; database chỉ giữ tham chiếu thay vì binary image.

### 7. Thiết kế bảo mật

- Tách deployment identity khỏi Lambda runtime role theo least privilege.
- JWT bảo vệ thao tác của người bán; backend lấy user ID từ token đã xác minh.
- Database credential và signing secret không được commit vào source.
- S3 bucket là private, có mã hóa, Block Public Access và tắt ACL.
- Presigned upload giới hạn thời hạn, object key, content type và kích thước.
- CloudFront OAC chỉ đọc ảnh processed.
- CORS chỉ cho phép HTTPS frontend origin cần thiết.
- Không log password, token, secret hoặc presigned URL đầy đủ.
- CloudWatch theo dõi API error, Lambda failure, throttle, duration và failure queue.
- Quyền quản trị phải được backend kiểm tra, không chỉ dựa vào UI.

### 8. Kế hoạch triển khai

| Giai đoạn | Tuần | Công việc chính | Kết quả |
|---|---:|---|---|
| Yêu cầu và thiết kế | 1–3 | Xác định MVP, kiến trúc, data model và API contract | Requirement, sơ đồ, ERD và API contract |
| Nền tảng backend | 4–6 | Clean Architecture, EF Core, Identity/JWT, category và item CRUD | API, validation, test và Swagger |
| Frontend và tích hợp | 7–9 | Marketplace UI, auth state, filtering, API client, seller workflow | Frontend responsive tích hợp API |
| Media và cloud readiness | 10–11 | Presigned upload, LocalStack, image Lambda, S3/CloudFront IaC, IAM review | Media pipeline và template đã validate |
| Kiểm thử và bàn giao | 12 | End-to-end check, security review, observability và tài liệu | Test report, runbook, deployment guide và demo |

### 9. Phương pháp ước tính ngân sách

Chi phí tháng cuối cùng sẽ được lập bằng AWS Pricing Calculator cho Region đã chọn sau khi chốt traffic và retention. Estimate gồm Amplify build và data transfer, API Gateway request, Lambda duration/memory, RDS instance/storage/backup, S3 storage/request, CloudFront delivery, Secrets Manager, CloudWatch và SQS.

PostgreSQL dự kiến là chi phí cố định chính; các dịch vụ theo request tăng giảm theo usage. Chi phí development được giảm bằng LocalStack, log retention ngắn, S3 lifecycle, resource tag, AWS Budgets và teardown môi trường tạm. Free Tier hoặc credit chỉ là giảm giá tạm thời, không phải ngân sách vận hành dài hạn.

### 10. Rủi ro và giảm thiểu

| Rủi ro | Ảnh hưởng | Giảm thiểu |
|---|---|---|
| Ít người dùng | Cao | Pilot với một nhóm sinh viên và ưu tiên luồng đăng/tìm đơn giản |
| Tin đăng gian lận | Cao | Ownership rule, report, moderation, account status và audit log |
| Truy cập trái phép | Cao | JWT, least-privilege IAM, private RDS networking và negative test |
| Upload ảnh độc hại | Cao | Presigned constraint, MIME/magic-byte check, limit, re-encode và raw storage private |
| Event trùng hoặc thất bại | Trung bình | Object key idempotent, output path xác định, retry, alarm và SQS |
| Chi phí ngoài dự kiến | Trung bình | Budget, alarm, lifecycle, log retention và cost review |
| Database gián đoạn | Cao | Backup, restore test, Multi-AZ khi hợp lý, migration và recovery objective |
| Mở rộng phạm vi quá mức | Trung bình | Tách MVP khỏi future feature và xác định acceptance criteria |

### 11. Trạng thái triển khai hiện tại

Repository hiện đã có:

- giao diện Next.js với xác thực, tìm sản phẩm, đăng tin, trang chính sách và khung quản trị;
- solution ASP.NET Core 8 tách API, Application, Domain, Infrastructure, ImageProcessor và Tests;
- JWT register/login, category query, tìm sản phẩm và item CRUD có kiểm tra người bán;
- EF Core PostgreSQL migration và domain model marketplace;
- contract và validation cho presigned S3 upload;
- .NET image processor kích hoạt bởi S3 và automated tests;
- LocalStack initialization cho media pipeline;
- SAM/CloudFormation cho S3 private, Lambda xử lý ảnh, CloudFront OAC, alarm và failure queue;
- frontend lint, typecheck, static export và Amplify build configuration.

Triển khai đầy đủ lên AWS vẫn là mốc tiếp theo. Lambda hosting adapter cho API, API Gateway integration, môi trường RDS, secret, production origin và IAM theo môi trường phải được hoàn thiện và kiểm chứng trước production.

### 12. Kết quả kỳ vọng

- Marketplace MVP có thể sử dụng trong cộng đồng sinh viên.
- Luồng xác thực và quản lý tin đăng an toàn.
- Upload trực tiếp và phân phối ảnh tối ưu.
- Backend Clean Architecture và frontend typed dễ bảo trì.
- Hạ tầng có thể tái tạo và môi trường giả lập cloud cục bộ.
- Tài liệu vận hành, bảo mật và kiểm soát chi phí.
- Nền tảng mở rộng cho trao đổi, nhắn tin, đánh giá, kiểm duyệt và thanh toán.

### Nguồn tham khảo

- [AWS Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/)
- [Amazon API Gateway HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [AWS Lambda với ASP.NET Core](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-asp.html)
- [Amazon RDS for PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [Amazon S3 presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
- [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [AWS Secrets Manager với .NET](https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets-net-sdk.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
