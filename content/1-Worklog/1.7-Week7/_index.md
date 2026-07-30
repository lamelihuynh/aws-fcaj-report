---
title: "Week 7 Worklog"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Build observability for ECS/Lambda/report processing.
* Add cost guardrails and an incident drill.

### Weekly Implementation Plan:

| Day | Tasks | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Mon | - Reviewed CloudWatch Container Insights thinking and ECS awslogs driver expectations. | 27/07/2026 | 27/07/2026 | [Module 8 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/module-08-ly-thuyet-serverless-observability-iac.md)<br>[Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Week 7 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/) |
| Tue | - Defined log group naming, retention and Logs Insights queries for app and scan errors. | 28/07/2026 | 28/07/2026 | [Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Lab 85 Serverless monitoring](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-85-serverless-monitoring-xray.md)<br>[Week 7 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/)<br>[000085 - Serverless monitoring with CloudWatch and X-Ray](https://000085.awsstudygroup.com/vi/) |
| Wed | - Planned dashboards for CPU, memory, task count, target health and network signals. | 29/07/2026 | 29/07/2026 | [Lab 74 VPC Flow Logs](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/Hands-on%20Labs/lab-74-vpc-flow-logs.md)<br>[Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Week 7 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000074 - VPC Flow Logs monitoring](https://000074.awsstudygroup.com/vi/)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/) |
| Thu | - Created AWS Budgets notes with thresholds and cleanup cadence for demo resources. | 30/07/2026 | 30/07/2026 | [Lab 01 Free Tier/Budgets](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/Hands-on%20Labs/lab-01-free-tier-budget-guardrail.md)<br>[Week 7 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000007 - AWS Budgets cost control](https://000007.awsstudygroup.com/vi/) |
| Fri | - Wrote an incident drill covering failed service detection, logs, target health and rollback/scale down. | 31/07/2026 | 31/07/2026 | [Lab 31 Systems Manager](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/Hands-on%20Labs/lab-31-systems-manager-run-command.md)<br>[Lab 98 GuardDuty](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-98-guardduty-finding-practice.md)<br>[Week 7 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000031 - AWS Systems Manager](https://000031.awsstudygroup.com/vi/)<br>[000098 - Amazon GuardDuty hands-on](https://000098.awsstudygroup.com/vi/) |

### Week 7 Outcomes:

* An observability checklist was prepared for logs, metrics, alarms, dashboard panels and cost alerts.
* A basic incident drill was mapped from failed health check to rollback or scale down.
