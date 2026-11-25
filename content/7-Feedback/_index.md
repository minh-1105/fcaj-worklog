---
title: "Sharing and Feedback"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

> Below are my personal reflections on my experience in the First Cloud AI Journey program, shared to help the FCAJ team improve the program for future cohorts.

### Overall Evaluation

**1. Working Environment**  
The environment is friendly and open, and people are always willing to help when I run into difficulties. The 12-week learning path is clearly structured — from cloud fundamentals, IAM, compute, storage, and networking to serverless, IaC, monitoring, and security — so I always knew what to focus on each week.

**2. Support from Mentor / Team Admin**  
The mentor explains clearly and encourages me to research and try things myself before asking. This approach helped me solve many problems on my own while building Student Market, such as choosing Supabase instead of RDS to save credit, or moving realtime chat to ECS Fargate after discovering that SignalR cannot run on API Gateway + Lambda. The admin team handled program procedures and documents well.

**3. Relevance of Work to Academic Major**  
My major is Artificial Intelligence, so most of the cloud, infrastructure, and security knowledge was new to me. Still, my programming background from university helped me catch up, and cloud knowledge is an essential foundation for deploying AI applications in the future.

**4. Learning & Skill Development Opportunities**  
This is the part I value most. Through Student Market, I went through a complete cycle: designing the architecture, writing IaC with AWS SAM, deploying to production, configuring WAF, SSM Parameter Store, CloudWatch alarms, and CloudTrail, and tracking costs with AWS Budgets. Beyond technical skills, I also learned to write worklogs and deployment documentation and to collaborate with teammates through Git.

**5. Company Culture & Team Spirit**  
Everyone respects each other and works seriously while keeping things relaxed. Teammates are willing to share knowledge and debug problems together.

**6. Internship Policies / Benefits**  
Receiving **$170 of AWS credit** to practice on a real account is a big plus; it taught me how to operate a real system instead of only reading theory. The program's events and workshops also kept me up to date with new knowledge.

---

### Additional Questions

- **What did you find most satisfying during your internship?**  
  Taking a real project all the way to production on AWS myself — from design to security and monitoring — and understanding why each service was chosen.
- **What do you think the company should improve for future interns?**  
  Provide early guidance on the limits of the AWS Free account plan (for example, the CloudFront Free plan cannot be subscribed, so WAF must be paid) and on how to request IAM permissions, since our team lost time waiting for permissions before we could deploy.
- **If recommending to a friend, would you suggest they intern here? Why or why not?**  
  Yes. The program has a clear learning path, credit to practice on real AWS, and a capstone project large enough to apply almost everything learned.

---

### Suggestions & Expectations

- Add a session on **cost management and resource cleanup** (Budgets, tagging, teardown) in the first weeks, so interns avoid spending credit unexpectedly.
- Hold mid-term **architecture reviews** with mentors to catch unsuitable choices early (such as WebSockets on Lambda) before deployment.
- Share more templates or reference examples for **IaC, CI/CD, and security** (WAF, secrets, least privilege) so teams have a common starting point.
- I would like to continue with future FCAJ programs, especially AI/ML on AWS, to combine with my major.
