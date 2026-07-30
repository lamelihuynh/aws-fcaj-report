---
title: "Worklog Tuần 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Xây dựng workflow ECR với image tag truy vết được.
* Gắn finding từ image scan với rule promote release.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Học ECR private repository, repository policy và boundary theo account/region. | 06/07/2026 | 06/07/2026 | [Module 7 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Minh chứng tuần 4](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Triển khai Docker với AWS](https://000015.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành ECR login, Docker tag/push/pull và lỗi credential scope. | 07/07/2026 | 07/07/2026 | [Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Minh chứng tuần 4](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Triển khai Docker với AWS](https://000015.awsstudygroup.com/vi/) |
| Thứ 4 | - Định nghĩa image tag bằng commit SHA, branch/environment và release label bất biến. | 08/07/2026 | 08/07/2026 | [Module 7 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Minh chứng tuần 4](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thứ 5 | - So sánh ECR scan-on-push với pipeline scan và rule chặn promote. | 09/07/2026 | 09/07/2026 | [Module 5 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/module-05-ly-thuyet-security-iam-kms-detection.md)<br>[Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Minh chứng tuần 4](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/)<br>[000018 - Bắt đầu với AWS Security Hub](https://000018.awsstudygroup.com/vi/) |
| Thứ 6 | - Hoàn thiện checklist build-scan-auth-push-digest để tích hợp Jenkins. | 10/07/2026 | 10/07/2026 | [Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Minh chứng tuần 4](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Triển khai Docker với AWS](https://000015.awsstudygroup.com/vi/)<br>[000017 - CI/CD với ECS Container](https://000017.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 4:

* Nắm được workflow ECR từ build image, đặt tag, scan, push đến kiểm tra digest.
* Hiểu vì sao release evidence cần image tag và scan result, không chỉ là push image thành công.
