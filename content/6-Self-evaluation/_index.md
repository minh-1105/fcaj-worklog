---
title: "Self-Assessment"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

During my internship at **Amazon Web Services Vietnam Co., Ltd.** (the **Workforce Bootcamp – First Cloud AI Journey** program) from **14/09/2026** to **14/12/2026**, I had the opportunity to learn, practice, and apply the knowledge acquired in school to a real-world working environment.

I participated in building **Student Flea Market (Student Market)** — a secondhand marketplace for students with a Next.js frontend, an ASP.NET Core backend, and serverless infrastructure on AWS — from architecture design and Infrastructure as Code to production deployment, security, monitoring, and cost optimization. Through this project, I improved my skills in **full-stack development, cloud architecture design, AWS security, system operations, technical documentation, and teamwork**.

In terms of work ethic, I always strived to complete tasks well, complied with workplace regulations, and actively engaged with colleagues to improve work efficiency.

### Technologies applied in the project

| Area | Technologies | What I accomplished |
|---|---|---|
| Frontend | Next.js 16, React 19, TypeScript, Tailwind CSS, shadcn/ui, TanStack Query, Zustand, React Hook Form, Zod | Built a responsive marketplace UI, managed server/client state, validated forms, used static export, and deployed on Vercel. |
| Backend | ASP.NET Core 8, Entity Framework Core, Npgsql, JWT, FluentValidation, Mapster, Serilog, SignalR | Structured the solution with Clean Architecture (API, Application, Domain, Infrastructure); implemented JWT + refresh tokens, email OTP, Google sign-in, realtime chat, and automated tests. |
| Database | PostgreSQL on Supabase | Designed the data model, ran EF Core migrations, connected Lambda through the transaction pooler, and backed up with `pg_dump`. |
| Compute & API | AWS Lambda, Amazon API Gateway (HTTP API) | Ran the whole .NET API on Lambda and configured CORS and throttling. |
| Media | Amazon S3, ImageProcessor Lambda (Magick.NET), Amazon CloudFront (OAC) | Direct uploads with presigned URLs, event-driven WebP conversion, and CDN delivery from a private bucket. |
| Security | AWS WAF, SSM Parameter Store + KMS, IAM, IAM Access Analyzer, AWS CloudTrail | Put the API behind CloudFront + WAF with an origin-verify header, moved secrets to SecureString, applied least privilege, and enabled an audit trail. |
| Monitoring | Amazon CloudWatch, Amazon SNS, Amazon SQS | Structured logs, alarms for errors/throttles/duration, a failure queue for image processing, and email alerts. |
| Containers | Amazon ECS Fargate, Application Load Balancer, Amazon ECR, AWS CodeBuild, Docker | Ran the SignalR hub on ARM64 Fargate, built images on CodeBuild, and allowed the ALB to accept traffic only from CloudFront. |
| IaC & DevOps | AWS SAM, AWS CloudFormation, LocalStack, PowerShell, Git/GitHub | Wrote the media, API, WAF, realtime, and build stacks; emulated S3/Lambda locally with LocalStack; scripted deploy and teardown. |
| Cost | AWS Budgets, Cost Explorer | Tracked the $170 credit, chose Vercel/Supabase over Amplify/RDS, and enabled the realtime stack only for demos. |

To objectively reflect on my internship period, I would like to evaluate myself based on the following criteria:

| No. | Criteria | Description | Good | Fair | Average |
| --- | --- | --- | --- | --- | --- |
| 1 | **Professional knowledge & skills** | Understanding of the field, applying knowledge in practice, proficiency with tools, work quality | ✅ | ☐ | ☐ |
| 2 | **Ability to learn** | Ability to absorb new knowledge and learn quickly | ☐ | ✅ | ☐ |
| 3 | **Proactiveness** | Taking initiative, seeking out tasks without waiting for instructions | ✅ | ☐ | ☐ |
| 4 | **Sense of responsibility** | Completing tasks on time and ensuring quality | ✅ | ☐ | ☐ |
| 5 | **Discipline** | Adhering to schedules, rules, and work processes | ☐ | ☐ | ✅ |
| 6 | **Progressive mindset** | Willingness to receive feedback and improve oneself | ☐ | ✅ | ☐ |
| 7 | **Communication** | Presenting ideas and reporting work clearly | ☐ | ✅ | ☐ |
| 8 | **Teamwork** | Working effectively with colleagues and participating in teams | ✅ | ☐ | ☐ |
| 9 | **Professional conduct** | Respecting colleagues, partners, and the work environment | ✅ | ☐ | ☐ |
| 10 | **Problem-solving skills** | Identifying problems, proposing solutions, and showing creativity | ☐ | ✅ | ☐ |
| 11 | **Contribution to project/team** | Work effectiveness, innovative ideas, recognition from the team | ✅ | ☐ | ☐ |
| 12 | **Overall** | General evaluation of the entire internship period | ✅ | ☐ | ☐ |

### Strengths

* Understood how to design a complete serverless system on AWS and deploy it to production with Infrastructure as Code.
* Weighed cost when choosing the architecture (Vercel, Supabase, and Lambda instead of Amplify, RDS, and EC2) to fit the credit budget.
* Focused on security: private buckets, secrets in SSM, WAF, least privilege, and an audit trail.
* Kept a complete worklog and deployment/operations documentation that the team can follow.

### Needs Improvement

* Strengthen discipline and strictly comply with the rules and schedules of the company or any organization.
* Improve problem-solving by analyzing technical limits earlier (for example, SignalR cannot run on API Gateway + Lambda) instead of discovering them after deployment.
* Add end-to-end and integration tests for real flows (Google sign-in, image upload, two-user chat) before release.
* Track technology lifecycles (the `dotnet8` runtime end of support, container image vulnerabilities) and update in time.
* Communicate better in daily work: report progress and agree on changes with the team before merging.
