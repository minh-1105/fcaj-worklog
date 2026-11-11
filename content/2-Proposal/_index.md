---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Student Flea Market
## A Cloud-Native Secondhand Marketplace for Students

### 1. Executive Summary

Student Flea Market is an online marketplace where university students can buy, sell, or exchange textbooks, study materials, electronic devices, dormitory supplies, and other pre-owned items. The project addresses a common problem: listings are often scattered across social networks and chat groups, making them difficult to search, compare, manage, and verify.

The solution combines a responsive Next.js frontend, an ASP.NET Core backend, and managed AWS services. The frontend supports product discovery, authentication, listing management, and an administrative interface. The backend provides protected REST APIs, enforces business rules, stores transactional data in PostgreSQL, and issues short-lived presigned requests that allow browsers to upload images directly to Amazon S3. An event-driven Lambda function converts uploaded images into optimized WebP variants, which are then delivered through Amazon CloudFront.

The first release focuses on a marketplace MVP: registration and login, category browsing, search and filtering, seller-owned listing creation and updates, and secure image processing. Exchange requests, transactions, chat, reviews, reporting, and payments are included in the domain model but are planned for later phases.

### 2. Problem Statement

#### Current Problem

Students commonly use social networks, messaging groups, or personal contacts to exchange pre-owned items. This approach has several limitations:

- Posts quickly become buried and cannot be filtered through a consistent structure.
- Descriptions, item condition, prices, and contact details do not follow a common standard.
- Buyers find it difficult to compare products or determine whether an item is still available.
- Sellers do not have a dedicated workflow for managing listings and images.
- Identity verification, ownership control, moderation, and reporting are limited.
- Uploading large images through a traditional API increases application server load and bandwidth usage.

#### Proposed Solution

Student Flea Market provides a centralized platform where students can register, sign in, browse and filter products, manage listings, upload images securely, and contact sellers. Later phases will add exchanges, transactions, messaging, reviews, reporting, and payments.

The platform follows a cloud-native design. Request-driven components use managed, consumption-based services, while PostgreSQL provides transactional consistency for users, products, exchanges, transactions, conversations, reviews, and reports.

#### Benefits

- **Students:** find suitable products and save money more quickly within a familiar community.
- **Sellers:** use a clear workflow for publishing listings and managing images.
- **Campus community:** encourage the reuse of textbooks and equipment while reducing waste.
- **Administrators:** gain a foundation for moderation, reporting, and policy enforcement.
- **Development team:** work with a modular system that can be tested using PostgreSQL and LocalStack before deployment to AWS.

### 3. Project Scope

#### MVP Scope

1. Account registration and JWT-based login.
2. Product categories and paginated listings.
3. Search and filtering by category.
4. Create, view, update, and delete listings.
5. Ownership checks so that only the seller can modify a listing.
6. Direct image uploads using presigned S3 POST requests.
7. Automatic image validation and conversion to optimized WebP variants.
8. Health checks, structured logging, validation, and consistent error responses.
9. A responsive interface supporting Vietnamese and English.
10. An initial administrative interface and policy pages.

#### Extended Scope

The data model already anticipates favorites, exchange requests, conversations, transactions, payments, reviews, violation reports, account recovery, product recommendations, and analytics. These capabilities are not part of the first production MVP.

### 4. Solution Architecture

{{<mermaid align="left">}}
graph TD
    U[Student or administrator] -->|HTTPS| A[AWS Amplify Hosting<br/>Next.js static frontend]
    A -->|REST and JWT| G[Amazon API Gateway<br/>HTTP API]
    G --> B[AWS Lambda<br/>ASP.NET Core API]
    B --> R[(Amazon RDS<br/>PostgreSQL)]
    B --> S[AWS Secrets Manager]
    B -->|Presigned POST| U
    U -->|Direct upload| RAW[Amazon S3<br/>Raw images]
    RAW -->|ObjectCreated event| P[AWS Lambda<br/>Image Processor]
    P -->|WebP variants| PROCESSED[Amazon S3<br/>Processed images]
    PROCESSED --> C[Amazon CloudFront<br/>Origin Access Control]
    C -->|Cached images| U
    P --> Q[Amazon SQS<br/>Failure queue]
    G --> CW[Amazon CloudWatch]
    B --> CW
    P --> CW
{{< /mermaid >}}

