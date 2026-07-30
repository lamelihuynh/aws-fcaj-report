---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Thực hành nền tảng EC2, EBS, AMI và Auto Scaling.
* So sánh object storage, file storage, hybrid storage và các mô hình DR.

### Kế hoạch triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Launch EC2 Linux/Windows, kiểm tra SSH/RDP và ghi lỗi Security Group thường gặp. | 22/06/2026 | 22/06/2026 | [Module 3 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/module-03-ly-thuyet-ec2-ebs-auto-scaling.md)<br>[Lab 04 EC2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/Hands-on%20Labs/lab-04-ec2-linux-windows-operations.md)<br>[Minh chứng tuần 2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-02-compute-storage-dr.md)<br>[000004 - Giới thiệu về Amazon EC2](https://000004.awsstudygroup.com/vi/) |
| Thứ 3 | - Thực hành EBS snapshot, AMI và Auto Scaling Group phía sau ALB. | 23/06/2026 | 23/06/2026 | [Module 3 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/module-03-ly-thuyet-ec2-ebs-auto-scaling.md)<br>[Lab 06 Auto Scaling](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%203/Hands-on%20Labs/lab-06-auto-scaling-load-balancer.md)<br>[Minh chứng tuần 2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-02-compute-storage-dr.md)<br>[000006 - FCJ Management với Auto Scaling Group](https://000006.awsstudygroup.com/vi/) |
| Thứ 4 | - Cấu hình S3 cơ bản, versioning, lifecycle và phân phối static qua CloudFront. | 24/06/2026 | 24/06/2026 | [Module 4 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/module-04-ly-thuyet-s3-storage-backup-dr.md)<br>[Lab 57 S3/CloudFront](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-57-s3-cloudfront-static-website.md)<br>[Minh chứng tuần 2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-02-compute-storage-dr.md)<br>[000057 - Khởi đầu với Amazon S3](https://000057.awsstudygroup.com/vi/) |
| Thứ 5 | - So sánh Storage Gateway và FSx cho hybrid storage và Windows file sharing. | 25/06/2026 | 25/06/2026 | [Module 4 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/module-04-ly-thuyet-s3-storage-backup-dr.md)<br>[Lab 24 Storage Gateway](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-24-storage-gateway-file-share.md)<br>[Lab 25 FSx](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-25-fsx-windows-file-share.md)<br>[Minh chứng tuần 2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-02-compute-storage-dr.md)<br>[000024 - Triển khai AWS Storage Gateway](https://000024.awsstudygroup.com/vi/)<br>[000025 - Triển khai FSx trên Windows](https://000025.awsstudygroup.com/vi/) |
| Thứ 6 | - Lập bản đồ backup plan, retention, restore check và thuật ngữ RTO/RPO cho DR. | 26/06/2026 | 26/06/2026 | [Module 4 lý thuyết](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/module-04-ly-thuyet-s3-storage-backup-dr.md)<br>[Lab 13 AWS Backup](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/First%20Cloud%20AI%20Journey/Module%204/Hands-on%20Labs/lab-13-aws-backup-plan.md)<br>[Minh chứng tuần 2](https://github.com/lamelihuynh/aws-fcaj-learning/blob/main/Worklogs/tuan-02-compute-storage-dr.md)<br>[000013 - AWS Backup cho hệ thống](https://000013.awsstudygroup.com/vi/)<br>[000100 - AWS Elastic Disaster Recovery Workshop](https://000100.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 2:

* Hiểu rõ hơn vòng đời EC2, EBS/AMI backup và vai trò của Auto Scaling phía sau ALB.
* So sánh được S3, Storage Gateway, FSx và AWS Backup theo use case thay vì gom chung là storage.
