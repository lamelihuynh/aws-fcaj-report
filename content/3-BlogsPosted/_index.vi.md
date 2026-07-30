---
title: "Ghi chú blog kỹ thuật"
date: 2026-07-06
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Ghi chú blog kỹ thuật và tài liệu AWS tham khảo

Dưới đây là các bài viết kỹ thuật AWS được dùng làm tài liệu bổ trợ cho báo cáo:

---

### 1. [Blog 1 - Xây dựng hệ thống phát hiện gian lận bằng AWS Entity Resolution và Amazon Neptune Analytics](3.1-Blog1/)
**Tác giả:** Jessica Hung, Isaac Owusu, Ross Gabay | **Ngày đăng:** 05/02/2026 | 👉 **[AWS Database Blog](https://aws.amazon.com/blogs/database/build-fraud-detection-systems-using-aws-entity-resolution-and-amazon-neptune-analytics/)**

Bài viết trình bày một hướng thiết kế hệ thống phát hiện gian lận cho giao dịch Card Not Present. Giải pháp dùng **AWS Entity Resolution** để gom các bản ghi khách hàng có khả năng trùng nhau, sau đó đưa dữ liệu thực thể và giao dịch vào **Amazon Neptune Analytics** để truy vấn graph, phát hiện community và phân tích các cụm đáng ngờ.

---

### 2. [Blog 2 - Khai thác insight từ dữ liệu SAP bằng Amazon SageMaker AutoML và QuickSight](3.2-Blog2/)
**Tác giả:** Sourav Sadhu | **Ngày đăng:** 14/02/2024 | 👉 **[AWS for SAP Blog](https://aws.amazon.com/blogs/awsforsap/data-insights-from-sap-with-amazon-sagemaker-automl-and-quicksight/)**

Bài viết trình bày một pattern analytics và ML cho SAP: dùng **Amazon AppFlow** trích xuất dữ liệu SAP sang **Amazon S3**, dùng **Amazon SageMaker Autopilot** để huấn luyện và deploy model dự đoán giá nhà, sau đó dùng **Amazon QuickSight** để augment dashboard bằng prediction từ SageMaker. Bài cũng minh họa luồng refresh dữ liệu mới bằng **AWS Lambda** và **Amazon SNS**.

---

### 3. [Blog 3 - Tối ưu chi phí truyền dữ liệu khi sử dụng AWS Network Load Balancer](3.3-Blog3/)
**Tác giả:** Luis Felipe Silveira da Silva, Lucas Rolim | **Ngày đăng:** 02/04/2026 | 👉 **[AWS Networking & Content Delivery Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/optimizing-data-transfer-costs-when-using-aws-network-load-balancer/)**

Bài viết giải thích cách vị trí và cấu hình của **AWS Network Load Balancer** ảnh hưởng đến chi phí inter-zone data transfer. Nội dung tập trung vào **Availability Zone DNS affinity**, **cross-zone load balancing**, capacity planning theo từng AZ và **Availability Zone Independence** để giảm traffic cross-AZ không cần thiết nhưng vẫn giữ khả năng chịu lỗi.