#### API Request Flow

1. The user opens the static Next.js application hosted on AWS Amplify.
2. The frontend calls API Gateway over HTTPS.
3. API Gateway invokes the ASP.NET Core API as an on-demand Lambda event function.
4. The API validates the request, verifies the JWT, checks ownership, and reads or writes PostgreSQL data through Entity Framework Core.
5. Logs and operational metrics are sent to CloudWatch.

HTTP API is appropriate because the MVP requires REST/JSON routing and JWT-bearing requests but does not yet require usage plans, API keys, response transformation, or API caching. Lambda On-Demand is suitable for uneven early-stage traffic and avoids maintaining an application server when no requests are being processed.

#### Image Upload and Processing Flow

1. An authenticated seller requests permission to upload an image for a listing.
2. The API validates the seller, listing, MIME type, file size, and image count.
3. The API returns a short-lived presigned S3 POST policy with a fixed object key.
4. The browser uploads the image directly to the **raw/** prefix in a private S3 bucket.
5. An S3 ObjectCreated event invokes the image-processing Lambda function.
6. The Lambda function creates **thumbnail.webp** and **large.webp** in the **processed/** prefix.
7. CloudFront reads only processed images through Origin Access Control and caches them close to users.
8. Failed events are sent to an encrypted SQS failure queue.

### 5. Technologies and AWS Services

| Component | Technology or service | Responsibility |
|---|---|---|
| Web frontend | Next.js 16, React 19, TypeScript | Responsive marketplace interface |
| UI and state | Tailwind CSS, shadcn/ui, TanStack Query, Zustand | UI, server state, and client state |
| Frontend hosting | AWS Amplify Hosting | Static hosting, HTTPS, CDN, and branch-based deployment |
| Public API | Amazon API Gateway HTTP API | HTTPS entry point, routing, CORS, and throttling |
| Backend | ASP.NET Core 8 on AWS Lambda | Authentication, validation, catalog, ownership, and presigning |
| Relational data | Amazon RDS for PostgreSQL | Users, products, transactions, messages, and reports |
| Data access | Entity Framework Core and Npgsql | Migrations, constraints, and queries |
| Media storage | Amazon S3 | Private raw and processed images |
| Media processing | AWS Lambda with Magick.NET | Image verification and WebP generation |
| Media delivery | Amazon CloudFront with OAC | Secure, cached image delivery |
| Secret storage | AWS Secrets Manager | Database credentials and application secrets |
| Failure handling | Amazon SQS | Dead-letter destination for image-processing failures |
| Observability | Amazon CloudWatch | Logs, metrics, retention, and alarms |
| Infrastructure as Code | AWS SAM and CloudFormation | Repeatable AWS deployment |
| Local AWS emulation | LocalStack | Low-cost testing of S3 and the image pipeline |

### 6. Data Design

The relational model includes ApplicationUser, Category, Item, ItemImage, Favorite, ExchangeRequest, Transaction, Payment, Conversation, Message, Review, and Report. Indexes and constraints enforce uniqueness, non-negative prices, valid ratings, ownership relationships, and efficient queries. Images are stored in object storage; the database stores references instead of binary image data.

### 7. Security Design

- Separate the deployment identity from Lambda runtime roles in accordance with least privilege.
- Protect seller operations with JWTs; the backend obtains the user ID from the verified token.
- Never commit database credentials or signing secrets to source control.
- Keep S3 buckets private, encrypted, protected by Block Public Access, and configured with ACLs disabled.
- Restrict presigned uploads by expiration time, object key, content type, and file size.
- Allow CloudFront OAC to read only processed images.
- Restrict CORS to the required HTTPS frontend origins.
- Do not log passwords, tokens, secrets, or complete presigned URLs.
- Use CloudWatch to monitor API errors, Lambda failures, throttling, duration, and the failure queue.
- Enforce administrative permissions in the backend rather than relying only on the UI.

### 8. Implementation Plan

| Phase | Weeks | Main activities | Deliverables |
|---|---:|---|---|
| Requirements and design | 1–3 | Define the MVP, architecture, data model, and API contract | Requirements, diagrams, ERD, and API contract |
| Backend foundation | 4–6 | Clean Architecture, EF Core, Identity/JWT, category and item CRUD | API, validation, tests, and Swagger |
| Frontend and integration | 7–9 | Marketplace UI, authentication state, filtering, API client, and seller workflow | Responsive frontend integrated with the API |
| Media and cloud readiness | 10–11 | Presigned uploads, LocalStack, image Lambda, S3/CloudFront IaC, and IAM review | Media pipeline and validated templates |
| Testing and handover | 12 | End-to-end checks, security review, observability, and documentation | Test report, runbook, deployment guide, and demo |

### 9. Budget Estimation Method

The final monthly cost will be calculated with AWS Pricing Calculator for the selected Region after traffic and retention assumptions are finalized. The estimate will include Amplify builds and data transfer, API Gateway requests, Lambda duration and memory, RDS instances, storage and backups, S3 storage and requests, CloudFront delivery, Secrets Manager, CloudWatch, and SQS.

PostgreSQL is expected to be the main fixed cost, while request-based services scale with usage. Development costs are reduced through LocalStack, short log-retention periods, S3 lifecycle policies, resource tags, AWS Budgets, and teardown of temporary environments. Free Tier allowances or credits are temporary discounts, not a long-term operating budget.

### 10. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Low user adoption | High | Pilot with one student group and prioritize simple listing and search flows |
| Fraudulent listings | High | Ownership rules, reporting, moderation, account status, and audit logs |
| Unauthorized access | High | JWTs, least-privilege IAM, private RDS networking, and negative tests |
| Malicious image uploads | High | Presigned constraints, MIME and magic-byte checks, limits, re-encoding, and private raw storage |
| Duplicate or failed events | Medium | Idempotent object keys, deterministic output paths, retries, alarms, and SQS |
| Unexpected costs | Medium | Budgets, alarms, lifecycle policies, log retention, and cost reviews |
| Database disruption | High | Backups, restore tests, Multi-AZ when appropriate, migrations, and recovery objectives |
| Excessive scope growth | Medium | Separate the MVP from future features and define acceptance criteria |

### 11. Current Implementation Status

The repository currently includes:

- a Next.js interface with authentication, product search, listing creation, policy pages, and an administrative shell;
- an ASP.NET Core 8 solution separated into API, Application, Domain, Infrastructure, ImageProcessor, and Tests projects;
- JWT registration and login, category queries, product search, and seller-authorized item CRUD operations;
- EF Core PostgreSQL migrations and a marketplace domain model;
- contracts and validation for presigned S3 uploads;
- a .NET image processor invoked by S3 and covered by automated tests;
- LocalStack initialization for the media pipeline;
- SAM/CloudFormation resources for private S3 storage, the image-processing Lambda, CloudFront OAC, alarms, and a failure queue; and
- frontend linting, type checking, static export, and Amplify build configuration.

Full AWS deployment remains the next milestone. The Lambda hosting adapter for the API, API Gateway integration, RDS environment, secrets, production origins, and environment-specific IAM must be completed and verified before production use.

### 12. Expected Outcomes

- A usable marketplace MVP for a student community.
- Secure authentication and listing-management flows.
- Direct uploads and optimized image delivery.
- A maintainable Clean Architecture backend and typed frontend.
- Reproducible infrastructure and a local cloud-emulation environment.
- Operational, security, and cost-control documentation.
- A foundation for exchanges, messaging, reviews, moderation, and payments.

### References

- [AWS Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/)
- [Amazon API Gateway HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [AWS Lambda with ASP.NET Core](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-asp.html)
- [Amazon RDS for PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [Amazon S3 presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
- [CloudFront Origin Access Control](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [AWS Secrets Manager with .NET](https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets-net-sdk.html)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/)
