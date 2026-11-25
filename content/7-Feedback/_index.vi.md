---
title: "Chia sẻ, đóng góp ý kiến"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

> Dưới đây là những chia sẻ cá nhân của mình về trải nghiệm khi tham gia chương trình First Cloud AI Journey, với mong muốn giúp team FCAJ hoàn thiện chương trình cho các khóa sau.

### Đánh giá chung

**1. Môi trường làm việc**  
Môi trường thân thiện và cởi mở, mọi người sẵn sàng hỗ trợ khi mình gặp khó khăn. Lộ trình 12 tuần được sắp xếp rõ ràng, đi từ nền tảng cloud, IAM, compute, storage, networking đến serverless, IaC, giám sát và bảo mật, nên mình biết mỗi tuần cần tập trung vào đâu.

**2. Sự hỗ trợ của mentor / team admin**  
Mentor giải thích rõ ràng và khuyến khích mình tự tìm hiểu, tự thử trước khi hỏi. Cách hướng dẫn này giúp mình tự xử lý được nhiều vấn đề khi làm dự án Student Market, ví dụ chọn Supabase thay cho RDS để tiết kiệm credit hay chuyển chat realtime sang ECS Fargate khi phát hiện SignalR không chạy được trên API Gateway + Lambda. Team admin hỗ trợ tốt các thủ tục và tài liệu của chương trình.

**3. Sự phù hợp giữa công việc và chuyên ngành học**  
Mình học ngành Trí tuệ nhân tạo, nên phần lớn kiến thức về cloud, hạ tầng và bảo mật là mới. Tuy vậy, nền tảng lập trình ở trường giúp mình bắt kịp, và kiến thức cloud là nền tảng cần thiết để triển khai các ứng dụng AI sau này.

**4. Cơ hội học hỏi & phát triển kỹ năng**  
Đây là phần mình thấy giá trị nhất. Qua dự án Student Market, mình được làm trọn một vòng: thiết kế kiến trúc, viết IaC bằng AWS SAM, deploy lên production, cấu hình WAF, SSM Parameter Store, CloudWatch alarm, CloudTrail và theo dõi chi phí bằng AWS Budgets. Ngoài kỹ thuật, mình còn học cách viết worklog, tài liệu deploy và phối hợp với các thành viên khác trong nhóm qua Git.

**5. Văn hóa & tinh thần đồng đội**  
Mọi người tôn trọng lẫn nhau, làm việc nghiêm túc nhưng vẫn thoải mái. Các bạn trong nhóm sẵn sàng chia sẻ kiến thức và cùng nhau gỡ lỗi khi gặp vấn đề.

**6. Chính sách / phúc lợi cho thực tập sinh**  
Việc được cấp **$170 credit AWS** để thực hành trên tài khoản thật là điểm cộng lớn, giúp mình học cách vận hành hệ thống thật thay vì chỉ đọc lý thuyết. Các buổi event và workshop của chương trình cũng giúp mình cập nhật kiến thức mới.

### Một số câu hỏi khác

- **Điều bạn hài lòng nhất trong thời gian thực tập?**  
  Được tự tay đưa một dự án thật lên production trên AWS, từ lúc thiết kế đến lúc bảo mật và giám sát, và hiểu vì sao mỗi dịch vụ được chọn.
- **Điều bạn nghĩ công ty cần cải thiện cho các thực tập sinh sau?**  
  Nên có hướng dẫn sớm về giới hạn của gói tài khoản AWS Free (ví dụ không đăng ký được gói CloudFront Free nên WAF phải trả phí) và cách xin cấp quyền IAM, vì nhóm mình mất thời gian chờ quyền trước khi deploy được.
- **Nếu giới thiệu cho bạn bè, bạn có khuyên họ thực tập ở đây không? Vì sao?**  
  Có. Chương trình có lộ trình rõ ràng, có credit để thực hành trên AWS thật và có dự án cuối khóa đủ lớn để áp dụng gần như toàn bộ kiến thức đã học.

### Đề xuất & mong muốn

- Bổ sung một buổi hướng dẫn về **quản lý chi phí và dọn tài nguyên** (Budgets, tag, teardown) ngay từ những tuần đầu, để thực tập sinh tránh tốn credit ngoài ý muốn.
- Có thêm các buổi **review kiến trúc** giữa kỳ với mentor để phát hiện sớm những lựa chọn không phù hợp (như WebSocket trên Lambda) trước khi triển khai.
- Chia sẻ thêm template hoặc ví dụ mẫu về **IaC, CI/CD và bảo mật** (WAF, secret, least privilege) để các nhóm có điểm xuất phát chung.
- Mình mong muốn được tiếp tục tham gia các chương trình tiếp theo của FCAJ, đặc biệt là các nội dung về AI/ML trên AWS để kết hợp với chuyên ngành của mình.
