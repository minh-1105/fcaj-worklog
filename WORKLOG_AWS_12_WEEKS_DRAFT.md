# Bản nháp Worklog học AWS trong 12 tuần

> Trạng thái: **Draft để kiểm tra nội dung**  
> Ngôn ngữ: Tiếng Việt  
> Thời lượng đề xuất: 10–15 giờ/tuần  
> Định hướng: Cloud fundamentals → kiến trúc AWS → serverless/IoT → capstone

## 1. Mục tiêu tổng thể

Sau 12 tuần, người học có thể:

- Giải thích các khái niệm nền tảng của AWS: Region, Availability Zone, mô hình trách nhiệm chia sẻ và mô hình tính phí theo mức sử dụng.
- Quản lý truy cập theo nguyên tắc đặc quyền tối thiểu với IAM, MFA và thông tin xác thực tạm thời.
- Triển khai workload cơ bản với Amazon EC2, Amazon S3, Amazon VPC, Amazon RDS và Amazon DynamoDB.
- Xây dựng API serverless bằng AWS Lambda, Amazon API Gateway và AWS SAM.
- Tự động hóa hạ tầng bằng AWS CloudFormation.
- Theo dõi workload bằng Amazon CloudWatch và kiểm tra hoạt động tài khoản với AWS CloudTrail.
- Kết nối thiết bị mô phỏng với AWS IoT Core, đưa dữ liệu vào S3 và thử nghiệm catalog dữ liệu bằng AWS Glue.
- Thiết kế, ước tính chi phí và thuyết trình một kiến trúc theo AWS Well-Architected Framework.

## 2. Quy ước thực hiện

- Mỗi tuần phải có ít nhất một **đầu ra kiểm chứng được**: sơ đồ, ảnh chụp, file cấu hình, lệnh CLI, báo cáo hoặc demo.
- Mọi tài nguyên phải được gắn tag tối thiểu: `Project`, `Environment`, `Owner` và `Week`.
- Không dùng root user cho công việc hằng ngày; bật MFA và ưu tiên thông tin xác thực tạm thời.
- Không ghi Access Key, Secret Access Key, mật khẩu hoặc token vào Git.
- Luôn ghi lại chi phí dự kiến trước khi tạo tài nguyên và dọn tài nguyên sau khi hoàn thành bài lab.
- Nhật ký mỗi tuần nên có bốn phần: **Mục tiêu**, **Công việc**, **Kết quả**, **Khó khăn/Bài học**.

---

## Tuần 1 — Tổng quan Cloud và AWS

### Mục tiêu

- Hiểu lợi ích và đặc trưng của điện toán đám mây.
- Phân biệt Region, Availability Zone và Edge Location.
- Nhận biết các nhóm dịch vụ AWS cốt lõi: compute, storage, database, networking, security và management.
- Hiểu mô hình trách nhiệm chia sẻ giữa AWS và khách hàng.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Tìm hiểu IaaS, PaaS, SaaS; CapEx và OpEx; khả năng co giãn và thanh toán theo mức sử dụng. |
| 2 | Tìm hiểu hạ tầng toàn cầu AWS: Region, Availability Zone và điểm hiện diện. |
| 3 | Khảo sát AWS Management Console và các nhóm dịch vụ chính. |
| 4 | Học mô hình trách nhiệm chia sẻ và trách nhiệm bảo mật của khách hàng. |
| 5 | Vẽ sơ đồ khái niệm AWS và hoàn thành bài kiểm tra kiến thức nền tảng. |

### Đầu ra cần có

- Một sơ đồ mind map các nhóm dịch vụ AWS.
- Bảng so sánh on-premises và cloud.
- Bản tóm tắt một trang về mô hình trách nhiệm chia sẻ.
- Ảnh hoặc chứng nhận hoàn thành phần đầu của AWS Cloud Essentials Learning Plan.

### Nguồn học

