---
title: "Week 5 Worklog"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Design the ECS Fargate runtime for the final workshop.
* Use S3 and Lambda for security report aggregation.

### Weekly Implementation Plan:

| Day | Tasks | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Mon | - Compared ECS Fargate with EC2/EKS and selected a serverless container runtime for the demo. | 13/07/2026 | 13/07/2026 | [Module 7 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Week 5 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Amazon ECS application deployment](https://000016.awsstudygroup.com/vi/)<br>[000067 - Monolith to Microservices with ECS/Fargate](https://000067.awsstudygroup.com/vi/) |
| Tue | - Designed task definition fields, execution role, task role and secret handling. | 14/07/2026 | 14/07/2026 | [Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Lab 96 Secrets Manager](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-96-secrets-manager-fargate.md)<br>[Week 5 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Amazon ECS application deployment](https://000016.awsstudygroup.com/vi/)<br>[000096 - Secrets Manager with RDS and Fargate](https://000096.awsstudygroup.com/vi/) |
| Wed | - Mapped ECS service networking through subnets, security groups, target group and ALB health checks. | 15/07/2026 | 15/07/2026 | [Lab 16 ECS Fargate](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-16-ecs-fargate-alb-service.md)<br>[Module 2 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/module-02-ly-thuyet-vpc-networking-security.md)<br>[Week 5 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000016 - Amazon ECS application deployment](https://000016.awsstudygroup.com/vi/)<br>[000003 - Amazon VPC and Site-to-Site VPN](https://000003.awsstudygroup.com/vi/) |
| Thu | - Created the S3 report bucket model with encryption, versioning, lifecycle and least privilege policy. | 16/07/2026 | 16/07/2026 | [Module 4 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/module-04-ly-thuyet-s3-storage-backup-dr.md)<br>[Lab 57 S3/CloudFront](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-57-s3-cloudfront-static-website.md)<br>[Lab 33 KMS/S3 audit](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/Hands-on%20Labs/lab-33-kms-s3-cloudtrail-athena.md)<br>[Week 5 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000057 - Getting started with Amazon S3](https://000057.awsstudygroup.com/vi/)<br>[000033 - AWS KMS storage encryption](https://000033.awsstudygroup.com/vi/)<br>[000069 - S3 security practice](https://000069.awsstudygroup.com/vi/) |
| Fri | - Designed a Lambda aggregator triggered by new S3 report objects and logged findings to CloudWatch. | 17/07/2026 | 17/07/2026 | [Module 8 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/module-08-ly-thuyet-serverless-observability-iac.md)<br>[Lab 66 Lambda/SAM](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-66-lambda-api-gateway-sam.md)<br>[Week 5 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-05-ecs-fargate-s3-lambda.md)<br>[000066 - Serverless with Lambda, API Gateway and SAM](https://000066.awsstudygroup.com/vi/)<br>[000085 - Serverless monitoring with CloudWatch and X-Ray](https://000085.awsstudygroup.com/vi/) |

### Week 5 Outcomes:

* The ECS Fargate request path was mapped from ALB to task logs and IAM roles.
* A simple design was prepared for storing scan reports in S3 and processing them with Lambda.
