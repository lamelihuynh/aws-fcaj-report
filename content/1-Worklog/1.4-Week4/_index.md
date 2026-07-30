---
title: "Week 4 Worklog"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Build an ECR image workflow with traceable tags.
* Connect image scanning findings with promotion rules.

### Weekly Implementation Plan:

| Day | Tasks | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Mon | - Studied ECR private repositories, repository policy and account/region boundaries. | 06/07/2026 | 06/07/2026 | [Module 7 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Week 4 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Docker deployment on AWS](https://000015.awsstudygroup.com/vi/) |
| Tue | - Practiced ECR login, Docker tag/push/pull and common credential scope issues. | 07/07/2026 | 07/07/2026 | [Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Week 4 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Docker deployment on AWS](https://000015.awsstudygroup.com/vi/) |
| Wed | - Defined image tags with commit SHA, branch/environment and immutable release labels. | 08/07/2026 | 08/07/2026 | [Module 7 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Week 4 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thu | - Compared ECR scan-on-push with pipeline scans and promotion blocking rules. | 09/07/2026 | 09/07/2026 | [Module 5 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%205/module-05-ly-thuyet-security-iam-kms-detection.md)<br>[Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Week 4 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/)<br>[000018 - AWS Security Hub basics](https://000018.awsstudygroup.com/vi/) |
| Fri | - Finalized the build-scan-auth-push-digest checklist for Jenkins integration. | 10/07/2026 | 10/07/2026 | [Lab 15 Docker/ECR](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-15-docker-ecr-image-workflow.md)<br>[Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Week 4 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-04-ecr-container-image-security.md)<br>[000015 - Docker deployment on AWS](https://000015.awsstudygroup.com/vi/)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/) |

### Week 4 Outcomes:

* The ECR workflow was mapped from image build, tagging and scanning to push and digest verification.
* Release evidence was connected to image tags and scan results, not only successful push commands.