- [AWS Cloud Practitioner learning resources](https://aws.amazon.com/training/learn-about/cloud-practitioner/)
- [AWS Training and Certification](https://aws.amazon.com/training/)
- [Security in IAM and AWS STS — Shared responsibility](https://docs.aws.amazon.com/IAM/latest/UserGuide/security.html)
- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

---

## Tuần 2 — Bảo mật tài khoản, IAM và AWS CLI

### Mục tiêu

- Hiểu IAM user, group, role, policy và cơ chế đánh giá quyền.
- Áp dụng MFA, đặc quyền tối thiểu và thông tin xác thực tạm thời.
- Cài đặt và sử dụng AWS CLI để truy vấn tài khoản.
- Thiết lập AWS Budget để kiểm soát chi phí học tập.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Bảo vệ root user, bật MFA và kiểm tra thông tin liên hệ tài khoản. |
| 2 | Học cấu trúc IAM policy; phân biệt identity-based policy và resource-based policy. |
| 3 | Tạo role thực hành với quyền tối thiểu; kiểm tra hành vi Allow và Deny. |
| 4 | Cài AWS CLI v2; cấu hình đăng nhập phù hợp và chạy các lệnh `sts get-caller-identity`, `configure list`, `ec2 describe-regions`. |
| 5 | Tạo monthly cost budget và cảnh báo chi phí; ghi lại checklist bảo mật tài khoản. |

### Đầu ra cần có

- Sơ đồ IAM user/group/role/policy.
- Một IAM policy mẫu chỉ cho phép đọc danh sách S3 bucket.
- File log đã loại bỏ thông tin nhạy cảm từ ba lệnh AWS CLI.
- Ảnh cấu hình MFA và AWS Budget; không đưa QR code hoặc secret vào báo cáo.

### Nguồn học

- [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)
- [Install or update AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [Setting up the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html)
- [Creating a cost budget](https://docs.aws.amazon.com/cost-management/latest/userguide/create-cost-budget.html)

---

## Tuần 3 — Compute với Amazon EC2 và Systems Manager

### Mục tiêu

- Hiểu AMI, instance type, EBS, security group và EC2 pricing model.
- Khởi tạo và vận hành một EC2 instance Linux.
- Kết nối an toàn bằng AWS Systems Manager Session Manager.
- Biết dừng và terminate tài nguyên đúng cách.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Tìm hiểu EC2, AMI, instance family, On-Demand, Savings Plans và Spot. |
| 2 | Tạo EC2 instance trong môi trường lab; gắn IAM role và tag. |
| 3 | Kết nối bằng Session Manager, cập nhật hệ điều hành và cài web server đơn giản. |
| 4 | Tạo EBS volume, attach, format, mount và kiểm tra dữ liệu. |
| 5 | Tạo snapshot, kiểm tra metric cơ bản, sau đó dọn instance, volume và snapshot không cần thiết. |

### Đầu ra cần có

- Sơ đồ EC2 gồm AMI, instance, EBS, security group và IAM role.
- Trang web tĩnh “Hello AWS” chạy trên EC2 hoặc bằng chứng truy cập qua Session Manager.
- Bảng so sánh Stop, Reboot và Terminate.
- Nhật ký cleanup tài nguyên.

### Nguồn học

- [Get started with Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
- [Setting up Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)

---

## Tuần 4 — Amazon S3 và quản lý dữ liệu

### Mục tiêu

- Hiểu bucket, object, key, storage class và vòng đời dữ liệu.
- Phân biệt IAM policy, bucket policy và S3 Block Public Access.
- Sử dụng versioning, encryption và lifecycle rule.
- Truy cập S3 bằng Console và AWS CLI.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Tìm hiểu object storage, tính nhất quán, namespace và quy tắc đặt tên bucket. |
| 2 | Tạo private bucket; upload, download và đồng bộ file bằng AWS CLI. |
| 3 | Bật versioning, thử ghi đè và khôi phục phiên bản cũ. |
| 4 | Kiểm tra server-side encryption, Block Public Access và viết bucket policy giới hạn quyền. |
| 5 | Tạo lifecycle rule cho dữ liệu thử nghiệm; kiểm tra rồi xóa object versions và bucket. |

### Đầu ra cần có

- Một bảng so sánh S3 Standard, Intelligent-Tiering và các lớp lưu trữ archive.
- Các lệnh CLI để upload, list, copy và sync dữ liệu.
- Một bucket policy mẫu theo nguyên tắc đặc quyền tối thiểu.
- Báo cáo ngắn về versioning, encryption và lifecycle.

### Nguồn học

- [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [Getting started with Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html)
- [Security best practices for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)
- [Amazon S3 storage classes](https://aws.amazon.com/s3/storage-classes/)

---

## Tuần 5 — Networking với Amazon VPC

### Mục tiêu

- Hiểu CIDR, subnet, route table, internet gateway, NAT gateway, security group và network ACL.
- Thiết kế VPC nhiều Availability Zone với public/private subnet.
- Hiểu luồng gói tin từ workload đến Internet hoặc dịch vụ AWS.
- Làm quen với Gateway VPC Endpoint cho S3.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Ôn IPv4, CIDR và chia subnet; lập IP plan không chồng lấn. |
| 2 | Vẽ VPC hai Availability Zone gồm public và private subnet. |
| 3 | Tạo VPC, subnet, route table và internet gateway trong lab. |
| 4 | Phân tích stateful security group và stateless network ACL; thử nghiệm rule. |
| 5 | Tạo S3 Gateway Endpoint, cập nhật route table và kiểm tra đường truy cập riêng đến S3. |

### Đầu ra cần có

- Bảng IP plan và sơ đồ VPC hai Availability Zone.
- Bảng so sánh security group và network ACL.
- Route table trước và sau khi thêm S3 Gateway Endpoint.
- Bản mô tả từng bước của luồng traffic.

### Nguồn học

- [Plan your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html)
- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)
- [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)

---

## Tuần 6 — Database: Amazon RDS và DynamoDB

### Mục tiêu

- Phân biệt relational và NoSQL database.
- Hiểu DB instance, backup, Multi-AZ và read replica của Amazon RDS.
- Hiểu partition key, sort key và access pattern của DynamoDB.
- Chọn database dựa trên yêu cầu thay vì theo thói quen công nghệ.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | So sánh SQL/NoSQL, schema, transaction, scaling và access pattern. |
| 2 | Tìm hiểu RDS, DB subnet group, security group, backup và Multi-AZ. |
| 3 | Tạo RDS development instance nếu ngân sách cho phép, kết nối từ workload phù hợp rồi cleanup. |
| 4 | Thiết kế bảng DynamoDB cho dữ liệu cảm biến theo `station_id` và thời gian. |
| 5 | Tạo bảng DynamoDB, thêm/query dữ liệu bằng Console hoặc CLI và ghi lại tiêu chí lựa chọn database. |

### Đầu ra cần có

- Decision matrix so sánh RDS và DynamoDB.
- ERD đơn giản cho dữ liệu người dùng/trạm thời tiết.
- Thiết kế partition key và sort key cho dữ liệu cảm biến.
- Nhật ký tạo, truy vấn và cleanup database lab.

### Nguồn học

- [Getting started with Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html)
- [Setting up your Amazon RDS environment](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SettingUp.html)
- [Getting started with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html)
- [DynamoDB core components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)

---

## Tuần 7 — Serverless với Lambda, API Gateway và DynamoDB

### Mục tiêu

- Hiểu event-driven và serverless computing.
- Viết Lambda function có logging và error handling cơ bản.
- Tạo HTTP API bằng API Gateway.
- Kết nối API với DynamoDB theo nguyên tắc đặc quyền tối thiểu.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Tìm hiểu Lambda execution environment, event, timeout, memory, concurrency và cold start. |
| 2 | Viết Lambda function nhận và kiểm tra payload dữ liệu thời tiết. |
| 3 | Tạo HTTP API với các route cơ bản và tích hợp Lambda. |
| 4 | Ghi/đọc dữ liệu DynamoDB; thu hẹp quyền của Lambda execution role. |
| 5 | Kiểm thử happy path, invalid input và lỗi dependency; xem log trong CloudWatch. |

### Đầu ra cần có

- API nhỏ gồm ít nhất hai operation, ví dụ `POST /readings` và `GET /readings/{stationId}`.
- Collection kiểm thử bằng curl/Postman hoặc script tương đương.
- Sơ đồ request flow: Client → API Gateway → Lambda → DynamoDB.
- Bảng test case và kết quả.

### Nguồn học

- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)
- [Get started with API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html)
- [Lambda best practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
- [Using Lambda with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html)

---

## Tuần 8 — Infrastructure as Code với CloudFormation và AWS SAM

### Mục tiêu

- Hiểu lợi ích của Infrastructure as Code.
- Đọc và viết CloudFormation template bằng YAML.
- Đóng gói ứng dụng serverless bằng AWS SAM.
- Biết validate, deploy, cập nhật và xóa stack.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Tìm hiểu template, resource, parameter, output, reference và stack lifecycle. |
| 2 | Viết CloudFormation template tạo tài nguyên S3 hoặc DynamoDB đơn giản. |
| 3 | Cài AWS SAM CLI và chạy SAM Hello World. |
| 4 | Chuyển API tuần 7 thành SAM template; chạy build và test phù hợp. |
| 5 | Deploy stack, kiểm tra output, cập nhật một cấu hình rồi xóa stack. |

### Đầu ra cần có

- CloudFormation/SAM template có comment rõ ràng.
- README gồm lệnh validate, build, deploy và delete.
- Ảnh stack events và outputs.
- Ghi chú về idempotency, rollback và drift.

### Nguồn học

- [Getting started with CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html)
- [How CloudFormation works](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cloudformation-overview.html)
- [Getting started with AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html)
- [AWS SAM Hello World tutorial](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html)

---

## Tuần 9 — Observability, auditing và vận hành

### Mục tiêu

- Phân biệt metrics, logs, traces, events và audit records.
- Tạo CloudWatch dashboard và alarm.
- Dùng CloudTrail Event history để điều tra thao tác API.
- Viết runbook xử lý một sự cố đơn giản.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Khảo sát CloudWatch metrics, logs, dashboards và alarms. |
| 2 | Thêm structured logging cho Lambda và xác định các metric quan trọng. |
| 3 | Tạo dashboard cho request count, error count, latency và Lambda duration. |
| 4 | Tạo alarm cho error hoặc throttling; thử kích hoạt có kiểm soát. |
| 5 | Dùng CloudTrail Event history tìm thao tác tạo/xóa tài nguyên và hoàn thành runbook. |

### Đầu ra cần có

- Một CloudWatch dashboard có tối thiểu bốn widget.
- Một alarm và bằng chứng chuyển trạng thái khi thử nghiệm.
- Báo cáo điều tra một API event trong CloudTrail.
- Runbook “API trả lỗi 5xx” gồm bước phát hiện, khoanh vùng và khắc phục.

### Nguồn học

- [Amazon CloudWatch Documentation](https://docs.aws.amazon.com/cloudwatch/)
- [Getting started with CloudWatch dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)
- [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [Creating a CloudTrail trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html)

---

## Tuần 10 — AWS IoT Core và data pipeline

### Mục tiêu

- Hiểu MQTT, topic, publish/subscribe, thing, certificate và IoT policy.
- Kết nối một thiết bị thật hoặc thiết bị mô phỏng đến AWS IoT Core.
- Dùng IoT Rule chuyển dữ liệu cảm biến đến S3 hoặc Lambda.
- Hiểu vai trò của Glue Data Catalog, crawler và ETL trong data lake.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Ôn MQTT và kiến trúc AWS IoT Core; thiết kế topic hierarchy cho trạm thời tiết. |
| 2 | Tạo IoT thing, certificate và IoT policy giới hạn đúng client/topic. |
| 3 | Chạy thiết bị mô phỏng gửi JSON telemetry và xem dữ liệu bằng MQTT test client. |
| 4 | Tạo IoT Rule đưa dữ liệu đến S3 hoặc gọi Lambda; kiểm tra lỗi và retry. |
| 5 | Tạo Glue crawler cho dữ liệu mẫu trong S3, xem schema trong Data Catalog và ghi lại kế hoạch ETL. |

### Đầu ra cần có

- Topic convention, ví dụ `weather/{stationId}/telemetry`.
- JSON schema của bản tin cảm biến.
- Bằng chứng thiết bị publish và cloud subscribe thành công.
- Sơ đồ pipeline: Device → IoT Core → Rule → S3/Lambda → Glue Data Catalog.
- IoT policy đã che Account ID và không chứa certificate/private key.

### Nguồn học

- [Getting started with AWS IoT Core](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html)
- [AWS IoT Core quick connect tutorial](https://docs.aws.amazon.com/iot/latest/developerguide/iot-quick-start.html)
- [AWS IoT rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)
- [What is AWS Glue?](https://docs.aws.amazon.com/glue/latest/dg/)
- [Using crawlers to populate the Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)

---

## Tuần 11 — Thiết kế kiến trúc, bảo mật và tối ưu chi phí

### Mục tiêu

- Đánh giá kiến trúc theo sáu trụ cột AWS Well-Architected.
- Áp dụng defense in depth, least privilege, encryption và private connectivity.
- Ước tính và kiểm soát chi phí workload.
- Hoàn thiện thiết kế IoT Weather Platform trước khi triển khai capstone.

### Kế hoạch học và thực hành

| Ngày | Nội dung |
|---|---|
| 1 | Học sáu trụ cột của AWS Well-Architected Framework. |
| 2 | Threat-model luồng thiết bị → cloud → người dùng; xác định trust boundary và dữ liệu nhạy cảm. |
| 3 | Rà soát IAM role, encryption, logging, backup, retry và failure mode của kiến trúc. |
| 4 | Lập cost estimate; xác định cost driver, budget threshold và phương án cleanup. |
| 5 | Thực hiện mini architecture review, ghi lại rủi ro, trade-off và quyết định thiết kế. |

### Đầu ra cần có

- Sơ đồ kiến trúc logic và deployment diagram.
- Bảng threat/risk/mitigation.
- Bảng estimate chi phí tháng và các giả định đầu vào.
- Well-Architected checklist cùng ít nhất năm hành động cải thiện.
- Architecture Decision Record cho hai quyết định quan trọng.

### Nguồn học

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Pricing Calculator](https://calculator.aws/)
- [Getting started with AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/billing-getting-started.html)
- [Managing costs with AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

---

## Tuần 12 — Capstone, kiểm thử và trình bày kết quả

### Mục tiêu

- Tích hợp kiến thức 11 tuần thành một sản phẩm mẫu có thể demo.
- Kiểm thử chức năng, bảo mật, khả năng quan sát và cleanup.
- Viết tài liệu triển khai/vận hành rõ ràng.
- Tổng kết kết quả học tập và lập kế hoạch học tiếp theo.

### Phạm vi capstone đề xuất

Xây dựng bản MVP của **IoT Weather Platform**:

1. Thiết bị mô phỏng publish telemetry qua MQTT đến AWS IoT Core.
2. IoT Rule chuyển dữ liệu đến S3 hoặc Lambda.
3. Lambda kiểm tra payload và lưu dữ liệu cần truy vấn vào DynamoDB.
4. API Gateway cung cấp API đọc dữ liệu.
5. CloudWatch thu thập log/metric và cảnh báo lỗi.
6. Hạ tầng cốt lõi được mô tả bằng AWS SAM hoặc CloudFormation.
7. Tùy thời gian: triển khai giao diện mẫu bằng AWS Amplify Hosting và bảo vệ người dùng bằng Amazon Cognito.

### Kế hoạch thực hiện

| Ngày | Nội dung |
|---|---|
| 1 | Chốt phạm vi MVP, acceptance criteria và danh sách tài nguyên. |
| 2 | Deploy backend/data pipeline bằng IaC và chạy smoke test. |
| 3 | Hoàn thiện dashboard, alarm, quyền IAM và kiểm thử negative cases. |
| 4 | Viết README, runbook, architecture document và hướng dẫn cleanup. |
| 5 | Demo, thu thập phản hồi, tổng kết bài học và xóa tài nguyên không cần thiết. |

### Đầu ra cần có

- Source code và IaC template không chứa secret.
- Architecture diagram và data-flow diagram.
- README có prerequisites, deploy, test, troubleshoot và cleanup.
- Test report với happy path, invalid input, unauthorized access và dependency failure.
- Demo 5–10 phút và báo cáo tổng kết 12 tuần.
- Backlog cho giai đoạn tiếp theo.

### Nguồn học

- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/)
- [Getting started with Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/getting-started.html)
- [What is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)

---

## 3. Tiêu chí đánh giá cuối kỳ

| Nhóm tiêu chí | Tỷ trọng | Minh chứng |
|---|---:|---|
| Kiến thức nền tảng AWS | 20% | Quiz, giải thích kiến trúc và lựa chọn dịch vụ |
| Thực hành kỹ thuật | 30% | Lab, CLI, cấu hình, API và data pipeline |
| Bảo mật và quản trị chi phí | 15% | IAM review, MFA, Budget, encryption và cleanup |
| Infrastructure as Code và vận hành | 15% | CloudFormation/SAM, logs, dashboard, alarm và runbook |
| Capstone và tài liệu | 20% | Demo, sơ đồ, test report, README và bài thuyết trình |

## 4. Checklist dùng cho mỗi tuần

- [ ] Hoàn thành mục tiêu kiến thức.
- [ ] Hoàn thành bài thực hành.
- [ ] Lưu minh chứng nhưng không để lộ thông tin nhạy cảm.
- [ ] Ghi lại lỗi gặp phải và cách xử lý.
- [ ] Kiểm tra Cost Explorer/Billing dashboard nếu có quyền.
- [ ] Dọn các tài nguyên không còn sử dụng.
- [ ] Viết phần kết quả và bài học rút ra.

## 5. Ghi chú về chi phí và an toàn

- AWS Free Tier hoặc promotional credits không đồng nghĩa mọi dịch vụ đều miễn phí.
- RDS, NAT Gateway, AWS Glue jobs/crawlers, public IPv4, CloudTrail data events và một số loại log có thể phát sinh chi phí.
- Chỉ tạo tài nguyên cần cho bài lab, chọn Region nhất quán và xóa tài nguyên ngay sau khi thu thập đủ minh chứng.
- Không công khai Account ID, email cá nhân, IP nhạy cảm, access key, certificate hoặc private key trong ảnh và source code.
- Khi không có ngân sách lab, thay phần triển khai tốn phí bằng sơ đồ, template IaC được validate, dữ liệu mô phỏng hoặc local test.
