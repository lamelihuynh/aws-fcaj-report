---
title: "Worklog Tuần 7"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Xây dựng observability cho ECS/Lambda/report processing.
* Bổ sung cost guardrail và incident drill.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Ôn CloudWatch Container Insights và kỳ vọng log driver awslogs cho ECS. | 27/07/2026 | 27/07/2026 | [Module 8 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/module-08-ly-thuyet-serverless-observability-iac.md)<br>[Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Minh chứng tuần 7](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thứ 3 | - Định nghĩa Log Group naming, retention và Logs Insights query cho lỗi app/scan. | 28/07/2026 | 28/07/2026 | [Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Lab 85 Serverless monitoring](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-85-serverless-monitoring-xray.md)<br>[Minh chứng tuần 7](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/)<br>[000085 - Monitoring serverless với CloudWatch và X-Ray](https://000085.awsstudygroup.com/vi/) |
| Thứ 4 | - Lập dashboard CPU, memory, task count, target health và network signal. | 29/07/2026 | 29/07/2026 | [Lab 74 VPC Flow Logs](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/Hands-on%20Labs/lab-74-vpc-flow-logs.md)<br>[Lab 08 CloudWatch](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-08-cloudwatch-logs-metrics-alarms.md)<br>[Minh chứng tuần 7](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000074 - Giám sát hạ tầng mạng với VPC Flow Logs](https://000074.awsstudygroup.com/vi/)<br>[000008 - AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/) |
| Thứ 5 | - Tạo ghi chú AWS Budgets với threshold và cleanup cadence cho tài nguyên demo. | 30/07/2026 | 30/07/2026 | [Lab 01 Free Tier/Budgets](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/Hands-on%20Labs/lab-01-free-tier-budget-guardrail.md)<br>[Minh chứng tuần 7](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000007 - Quản lý chi phí với AWS Budgets](https://000007.awsstudygroup.com/vi/) |
| Thứ 6 | - Viết incident drill: phát hiện service lỗi, đọc log, kiểm target health và rollback/scale down. | 31/07/2026 | 31/07/2026 | [Lab 31 Systems Manager](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/Hands-on%20Labs/lab-31-systems-manager-run-command.md)<br>[Lab 98 GuardDuty](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-98-guardduty-finding-practice.md)<br>[Minh chứng tuần 7](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-07-cloudwatch-cost-operations.md)<br>[000031 - AWS Systems Manager](https://000031.awsstudygroup.com/vi/)<br>[000098 - Thực hành Amazon GuardDuty](https://000098.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 7:

* Có checklist observability cho log, metric, alarm, dashboard và cost alert.
* Đi qua được một incident drill cơ bản từ health check lỗi tới rollback hoặc scale down.
