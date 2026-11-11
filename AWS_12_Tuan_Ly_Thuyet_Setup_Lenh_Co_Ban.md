# LỘ TRÌNH HỌC AWS 12 TUẦN — LÝ THUYẾT, SETUP & LỆNH CƠ BẢN

> **Phiên bản:** 2.1 — điều chỉnh từ bản worklog 12 tuần  
> **Ngôn ngữ:** Tiếng Việt  
> **Thời lượng gợi ý:** 6–10 giờ/tuần (có thể tăng lên 10–15 giờ/tuần để đọc sâu)  
> **Phương pháp:** Học nguyên lý → đọc tài liệu → vẽ sơ đồ → xem ví dụ cấu hình/lệnh → tự kiểm tra kiến thức.  
> **Không bắt buộc:** Tạo tài nguyên AWS, thực hành lab có phí, triển khai dự án, xây API hoặc vận hành workload thực tế.

## 1. Mục tiêu

Sau 12 tuần, người học có thể:

1. Giải thích hạ tầng AWS, mô hình dịch vụ cloud, shared responsibility và pricing.
2. Phân biệt IAM user, role, policy, MFA, STS và cơ chế đánh giá quyền.
3. Mô tả hoạt động của EC2, EBS, S3, VPC, RDS, DynamoDB và lựa chọn dịch vụ theo nhu cầu.
4. Hiểu luồng hoạt động của Lambda, API Gateway, CloudFormation và AWS SAM.
5. Hiểu monitoring/auditing với CloudWatch và CloudTrail.
6. Giải thích MQTT, AWS IoT Core, S3, Glue và kiến trúc xử lý telemetry.
7. Đọc sơ đồ kiến trúc, nhận diện rủi ro bảo mật/chi phí và dùng AWS Well-Architected để đánh giá.
8. Biết **các bước setup và lệnh CLI cơ bản**, nhưng không cần chạy lệnh nếu chưa có môi trường.

## 2. Nguyên tắc học

- **Ưu tiên lý thuyết:** tập trung vào *là gì, vì sao dùng, cơ chế hoạt động, khi nào nên/không nên dùng*.
- **Setup chỉ để tham khảo:** hướng dẫn từng bước, không yêu cầu thực hiện ngay.
- **Lệnh chỉ ở mức cơ bản:** hiểu cú pháp, tham số, tác dụng và quyền cần có; không cần sao chép/chạy tất cả.
- **Không yêu cầu chi phí:** mặc định không tạo EC2, RDS, NAT Gateway, Glue crawler hoặc dịch vụ trả phí.
- **Ưu tiên credentials tạm thời:** không sử dụng root cho công việc hằng ngày, không đưa secret vào tài liệu/Git.
- **Minh chứng mỗi tuần:** 1 file ghi chú, 1 bảng so sánh/sơ đồ và 5–10 câu hỏi tự kiểm tra; không yêu cầu ảnh triển khai.

### Mẫu ghi chú hàng tuần

```markdown
# Week XX — Chủ đề
## 1. Khái niệm
## 2. Cơ chế hoạt động
## 3. Khi nào dùng / không nên dùng
## 4. So sánh với dịch vụ khác
## 5. Các bước setup (tham khảo)
## 6. Lệnh cơ bản (nếu có)
## 7. Câu hỏi tự kiểm tra
## 8. Điểm chưa hiểu / Cần đọc thêm
```

---

## Tuần 1 — Cloud Fundamentals và AWS Global Infrastructure

**Mục tiêu:** hiểu mô hình cloud và cấu trúc AWS trước khi học dịch vụ riêng lẻ.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Cloud computing, on-premises vs cloud; IaaS/PaaS/SaaS. |
| 2 | CapEx vs OpEx, elasticity, scalability, high availability. |
| 3 | Region, Availability Zone, Edge Location, điểm hiện diện. |
| 4 | Shared Responsibility Model; phân loại compute, storage, networking, database, security. |
| 5 | Pricing cơ bản, AWS Free Tier/credits và cách đọc tài liệu AWS. |

**Setup cần biết (không bắt buộc làm):** cách đăng nhập AWS Console; chọn Region; định vị Billing dashboard và service search.

**Đầu ra lý thuyết:** bảng so sánh cloud/on-premises, mind map nhóm dịch vụ, ví dụ trách nhiệm của AWS và khách hàng.

