---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Đi sâu IAM, KMS, Security Hub và GuardDuty.
* So sánh dịch vụ dữ liệu AWS và chuẩn bị cho CI/CD container.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Ôn Shared Responsibility, IAM policy evaluation, role và permission boundary. | 29/06/2026 | 29/06/2026 | [Module 5 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/module-05-ly-thuyet-security-iam-kms-detection.md)<br>[Lab 02 IAM nền tảng](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/Hands-on%20Labs/lab-02-iam-user-role-baseline.md)<br>[Minh chứng tuần 3](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-03-security-data-cicd.md)<br>[000002 - Quản trị quyền truy cập với AWS IAM](https://000002.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành permission boundary, role condition và EC2 instance profile thay cho access key dài hạn. | 30/06/2026 | 30/06/2026 | [Module 5 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/module-05-ly-thuyet-security-iam-kms-detection.md)<br>[Lab 30 Permission Boundary](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-30-permission-boundary.md)<br>[Lab 44 IAM Role Condition](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-44-iam-role-condition.md)<br>[Lab 48 EC2 Instance Profile](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-48-ec2-instance-profile.md)<br>[Minh chứng tuần 3](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-03-security-data-cicd.md)<br>[000030 - IAM Permission Boundary](https://000030.awsstudygroup.com/vi/)<br>[000044 - IAM Role & Condition](https://000044.awsstudygroup.com/vi/)<br>[000048 - Ứng dụng truy cập dịch vụ AWS với IAM Role](https://000048.awsstudygroup.com/vi/) |
| Thứ 4 | - Học KMS encryption, CloudTrail audit, Security Hub finding và GuardDuty signal. | 01/07/2026 | 01/07/2026 | [Module 5 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/module-05-ly-thuyet-security-iam-kms-detection.md)<br>[Lab 33 KMS/S3 audit](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-33-kms-s3-cloudtrail-athena.md)<br>[Lab 18 Security Hub](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-18-security-hub-baseline.md)<br>[Lab 98 GuardDuty](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-98-guardduty-finding-practice.md)<br>[Minh chứng tuần 3](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-03-security-data-cicd.md)<br>[000033 - Mã hóa lưu trữ với AWS KMS](https://000033.awsstudygroup.com/vi/)<br>[000018 - Bắt đầu với AWS Security Hub](https://000018.awsstudygroup.com/vi/)<br>[000098 - Thực hành Amazon GuardDuty](https://000098.awsstudygroup.com/vi/) |
| Thứ 5 | - So sánh RDS, Aurora, DynamoDB, Redshift, S3 data lake và ElastiCache theo workload. | 02/07/2026 | 02/07/2026 | [Module 6 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%206/module-06-ly-thuyet-rds-cache-data-services.md)<br>[Lab 05 RDS](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%206/Hands-on%20Labs/lab-05-rds-application-backup.md)<br>[Lab 61 ElastiCache](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%206/Hands-on%20Labs/lab-61-elasticache-redis.md)<br>[Minh chứng tuần 3](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-03-security-data-cicd.md)<br>[000005 - Bắt đầu với Amazon RDS](https://000005.awsstudygroup.com/vi/)<br>[000061 - Amazon ElastiCache Redis](https://000061.awsstudygroup.com/vi/) |
| Thứ 6 | - Đọc luồng CI/CD ECS để chuẩn bị cho ECR và Fargate. | 03/07/2026 | 03/07/2026 | [Module 7 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Minh chứng tuần 3](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-03-security-data-cicd.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 3:

* Đọc được policy IAM, trust relationship, quyền KMS và security finding ở mức thực hành.
* Có bản đồ lựa chọn ban đầu cho RDS, cache, object storage và CI/CD container.
