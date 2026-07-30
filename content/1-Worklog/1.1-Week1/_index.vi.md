---
title: "Worklog Tuần 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Nắm yêu cầu báo cáo FCAJ, nền tảng tài khoản AWS và thói quen kiểm soát chi phí.
* Thực hành truy cập an toàn bằng IAM Identity Center và credential CLI tạm thời.
* Xây dựng baseline VPC/networking cho kiến trúc ECS và workshop các tuần sau.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tham gia onboarding FCAJ, đọc phạm vi báo cáo, kiểm tra Free Tier/credit và ghi lại thói quen cost guardrail. | 15/06/2026 | 15/06/2026 | [Module 1 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/module-01-ly-thuyet-cloud-account-identity.md)<br>[Lab 01 Free Tier/Budgets](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/Hands-on%20Labs/lab-01-free-tier-budget-guardrail.md)<br>[Minh chứng tuần 1](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-01-nen-tang-cloud-iam-vpc.md)<br>[000001 - AWS Free Tier 2025](https://000001.awsstudygroup.com/vi/)<br>[000007 - Quản lý chi phí với AWS Budgets](https://000007.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành IAM Identity Center với user, group, permission set và credential AWS CLI tạm thời. | 16/06/2026 | 16/06/2026 | [Module 1 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/module-01-ly-thuyet-cloud-account-identity.md)<br>[Lab 12 IAM Identity Center](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%201/Hands-on%20Labs/lab-12-iam-identity-center.md)<br>[Minh chứng tuần 1](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-01-nen-tang-cloud-iam-vpc.md)<br>[000012 - AWS IAM Identity Center](https://000012.awsstudygroup.com/vi/)<br>[000002 - Quản trị quyền truy cập với AWS IAM](https://000002.awsstudygroup.com/vi/) |
| Thứ 4 | - Thiết kế VPC nền tảng với public/private subnet, route table, Internet Gateway và NAT Gateway. | 17/06/2026 | 17/06/2026 | [Module 2 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/module-02-ly-thuyet-vpc-networking-security.md)<br>[Lab 03 VPC](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/Hands-on%20Labs/lab-03-vpc-site-to-site-vpn.md)<br>[Minh chứng tuần 1](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-01-nen-tang-cloud-iam-vpc.md)<br>[000003 - Amazon VPC và Site-to-Site VPN](https://000003.awsstudygroup.com/vi/) |
| Thứ 5 | - So sánh Security Group, NACL và VPC Flow Logs như một luồng troubleshooting mạng. | 18/06/2026 | 18/06/2026 | [Module 2 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/module-02-ly-thuyet-vpc-networking-security.md)<br>[Lab 74 VPC Flow Logs](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/Hands-on%20Labs/lab-74-vpc-flow-logs.md)<br>[Minh chứng tuần 1](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-01-nen-tang-cloud-iam-vpc.md)<br>[000074 - Giám sát hạ tầng mạng với VPC Flow Logs](https://000074.awsstudygroup.com/vi/) |
| Thứ 6 | - Tổng hợp VPC Peering, Transit Gateway, VPN, Direct Connect và các tình huống dùng Load Balancer. | 19/06/2026 | 19/06/2026 | [Module 2 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%202/module-02-ly-thuyet-vpc-networking-security.md)<br>[Minh chứng tuần 1](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-01-nen-tang-cloud-iam-vpc.md)<br>[000003 - Amazon VPC và Site-to-Site VPN](https://000003.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 1:

* Thiết lập được baseline về tài khoản, định danh và chi phí trước khi đi vào phần networking.
* Giải thích được vì sao credential tạm thời và cost guardrail nên được cấu hình trước khi tạo tài nguyên demo.
