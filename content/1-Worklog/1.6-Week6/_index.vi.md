---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Chuyển deployment path thành GitOps/release flow có kiểm soát.
* Tài liệu hóa rollback, drift và quản lý credential.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Học manifest khai báo, Git là source of truth, reconciliation và rollback qua version history. | 20/07/2026 | 20/07/2026 | [Module 9 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/module-09-ly-thuyet-architecture-devsecops-report.md)<br>[Lab 37 CloudFormation](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-37-cloudformation-baseline.md)<br>[Minh chứng tuần 6](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/) |
| Thứ 3 | - Xem sync/health status và cách áp dụng vào deployment kiểu ECS/EKS. | 21/07/2026 | 21/07/2026 | [Module 7 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Minh chứng tuần 6](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thứ 4 | - Thiết kế staging flow: Jenkins build image, update tag và kiểm tra health. | 22/07/2026 | 22/07/2026 | [Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Minh chứng tuần 6](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thứ 5 | - Thiết kế promote production có manual approval, controlled sync và rollback note. | 23/07/2026 | 23/07/2026 | [Lab 09 DevSecOps checklist](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/Hands-on%20Labs/lab-09-devsecops-architecture-checklist.md)<br>[Minh chứng tuần 6](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/) |
| Thứ 6 | - Hoàn thiện release runbook về credential, drift check và incident note. | 24/07/2026 | 24/07/2026 | [Module 9 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/module-09-ly-thuyet-architecture-devsecops-report.md)<br>[Minh chứng tuần 6](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/)<br>[000031 - AWS Systems Manager](https://000031.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 6:

* Hiểu GitOps như một thói quen release có kiểm soát, không chỉ là thêm một tool deploy.
* Phác thảo được runbook gồm staging, production approval, rollback và drift check.
