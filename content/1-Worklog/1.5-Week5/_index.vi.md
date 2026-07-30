---
title: "Worklog Tuần 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Thiết kế runtime ECS Fargate cho workshop cuối kỳ.
* Dùng S3 và Lambda để tổng hợp security report.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - So sánh ECS Fargate với EC2/EKS và chọn runtime container serverless cho demo. | 13/07/2026 | 13/07/2026 | [Module 7 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Minh chứng tuần 5](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Triển khai ứng dụng trên Amazon ECS](https://000016.awsstudygroup.com/vi/)<br>[000067 - Monolith to Microservices với ECS/Fargate](https://000067.awsstudygroup.com/vi/) |
| Thứ 3 | - Thiết kế task definition, execution role, task role và cách xử lý secret. | 14/07/2026 | 14/07/2026 | [Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Lab 96 Secrets Manager](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-96-secrets-manager-fargate.md)<br>[Minh chứng tuần 5](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Triển khai ứng dụng trên Amazon ECS](https://000016.awsstudygroup.com/vi/)<br>[000096 - Secrets Manager với RDS và Fargate](https://000096.awsstudygroup.com/vi/) |
| Thứ 4 | - Vẽ ECS service networking qua subnet, Security Group, Target Group và ALB health check. | 15/07/2026 | 15/07/2026 | [Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Module 2 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/module-02-ly-thuyet-vpc-networking-security.md)<br>[Minh chứng tuần 5](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Triển khai ứng dụng trên Amazon ECS](https://000016.awsstudygroup.com/vi/)<br>[000003 - Amazon VPC và Site-to-Site VPN](https://000003.awsstudygroup.com/vi/) |
| Thứ 5 | - Thiết kế S3 report bucket với encryption, versioning, lifecycle và policy least privilege. | 16/07/2026 | 16/07/2026 | [Module 4 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/module-04-ly-thuyet-s3-storage-backup-dr.md)<br>[Lab 57 S3/CloudFront](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-57-s3-cloudfront-static-website.md)<br>[Lab 33 KMS/S3 audit](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-33-kms-s3-cloudtrail-athena.md)<br>[Minh chứng tuần 5](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000057 - Khởi đầu với Amazon S3](https://000057.awsstudygroup.com/vi/)<br>[000033 - Mã hóa lưu trữ với AWS KMS](https://000033.awsstudygroup.com/vi/)<br>[000069 - Thực hành bảo mật S3](https://000069.awsstudygroup.com/vi/) |
| Thứ 6 | - Thiết kế Lambda aggregator trigger bởi S3 report object mới và log finding ra CloudWatch. | 17/07/2026 | 17/07/2026 | [Module 8 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/module-08-ly-thuyet-serverless-observability-iac.md)<br>[Lab 66 Lambda/SAM](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-66-lambda-api-gateway-sam.md)<br>[Minh chứng tuần 5](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000066 - Serverless với Lambda, API Gateway và SAM](https://000066.awsstudygroup.com/vi/)<br>[000085 - Monitoring serverless với CloudWatch và X-Ray](https://000085.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 5:

* Vẽ được đường đi của ECS Fargate service từ ALB tới task log và IAM role.
* Có thiết kế đơn giản cho việc lưu scan report ở S3 và xử lý bằng Lambda.
