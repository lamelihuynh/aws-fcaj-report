---
title: "Technical Blog Notes"
date: 2026-07-06
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Technical Blog Notes and AWS References

Below are the AWS technical articles used as supporting material for the report:

---

### 1. [Blog 1 - Build fraud detection systems using AWS Entity Resolution and Amazon Neptune Analytics](3.1-blog1/)
**Authors:** Jessica Hung, Isaac Owusu, Ross Gabay | **Date:** February 05, 2026 | 👉 **[AWS Database Blog](https://aws.amazon.com/blogs/database/build-fraud-detection-systems-using-aws-entity-resolution-and-amazon-neptune-analytics/)**

This article explains a fraud detection pattern for Card Not Present transactions. The solution uses **AWS Entity Resolution** to group likely duplicate customer records, then loads the resolved identities and transaction data into **Amazon Neptune Analytics** for graph queries, community detection and suspicious cluster analysis.

---

### 2. [Blog 2 - Data insights from SAP with Amazon SageMaker AutoML and QuickSight](3.2-blog2/)
**Author:** Sourav Sadhu | **Date:** February 14, 2024 | 👉 **[AWS for SAP Blog](https://aws.amazon.com/blogs/awsforsap/data-insights-from-sap-with-amazon-sagemaker-automl-and-quicksight/)**

This article presents an SAP analytics and ML pattern using **Amazon AppFlow** to extract SAP data to **Amazon S3**, **Amazon SageMaker Autopilot** to train and deploy a housing price prediction model, and **Amazon QuickSight** to augment dashboards with SageMaker predictions. It also shows a refresh flow using **AWS Lambda** and **Amazon SNS** for newly ingested SAP data.

---

### 3. [Blog 3 - Optimizing data transfer costs when using AWS Network Load Balancer](3.3-blog3/)
**Authors:** Luis Felipe Silveira da Silva, Lucas Rolim | **Date:** April 02, 2026 | 👉 **[AWS Networking & Content Delivery Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/optimizing-data-transfer-costs-when-using-aws-network-load-balancer/)**

This article explains how **AWS Network Load Balancer** placement and configuration affect inter-zone data transfer costs. It covers **Availability Zone DNS affinity**, **cross-zone load balancing**, target capacity planning per AZ and **Availability Zone Independence** as a pattern for reducing unnecessary cross-AZ traffic while preserving resilience.
