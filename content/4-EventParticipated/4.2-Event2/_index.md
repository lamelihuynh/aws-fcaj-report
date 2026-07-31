---
title: "Event 2"
date: 2026-07-11
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# SUMMARY REPORT: AWS TECH MEETUP & COMMUNITY KNOWLEDGE SHARING

## Event Objectives

- **Objectives:**
  - Share practical lessons about system reliability, monitoring, incident alerting and web application security on AWS.
  - Provide a clear preparation direction for the AWS Certified Cloud Practitioner CLF-C02 exam.
  - Discuss how Generative AI and AI Agents can support DevSecOps activities such as design review, code review and automated penetration testing.
  - Create a technical exchange space for AWS First Cloud AI Journey members, infrastructure engineers and security practitioners.

## Speakers

- **Speakers:**
  - **Nguyen Huynh Son:** Infrastructure Support Engineer at Endava, Ex-Infrastructure Reliability Engineer at SPS, Member of AWS Student Builder Group HUFLIT.
  - **Ngo Le Tan Huy:** Speaker sharing the AWS Cloud Practitioner certification roadmap.
  - **Nguyen Tuan Thinh:** DevOps/DevSecOps/Cloud Engineer at Styl Solutions, Member of First Cloud AI Journey.

## Key Highlights

### Session 1: SLA and Monitoring - From SLA to Monitoring What Really Matters

**Speaker:** Nguyen Huynh Son

The first session focused on a common misunderstanding in operations: a system can look healthy from the infrastructure dashboard but still fail from the user's point of view. SLA is not only a contractual target; it is also a signal that the engineering team must translate into monitoring, alerting and improvement activities.

Important points from the session:

- **SLA:** A service-level commitment between the provider and users or customers.
- **Monitoring:** A risk management activity that helps detect weak signals before the SLA is affected.
- **Risk loop:** Identify risk, monitor signals, respond through alarms or SOPs, then review and improve.
- **Monitoring pyramid:** Cloud provider layer, infrastructure layer, application layer, business layer and customer experience layer.
- **Key message:** Healthy infrastructure does not always mean healthy user experience.

One memorable example was a system with normal CPU, healthy ALB targets and a `/health` endpoint returning `200 OK`, while real users still failed to log in because the application could not connect to the RDS database. This showed why monitoring should include business custom metrics such as login success, order success or payment failure, not only CPU and memory.

The suggested alerting flow was:

1. Emit custom metrics for important user actions, such as `LoginFailure`.
2. Configure a CloudWatch Alarm with a meaningful threshold.
3. Send alerts through SNS Topic to Email or Slack.
4. Review the incident after response and update the monitoring design.

The session also reminded participants of Dr. Werner Vogels' operation mindset: systems should be designed with failure in mind, because failures are normal in distributed systems.

### Session 2: Inside The Exam - AWS Certified Cloud Practitioner CLF-C02

**Speaker:** Ngo Le Tan Huy

The second session explained how to approach the AWS Certified Cloud Practitioner exam. The speaker did not only list exam facts, but also gave a practical way to study cloud services through keywords, service responsibilities and common traps in multiple-choice questions.

The exam overview included:

- **Question count:** 65 multiple-choice questions.
- **Duration:** 90 minutes, with extra time available for non-native English speakers.
- **Passing score:** 700/1000.
- **Validity:** 3 years.
- **Domains:**
  - Cloud Concepts: 24%
  - Security and Compliance: 30%
  - Cloud Technology and Services: 34%
  - Billing, Pricing and Support: 12%

The Shared Responsibility Model was one of the most important ideas:

- AWS is responsible for **Security OF the Cloud**, including physical facilities, hardware and global infrastructure.
- Customers are responsible for **Security IN the Cloud**, including data, IAM, operating system patching, security groups and application-level configuration.

Practical exam preparation advice:

- Link each AWS service to one or two keywords, such as **SQS = decoupling**, **CloudFront = content delivery**, **IAM = identity and permission**.
- When reviewing mock tests, analyze why the correct option is right and why the other options are wrong.
- Remove fake or unrelated service names first to improve the chance of selecting the correct answer.
- For CLF-C02, prefer simple and high-level answers instead of over-engineering the solution.

### Session 3: Securing Your Web Apps With AWS Security Agent

**Speaker:** Nguyen Tuan Thinh

