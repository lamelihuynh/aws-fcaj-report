---
title: "Week 6 Worklog"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Turn the deployment path into a controlled GitOps/release flow.
* Document rollback, drift and credential handling.

### Weekly Implementation Plan:

| Day | Tasks | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Mon | - Studied declarative manifests, Git as source of truth, reconciliation and rollback through version history. | 20/07/2026 | 20/07/2026 | [Module 9 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/module-09-ly-thuyet-architecture-devsecops-report.md)<br>[Lab 37 CloudFormation](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%208/Hands-on%20Labs/lab-37-cloudformation-baseline.md)<br>[Week 6 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/) |
| Tue | - Reviewed sync/health status concepts and how they apply to ECS/EKS-style deployments. | 21/07/2026 | 21/07/2026 | [Module 7 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/module-07-ly-thuyet-containers-ecr-ecs-cicd.md)<br>[Week 6 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/) |
| Wed | - Designed staging flow where Jenkins builds the image, updates the tag and verifies health. | 22/07/2026 | 22/07/2026 | [Lab 17 ECS CI/CD](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%207/Hands-on%20Labs/lab-17-ecs-cicd-pipeline.md)<br>[Week 6 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/) |
| Thu | - Designed production promotion with manual approval, controlled sync and rollback notes. | 23/07/2026 | 23/07/2026 | [Lab 09 DevSecOps checklist](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/Hands-on%20Labs/lab-09-devsecops-architecture-checklist.md)<br>[Week 6 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000017 - CI/CD with ECS Container](https://000017.awsstudygroup.com/vi/)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/) |
| Fri | - Finalized the release runbook for credential handling, drift checks and incident notes. | 24/07/2026 | 24/07/2026 | [Module 9 theory](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%209/module-09-ly-thuyet-architecture-devsecops-report.md)<br>[Week 6 evidence](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-06-gitops-release-control.md)<br>[000037 - AWS CloudFormation](https://000037.awsstudygroup.com/vi/)<br>[000031 - AWS Systems Manager](https://000031.awsstudygroup.com/vi/) |

### Week 6 Outcomes:

* GitOps was documented as a controlled release practice, not only as another deployment tool.
* A release runbook was drafted with staging, production approval, rollback and drift checks.
