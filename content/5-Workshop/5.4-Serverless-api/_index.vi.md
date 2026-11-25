---
title : "API serverless: SSM, Lambda và API Gateway"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

Toàn bộ API ASP.NET Core chạy trong một hàm Lambda (`AddAWSLambdaHosting(LambdaEventSource.HttpApi)`) phía sau API Gateway HTTP API. Khi cold start, API nạp secret từ SSM Parameter Store, nên template không chứa giá trị secret nào.

#### Vì sao thiết kế như vậy

- **HTTP API** là đủ: routing REST/JSON, CORS và throttling (20 request/giây, burst 50), không cần API key hay usage plan.
- **Lambda on-demand** không tốn tiền khi không có traffic.
- **SSM SecureString** mã hóa secret bằng AWS managed key `aws/ssm`; role của Lambda chỉ đọc được đúng path của mình và chỉ giải mã qua SSM.

#### Nội dung

1. [Lưu secret vào SSM Parameter Store](5.4.1-store-secrets/)
2. [Deploy stack API](5.4.2-deploy-api/)
3. [Kiểm tra API](5.4.3-test-api/)