The third session introduced how AI Agent technology can support modern application security. Traditional penetration testing is expensive, slow and difficult to repeat frequently. The session presented AWS Security Agent, also referred to as Frontier Agent, as an AI-assisted approach for automated security review and testing.

The main difference from a normal chatbot is that the agent is designed to verify findings. Instead of only saying that a vulnerability may exist, it can plan and execute controlled security tests to produce verifiable evidence.

The security lifecycle discussed in the session:

- **Design Review:** Review architecture documents, Markdown design notes or Terraform files before implementation; compare the design with standards such as PCI DSS, NIST CSF and AWS Well-Architected.
- **Code Review:** Integrate with GitHub or GitLab pull requests, comment directly on risky lines of code and suggest automated fixes.
- **Automated Pentesting:** Test a running application with multi-step exploit chains such as IDOR followed by XSS, then export attack evidence and diagrams.

The session also discussed cost and limitations:

- Traditional pentesting can cost around 5,000 to 20,000 USD per assessment and may take weeks.
- The AI Agent model uses pay-as-you-go pricing, mentioned as 50 USD per task-hour in the session.
- A practical project can reduce testing cost to roughly 1,500 to 2,500 USD, depending on scope.
- The agent still has limitations with MFA, biometrics, mTLS and complex business-logic fraud.

## Comparative Discussion

| Criteria | Traditional Approach | AWS / AI-assisted Approach |
| :--- | :--- | :--- |
| **Monitoring Target** | Mainly server and infrastructure metrics such as CPU, RAM and disk. | Includes user journey and business metrics such as login, order and payment success. |
| **Incident Detection** | Often starts after user complaints or manual investigation. | CloudWatch Alarm and SNS can notify the team earlier through Email or Slack. |
| **Security Review Timing** | Security testing is usually done near the end of development. | Shift-left approach reviews design and code earlier in the lifecycle. |
| **Pentest Effort** | Manual, slow and dependent on expert availability. | Agent-assisted testing can repeat checks and produce verifiable findings. |
| **Certification Study** | Memorize many service names without clear grouping. | Map services to keywords, responsibilities and exam domains. |

## Lessons Learned

### Monitoring and Reliability

- A green dashboard is not enough if users still cannot complete important actions.
- Monitoring should move from infrastructure-only metrics to user-centric and business-centric signals.
- CloudWatch custom metrics, alarms and SNS notifications can create an early warning path before the incident becomes visible to customers.

### Security and DevSecOps

- Security work should start from architecture and code review, not only from final pentesting.
- AI Agents can reduce repetitive security review effort, but human engineers still need to validate context, business logic and risk.
- The Shared Responsibility Model is a practical security boundary, not only exam theory.

### Certification and Personal Learning

- CLF-C02 preparation should focus on concepts, service positioning and keyword recognition.
- Reviewing wrong answers is more valuable than only counting mock test scores.
- The exam strategy is also useful for real work because it trains clear service selection and responsibility mapping.

## Applying to Work

- Add custom application metrics to personal or team projects, especially login, checkout, order and error-rate metrics.
- Review IAM policies and security groups with the principle of least privilege.
- Use GitHub Actions or similar CI/CD checks to scan for secret leaks and common code issues early.
- Build an AWS certification note set based on service keywords, exam domains and common wrong-answer patterns.
- When designing a cloud application, include monitoring and security review as part of the first architecture draft.

## Event Experience

- **Practical Experience:** The event connected three topics that often appear separately: monitoring, certification and application security. Seeing them together made the AWS learning path more complete.
- **Technical Impression:** The monitoring example was easy to remember because it showed a real gap between infrastructure health and user experience. It changed the way I think about what should be measured in a cloud system.
- **Community Value:** The event gave participants a chance to hear from engineers working in infrastructure and DevSecOps roles, making the discussion more practical than only reading documentation.
- **Personal Takeaway:** The most useful lesson was to design systems from the user's outcome backward: monitor what users actually need, secure the workflow early and understand service responsibilities clearly.

## Key Takeaways

- Monitoring should be designed around real user journeys, not only infrastructure resource usage.
- AWS certification study is more effective when each service is tied to keywords and responsibility boundaries.
- AI-assisted security testing can speed up DevSecOps, but human validation remains necessary for business logic and production risk.
- The Shared Responsibility Model should guide both exam preparation and real AWS environment review.

## Some Event Photos

![event2](/images/event2.jpg)