**Tự kiểm tra:** Region khác AZ thế nào? High availability có đồng nghĩa autoscaling không? Ai chịu trách nhiệm cập nhật OS trên EC2?

### Nguồn học tuần 1

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [AWS Cloud Practitioner learning resources](https://aws.amazon.com/training/learn-about/cloud-practitioner/) — Lộ trình nhập môn cloud, dịch vụ AWS và bài học cơ bản.
- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/) — Region, AZ và hạ tầng toàn cầu.
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) — Phân biệt trách nhiệm bảo mật AWS/khách hàng.
- [AWS Pricing Overview](https://aws.amazon.com/pricing/) — Đọc mô hình định giá, Free Tier và yếu tố tạo chi phí.

---

## Tuần 2 — AWS Account, IAM, STS, CLI và Budget

**Mục tiêu:** hiểu danh tính, xác thực, phân quyền và công cụ thao tác AWS.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Root user, MFA, IAM user/group/role, IAM Identity Center. |
| 2 | JSON policy: Version, Statement, Effect, Action, Resource, Condition. |
| 3 | Allow, implicit deny, explicit deny; identity-based vs resource-based policy; permissions boundary và SCP (tổng quan). |
| 4 | Temporary credentials, STS AssumeRole, AWS CLI profile và region. |
| 5 | IAM least privilege; Budget, Cost Explorer, cảnh báo và giới hạn của Budget. |

**Setup cần biết (tham khảo):**

1. Vào AWS Console → bảo vệ root bằng MFA.
2. Tìm hiểu IAM Identity Center và quyền truy cập theo permission set; IAM user dùng để học khái niệm, không ưu tiên access key dài hạn.
3. Cài AWS CLI v2 từ trang chính thức.
4. Cấu hình profile qua IAM Identity Center (`aws configure sso`) nếu môi trường hỗ trợ.
5. Tạo AWS Budget/cảnh báo nếu sở hữu tài khoản. **Budget không phải hard spending cap.**

**Lệnh đọc hiểu:**

```bash
aws --version
aws configure list
aws configure sso --profile learning
aws sso login --profile learning
aws sts get-caller-identity --profile learning
aws ec2 describe-regions --profile learning
```

`--profile learning` chỉ định cấu hình đã tạo; không nên chép access key vào file ghi chú.

**Policy ví dụ (chỉ để phân tích):**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListBucketsOnly",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
```

**Đầu ra lý thuyết:** giải thích từng trường policy, sơ đồ IAM user ↔ role ↔ policy, checklist account security.

**Tự kiểm tra:** Khi nào dùng role? Explicit deny ảnh hưởng Allow thế nào? Vì sao `iam:PassRole` khác `iam:CreateRole`?

### Nguồn học tuần 2

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) — Đặc quyền tối thiểu, root MFA và credentials tạm thời.
- [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) — Cơ chế Allow/Deny, identity và resource policy.
- [IAM JSON Policy Elements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) — Giải thích Statement, Action, Resource, Condition.
- [AWS CLI v2 installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) — Xem cách cài CLI trên Windows/macOS/Linux.
- [CLI IAM Identity Center setup](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) — Đọc cách thiết lập profile với aws configure sso.
- [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) — Cơ chế cảnh báo chi phí và giới hạn của Budget.

---

## Tuần 3 — Compute: EC2, EBS và Systems Manager

**Mục tiêu:** hiểu vòng đời máy ảo và cách quản trị an toàn.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | EC2, AMI, instance family/type, vCPU/RAM và shared tenancy. |
| 2 | On-Demand, Savings Plans, Spot; yếu tố ảnh hưởng chi phí. |
| 3 | EBS volume, snapshot, instance store, persistence. |
| 4 | Security Group, instance profile, SSH và Session Manager. |
| 5 | EC2 lifecycle: launch, reboot, stop, start, terminate; cleanup và public IPv4 charges. |

**Setup cần biết (không cần launch):** quy trình chọn Region → AMI → instance type → key pair (nếu SSH) → VPC/subnet → security group → IAM instance profile → storage → tags → review cost → launch.

**Session Manager prerequisites:** SSM Agent phù hợp, IAM permissions, và kết nối tới SSM endpoints. Private subnet không tự động có kết nối tới dịch vụ quản lý.

**Lệnh đọc hiểu:**

```bash
aws ec2 describe-instances --region ap-southeast-1
aws ec2 describe-volumes --region ap-southeast-1
aws ec2 describe-security-groups --region ap-southeast-1
aws ssm describe-instance-information --region ap-southeast-1
```

**Đầu ra:** sơ đồ EC2/EBS/SG/IAM; bảng Stop vs Reboot vs Terminate; giải thích lý do EC2 không kết nối được.

**Tự kiểm tra:** Dừng EC2 thì EBS có còn tính phí không? SG stateful là gì? Session Manager khác SSH ở đâu?

### Nguồn học tuần 3

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) — AMI, instance, security group và EBS.
- [EC2 Getting Started](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) — Đọc quy trình launch, connect, stop, terminate; không cần thực hiện.
- [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) — Kết nối instance không cần mở SSH inbound.
- [Session Manager setup](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html) — Điều kiện IAM, SSM Agent và mạng.

---

## Tuần 4 — Amazon S3 và Object Storage

**Mục tiêu:** hiểu cấu trúc, truy cập, bảo vệ và vòng đời object.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Bucket, object, key, namespace, consistency, object storage vs block/file storage. |
| 2 | S3 storage classes, request/storage/retrieval charges. |
| 3 | IAM policy, bucket policy, S3 Block Public Access và presigned URL. |
| 4 | Versioning, delete marker, noncurrent versions, SSE-S3/SSE-KMS. |
| 5 | Lifecycle transitions/expiration; replication và kiểm soát chi phí. |

**Setup cần biết:** cách tạo private bucket, bật Block Public Access/versioning và review encryption/lifecycle; không cần tạo bucket thật.

**Lệnh đọc hiểu:**

```bash
aws s3 ls
aws s3 ls s3://BUCKET_NAME
aws s3 cp sample.txt s3://BUCKET_NAME/sample.txt
aws s3 sync ./data s3://BUCKET_NAME/data/
aws s3api get-bucket-versioning --bucket BUCKET_NAME
```

Các lệnh `cp` và `sync` **thực hiện ghi dữ liệu khi chạy**; chỉ dùng để đọc hiểu cú pháp, không chạy nếu chưa được phép.

**Đầu ra:** bảng storage classes, sơ đồ xử lý request vào private bucket, giải thích versioning và lifecycle.

**Tự kiểm tra:** Vì sao xóa object không nhất thiết xóa mọi version? S3 bucket policy khác IAM policy thế nào?

### Nguồn học tuần 4

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) — Bucket, object, key, consistency.
- [S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/) — So sánh standard, infrequent access, archive.
- [S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) — Hiểu version ID, delete marker và phục hồi.
- [S3 Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html) — Block Public Access, bucket policy, mã hóa.

---

## Tuần 5 — Networking: VPC, Subnet, Routing và Endpoints

**Mục tiêu:** hiểu luồng mạng trong AWS.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | IPv4/CIDR, VPC, subnet, phân chia địa chỉ và AZ. |
| 2 | Route table, local route, Internet Gateway, NAT Gateway. |
| 3 | Public/private subnet: khác nhau ở tuyến route, không chỉ ở tên. |
| 4 | Security Group (stateful) vs Network ACL (stateless). |
| 5 | VPC Endpoint: S3 Gateway Endpoint và Interface Endpoint; DNS, connectivity và chi phí. |

**Setup cần biết:** mô phỏng cách chọn VPC CIDR → tạo public/private subnet ở hai AZ → route table → IGW → SG → endpoint. **Không tạo NAT Gateway chỉ để thử.**

**Lệnh đọc hiểu:**

```bash
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-route-tables
aws ec2 describe-vpc-endpoints
```

**Đầu ra:** bảng IP plan, sơ đồ hai AZ, diễn giải từng bước gói tin EC2 → S3 qua Gateway Endpoint.

**Tự kiểm tra:** Subnet nào được gọi là public? SG và NACL lọc traffic khác nhau thế nào? Khi nào endpoint giúp tránh NAT?

### Nguồn học tuần 5

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [Plan your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) — CIDR, subnet, route table và thiết kế AZ.
- [VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) — Quy tắc inbound/outbound và stateful.
- [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) — Phân biệt NACL và security group.
- [S3 Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html) — Endpoint, route table, traffic tới S3.

---

## Tuần 6 — Databases: RDS và DynamoDB

**Mục tiêu:** lựa chọn SQL hoặc NoSQL dựa trên access pattern và yêu cầu hệ thống.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Relational vs NoSQL, schema, transaction, ACID, indexing. |
| 2 | RDS instances, storage, backups, DB subnet group và connectivity. |
| 3 | Multi-AZ vs read replica; availability vs read scaling. |
| 4 | DynamoDB table, item, partition key, sort key, Query vs Scan, GSI. |
| 5 | On-demand vs provisioned capacity, TTL, backup, chi phí và decision matrix. |

**Setup cần biết:** các trường cấu hình khi tạo RDS hoặc DynamoDB; chỉ quan sát tài liệu/Console, không bắt buộc tạo database.

**Lệnh đọc hiểu:**

```bash
aws rds describe-db-instances
aws dynamodb list-tables
aws dynamodb describe-table --table-name TABLE_NAME
```

**Đầu ra:** decision matrix RDS/DynamoDB; ERD mẫu; DynamoDB key design với `(station_id, timestamp)`.

**Tự kiểm tra:** Vì sao DynamoDB Query thường phù hợp hơn Scan? Multi-AZ có phải read replica để scale đọc không?

### Nguồn học tuần 6

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [Amazon RDS Getting Started](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html) — DB instances, networking và kết nối.
- [RDS High Availability](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) — Multi-AZ và mục đích high availability.
- [DynamoDB Core Components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html) — Table, item, partition key và sort key.
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) — Đọc ví dụ Query, PutItem và Scan.

---

## Tuần 7 — Serverless: Lambda, API Gateway và Event-Driven Architecture

**Mục tiêu:** hiểu kiến trúc HTTP API và xử lý sự kiện không cần quản trị server.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Serverless, event-driven, Lambda execution environment. |
| 2 | Lambda handler, event/context, timeout, memory, cold start, concurrency. |
| 3 | API Gateway HTTP API, route, integration, authentication/authorization và CORS. |
| 4 | Lambda execution role, least privilege; AWS SDK truy vấn DynamoDB. |
| 5 | Error handling, retry, idempotency, synchronous vs asynchronous invocation. |

**Setup cần biết:** trình tự Console tạo Lambda → execution role → HTTP API → route/integration → CloudWatch logs; không deploy bắt buộc.

**Lệnh đọc hiểu:**

```bash
aws lambda list-functions
aws lambda get-function --function-name FUNCTION_NAME
aws apigatewayv2 get-apis
```

**Ví dụ logic:** `Client → API Gateway → Lambda → DynamoDB`. Phân biệt với `DynamoDB Streams → Lambda` (nguồn sự kiện khác, không phải Lambda gọi database).

**Đầu ra:** sequence diagram request/response; bảng 4xx vs 5xx; giải thích quyền `dynamodb:PutItem` / `dynamodb:Query`.

**Tự kiểm tra:** Lambda cần role để làm gì? CORS xử lý vấn đề nào? Retry có thể tạo duplicate record không?

### Nguồn học tuần 7

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) — Runtime, invocation, concurrency và timeout.
- [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html) — Logging, idempotency, IAM và hiệu năng.
- [API Gateway HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html) — Routes, integration, auth và xử lý request.
- [DynamoDB Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) — Phân biệt Lambda gọi SDK để đọc/ghi DB với DynamoDB Streams.

---

## Tuần 8 — Infrastructure as Code: CloudFormation và AWS SAM

**Mục tiêu:** biết đọc template và hiểu vòng đời cấu hình hạ tầng.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | IaC, declarative vs imperative, idempotency. |
| 2 | CloudFormation template: Resources, Parameters, Outputs, Conditions. |
| 3 | Stack, changeset, update, rollback, drift. |
| 4 | SAM template, transform, Lambda/API resources, build/package/deploy. |
| 5 | DeletionPolicy, Retain/Snapshot, stack cleanup và security review. |

**Setup cần biết:** cài SAM CLI (tùy chọn); cấu trúc dự án SAM; không tạo stack thật.

**Lệnh đọc hiểu:**

```bash
sam --version
sam validate
sam build
aws cloudformation list-stacks
aws cloudformation describe-stacks --stack-name STACK_NAME
```

**Chú ý:** `sam build` có thể tạo artifact ở máy cục bộ; `sam deploy` và `sam delete` là thao tác thực tế trên tài khoản, không thuộc phần bắt buộc.

**Đầu ra:** chú giải một template YAML ngắn; sơ đồ stack lifecycle; nêu trường hợp xóa stack nhưng tài nguyên còn tồn tại.

**Tự kiểm tra:** Idempotency nghĩa là gì? Vì sao `DeletionPolicy: Retain` cần được chú ý?

### Nguồn học tuần 8

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [CloudFormation Getting Started](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html) — Template, stack và vòng đời triển khai.
- [CloudFormation Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) — Parameters, Resources, Outputs, Ref.
- [AWS SAM Getting Started](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html) — SAM template, sam build/deploy/delete.
- [DeletionPolicy attribute](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-attribute-deletionpolicy.html) — Hiểu Retain, Snapshot và rủi ro còn tài nguyên sau cleanup.

---

## Tuần 9 — Monitoring, Logging và Auditing

**Mục tiêu:** phân biệt quan sát sức khỏe hệ thống với theo dõi hành động API.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Metrics, logs, traces, events, audit records. |
| 2 | CloudWatch metrics, logs, dashboard, alarm, log retention. |
| 3 | Lambda/API Gateway metrics: invocations, errors, duration, latency, throttles. |
| 4 | CloudTrail Event history, management events, data events, trail. |
| 5 | Runbook triage: lỗi 5xx, auth failure, timeout và phân tích nguyên nhân. |

**Setup cần biết:** nơi xem CloudWatch Logs/Alarms và CloudTrail Event history; không cần tạo alarm hay trail.

**Lệnh đọc hiểu:**

```bash
aws logs describe-log-groups
aws cloudwatch describe-alarms
aws cloudtrail lookup-events --max-results 5
```

**Đầu ra:** bảng CloudWatch vs CloudTrail; mẫu runbook 5xx; sơ đồ quan sát request qua API Gateway và Lambda.

**Tự kiểm tra:** CloudTrail có thay thế application logs không? Event history có bao gồm S3 object-level events mặc định không?

### Nguồn học tuần 9

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html) — Metrics, namespace, dimensions, alarms.
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) — Log groups, streams và retention.
- [What is CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) — Audit trail và API activity.
- [CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) — Tra cứu management events và giới hạn Event history.

---

## Tuần 10 — AWS IoT Core, MQTT và Data Pipeline

**Mục tiêu:** hiểu cách dữ liệu từ thiết bị đi vào dịch vụ cloud.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | IoT concepts, MQTT, broker, publish/subscribe, topic, QoS. |
| 2 | AWS IoT Thing, X.509 certificate, IoT policy, client ID và topic authorization. |
| 3 | IoT Rule, action, IAM role, luồng telemetry vào S3 hoặc Lambda. |
| 4 | S3 raw data, object key design, retry/idempotency và vấn đề small files. |
| 5 | Glue Data Catalog, crawler, ETL và sự khác biệt với S3 storage. |

**Setup cần biết:** quy trình đăng ký Thing/certificate/policy và dùng MQTT test client; **không tạo thiết bị/certificate thật** trong phần bắt buộc.

**Ví dụ topic và payload:**

```text
weather/station-01/telemetry
```

```json
{
  "station_id": "station-01",
  "timestamp": "2026-11-05T07:00:00Z",
  "temperature_c": 29.4,
  "humidity_percent": 74
}
```

**Đầu ra:** luồng `Device → IoT Core → Rule → S3` và luồng `Device → IoT Core → Rule → Lambda → DynamoDB`; giải thích khi nào dùng Glue.

**Tự kiểm tra:** IoT policy khác IAM policy thế nào? Tại sao phải kiểm tra quyền publish/subscribe theo topic?

### Nguồn học tuần 10

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [AWS IoT Core Overview](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) — Thing, broker, MQTT, certificate, policy.
- [IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html) — Chọn hướng đọc hoặc interactive tutorial không cần code.
- [AWS IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html) — Chuyển message đến AWS services qua rule actions.
- [AWS Glue Overview](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html) — Crawler, Data Catalog và ETL khái niệm.
- [Glue Crawlers](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html) — Cách crawler nhận diện schema trong dữ liệu S3.

---

## Tuần 11 — Security, Cost Optimization và Well-Architected

**Mục tiêu:** đánh giá kiến trúc mà không cần triển khai thực tế.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Sáu trụ cột AWS Well-Architected. |
| 2 | Defense in depth, least privilege, encryption at rest/in transit. |
| 3 | Availability, durability, fault tolerance, backup, RTO/RPO. |
| 4 | Cost driver: compute, storage, requests, data transfer, NAT, logs. |
| 5 | Trade-offs, Architecture Decision Record (ADR), kiến trúc tham chiếu. |

**Setup cần biết:** cách mở AWS Pricing Calculator và nhập giả định chi phí; không cần tạo workload.

**Đầu ra:** checklist Well-Architected mẫu; bảng risk/mitigation; 2 ADR về chọn database và chọn network pattern.

**Tự kiểm tra:** Chi phí nào vẫn tồn tại khi EC2 dừng? Vì sao high availability khác disaster recovery?

### Nguồn học tuần 11

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/) — Sáu trụ cột, câu hỏi review và best practices.
- [AWS Architecture Center](https://aws.amazon.com/architecture/) — Đọc và phân tích các reference architectures.
- [AWS Pricing Calculator](https://calculator.aws/) — Hiểu giả định đầu vào để ước tính chi phí.
- [AWS Cost Management](https://docs.aws.amazon.com/cost-management/latest/userguide/billing-getting-started.html) — Budget, Cost Explorer và cách đọc chi phí.

---

## Tuần 12 — Capstone lý thuyết: Phân tích kiến trúc IoT Weather Platform

**Mục tiêu:** tổng hợp kiến thức thông qua thiết kế và giải thích hệ thống, **không triển khai lên AWS**.

| Buổi | Nội dung lý thuyết |
|---|---|
| 1 | Xác định yêu cầu chức năng và phi chức năng cho nền tảng thời tiết. |
| 2 | Thiết kế luồng ingest: Device → IoT Core → S3 và Lambda → DynamoDB. |
| 3 | Thiết kế API đọc: Client → API Gateway → Lambda → DynamoDB. |
| 4 | Phân tích IAM, failure modes, CloudWatch, cost estimate và cleanup plan giả định. |
| 5 | Hoàn thiện sơ đồ, ADR, bản giải thích và câu hỏi phản biện. |

### Kiến trúc tham khảo

```text
Device Simulator (conceptual)
           |
           | MQTT
           v
      AWS IoT Core
           |
           v
       IoT Rule
       /       \
      v         v
     S3       Lambda (validate)
  Raw data        |
                  v
               DynamoDB
                  ^
                  |
               Lambda
                  ^
                  |
             API Gateway
                  ^
                  |
                Client
```

> Hai nhánh từ IoT Rule là hai action có thể được cấu hình có chủ đích. Sơ đồ chỉ thể hiện kiến trúc logic, không khẳng định rằng hệ thống đã được triển khai.

**Đầu ra chỉ cần tài liệu:**

- 1 architecture diagram.
- 1 bảng chức năng và lý do chọn từng dịch vụ.
- 1 bản giải thích data flow từ thiết bị đến API.
- 1 bảng rủi ro: mất mạng, dữ liệu trùng, lỗi quyền, chi phí, lộ certificate.
- 1 bài tổng kết: đã hiểu gì, chủ đề còn chưa rõ và định hướng học tiếp.

**Không yêu cầu:** Source code, tài nguyên AWS, IaC deploy, API thật, certificate thật hay demo hoạt động.

### Nguồn học tuần 12

> **Cách học:** đọc theo thứ tự từ trên xuống. Chỉ cần đọc, ghi chú và phân tích ví dụ; **không cần tạo tài nguyên AWS**.

- [AWS Architecture Center](https://aws.amazon.com/architecture/) — Tham khảo kiến trúc nhiều tầng và serverless.
- [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/) — Đọc template mô tả API và Lambda.
- [AWS IoT Core Getting Started](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs.html) — Hiểu đường đi dữ liệu từ thiết bị.
- [AWS Well-Architected](https://docs.aws.amazon.com/wellarchitected/latest/framework/) — Dùng sáu trụ cột để phản biện sơ đồ capstone.

---

## 3. Bảng lệnh AWS cơ bản cần nhận diện

| Nhóm | Lệnh | Ý nghĩa |
|---|---|---|
| CLI | `aws --version` | Xem phiên bản CLI. |
| CLI | `aws configure list` | Xem nguồn cấu hình hiện tại (cẩn trọng khi chia sẻ output). |
| Auth | `aws sso login --profile learning` | Đăng nhập SSO cho profile tương ứng. |
| STS | `aws sts get-caller-identity` | Kiểm tra danh tính đang gọi API. |
| EC2 | `aws ec2 describe-instances` | Liệt kê/mô tả EC2 trong Region/profile đang dùng. |
| VPC | `aws ec2 describe-vpcs` | Xem VPC. |
| S3 | `aws s3 ls` | Liệt kê bucket có quyền xem. |
| RDS | `aws rds describe-db-instances` | Xem RDS instances. |
| DynamoDB | `aws dynamodb list-tables` | Liệt kê bảng DynamoDB. |
| Lambda | `aws lambda list-functions` | Xem các Lambda functions. |
| CloudFormation | `aws cloudformation list-stacks` | Xem các stack. |
| CloudWatch | `aws logs describe-log-groups` | Xem log groups. |

**Quy tắc:** `describe`, `list`, `get` thường là thao tác truy vấn, nhưng vẫn phải có quyền API thích hợp và có thể liên quan đến giới hạn hoặc chi phí của dịch vụ. Những lệnh tạo/xóa/cập nhật tài nguyên không nằm trong yêu cầu học bắt buộc của khóa này.

## 4. Cách tự đánh giá cuối khóa

| Tiêu chí | Tỷ trọng | Minh chứng lý thuyết |
|---|---:|---|
| Cloud fundamentals và networking | 25% | Giải thích khái niệm, sơ đồ VPC |
| Identity, security, pricing | 20% | Phân tích IAM policy, cost drivers |
| Compute, storage, database | 20% | Bảng lựa chọn dịch vụ và trade-off |
| Serverless, IaC, observability, IoT | 20% | Sơ đồ luồng xử lý và câu hỏi cơ chế |
| Kiến trúc capstone lý thuyết | 15% | Diagram, risk table, ADR và giải thích |

## 5. Checklist cho mỗi tuần

- [ ] Giải thích được khái niệm bằng lời của mình.
- [ ] Mô tả được luồng hoạt động hoặc vòng đời dịch vụ.
- [ ] So sánh được dịch vụ với ít nhất một lựa chọn thay thế.
- [ ] Biết cách tìm mục cấu hình trong Console hoặc cú pháp lệnh cơ bản.
- [ ] Hoàn thành 5–10 câu hỏi tự kiểm tra.
- [ ] Lưu một trang ghi chú và danh sách điểm cần đọc lại.

## 6. Ghi chú an toàn và chi phí

- **Không cần tạo tài khoản AWS** để hoàn thành nội dung lý thuyết.
- Các ví dụ Console và CLI nhằm mục đích đọc hiểu; không cần chạy và không mặc định miễn phí.
- Nếu tự thử về sau, luôn kiểm tra pricing ở Region được chọn, dùng MFA, credentials tạm thời và nguyên tắc least privilege.
- Không lưu access key, secret key, token, private key hoặc certificate vào Git/worklog.
- Không xem AWS Budget là cơ chế tự động khóa chi tiêu.

---

**Kết quả mong đợi:** Sau 12 tuần, bạn có thể giải thích cơ chế hoạt động của các dịch vụ AWS phổ biến, đọc kiến trúc mẫu, hiểu bước setup và lệnh CLI nền tảng, và sẵn sàng chuyển sang giai đoạn thực hành khi cần.


## Phụ lục — Cách sử dụng nguồn học hiệu quả

1. **Đọc trước:** chọn 1–2 tài liệu chính của tuần, tóm tắt định nghĩa và cơ chế.
2. **Đọc để phân biệt:** lập bảng dịch vụ tương tự nhau (ví dụ RDS/DynamoDB, SG/NACL).
3. **Xem setup/lệnh:** đọc các bước và giải thích lệnh làm gì, không cần chạy hoặc tạo tài khoản.
4. **Kiểm tra:** tự giải thích một sơ đồ hoặc tình huống; ghi điều còn chưa rõ.
5. **Chi phí:** nếu tài liệu có bước tạo tài nguyên, chỉ đọc; không nhấn Deploy/Create khi chưa có nhu cầu và ngân sách.

**Nguồn học tổng hợp:** [AWS Skill Builder](https://skillbuilder.aws/) · [AWS Training and Certification](https://aws.amazon.com/training/) · [AWS Documentation](https://docs.aws.amazon.com/) · [AWS Architecture Center](https://aws.amazon.com/architecture/). Một số khóa học trên Skill Builder có thể yêu cầu đăng nhập hoặc trả phí; ưu tiên nội dung miễn phí.
