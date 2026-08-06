---
title: "Blog 2"
date: 2024-02-14
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Khai thác insight từ dữ liệu SAP bằng Amazon SageMaker AutoML và QuickSight

**Nguồn:** [AWS for SAP Blog](https://aws.amazon.com/blogs/awsforsap/data-insights-from-sap-with-amazon-sagemaker-automl-and-quicksight/)

**Tác giả:** Sourav Sadhu

**Ngày đăng:** 14/02/2024

**Nhóm nội dung:** Amazon QuickSight, Amazon SageMaker, SAP on AWS, Technical How-to, Thought Leadership

**Dịch vụ chính:** Amazon AppFlow, Amazon S3, Amazon SageMaker Autopilot, Amazon SageMaker Studio, Amazon QuickSight, AWS Lambda, Amazon SNS

---

## Giới thiệu

Các hệ thống doanh nghiệp như SAP tạo ra rất nhiều dữ liệu vận hành và dữ liệu nghiệp vụ. Dữ liệu đó có giá trị hơn khi được kết hợp với phân tích và machine learning bên ngoài hệ thống giao dịch. Bài viết trình bày một pattern trên AWS để trích xuất dữ liệu SAP, huấn luyện mô hình machine learning, tạo dự đoán và trực quan hóa kết quả bằng Amazon QuickSight.

Ví dụ sử dụng một bộ dữ liệu nhà ở công khai được nạp vào hệ thống SAP. Use case là dự đoán giá nhà trong tương lai theo vị trí, sau đó hiển thị giá trị dự đoán trong dashboard QuickSight.

Mục tiêu kỹ thuật là giảm data silo quanh dữ liệu SAP:

- SAP đóng vai trò nguồn dữ liệu nghiệp vụ.
- Amazon AppFlow trích xuất dữ liệu SAP thông qua OData service.
- Amazon S3 lưu dữ liệu raw, dữ liệu training và dữ liệu prediction.
- Amazon SageMaker Autopilot xây dựng, huấn luyện, tune và deploy mô hình ML.
- Amazon QuickSight đọc dữ liệu S3 và augment dữ liệu bằng prediction từ SageMaker.
- AWS Lambda và Amazon SNS hỗ trợ luồng refresh và notification cho dữ liệu mới được ingest.

## Tổng quan

Kiến trúc bắt đầu bằng việc trích xuất dữ liệu từ SAP sang Amazon S3, sau đó dùng SageMaker AutoML để xây dựng mô hình và QuickSight để phân tích. Ở bước sau, dữ liệu SAP mới được trích xuất sẽ dùng lại mô hình hiện có để bổ sung giá trị dự đoán vào QuickSight.

![Kiến trúc data pipeline](/images/3-BlogsPosted/blog2-01-data-pipeline-architecture.png)

Walkthrough gồm bốn bước:

1. Chuẩn bị dữ liệu và feature engineering.
2. Phát triển, huấn luyện, tune và deploy model bằng SageMaker AutoML.
3. Inference và trực quan hóa dữ liệu dự đoán bằng QuickSight.
4. Augment dữ liệu QuickSight Enterprise edition mới ingest bằng SageMaker.

## Điều kiện cần có

Môi trường cần:

- AWS account có IAM permission cho Amazon S3, AWS Lambda, SageMaker, QuickSight, Amazon AppFlow và Amazon SNS,
- SageMaker domain và domain user profile để mở SageMaker Studio,
- SAP system làm data source,
- OData service cho luồng trích xuất dữ liệu SAP,
- S3 bucket/prefix cho training input, prediction input và AutoML output,
- QuickSight account cùng AWS Region với SageMaker và S3.

## Bước 1 - Chuẩn bị dữ liệu và feature engineering

### Chuẩn bị và trích xuất dữ liệu SAP

Bài viết đưa ra hai cách chuẩn bị dữ liệu mẫu:

- Dùng SAP NetWeaver Enterprise Procurement Model (EPM) cho kịch bản procurement. Với hướng này, SAP HANA CDS view được expose qua SAP OData service rồi trích xuất bằng Amazon AppFlow.
- Dùng public dataset, ví dụ dataset CSV từ Kaggle. CSV được import vào SAP HANA table, expose bằng ABAP CDS view, rồi publish thành OData service bằng annotation `@OData.publish:true`.

Amazon AppFlow được dùng làm dịch vụ extraction. AppFlow trích xuất dữ liệu từ application layer của SAP qua SAP OData service, giữ được business logic ở tầng ứng dụng, hỗ trợ delta capture và cũng có khả năng ghi dữ liệu ngược về SAP.

Bài viết cấu hình hai AppFlow flow:

- `SAP - Housing Modeling Data`: chạy on-demand cho vòng đời chuẩn bị model, training và retraining.
- `SAP - California Housing Prediction Data`: chạy theo schedule để inference dữ liệu SAP mới.

### Chuẩn bị dữ liệu ML

SageMaker notebook được dùng để feature engineering và chia dữ liệu thành train, test, prediction. SageMaker Data Wrangler được nhắc tới như một lựa chọn visual cho data preparation.

Logic chuẩn bị dữ liệu:

```python
import pandas as pd
import sagemaker
from sklearn.model_selection import train_test_split

model_data = pd.read_csv(
    "s3://<bucket>/california_housing/onpremises-modelling-data/california_housing_data.csv",
    index_col=0
)

train_data, test_data = train_test_split(model_data, test_size=0.2, random_state=42)
prediction_data = test_data.drop(["median_house_value"], axis=1)

train_data.to_csv("automl-train-data.csv", header=True, sep=",")
test_data.to_csv("automl-test.csv", header=True, sep=",")
prediction_data.to_csv("prediction-data.csv", header=True, sep=",")

session = sagemaker.Session()
session.upload_data("automl-train-data.csv", bucket="<bucket>", key_prefix="sagemaker/automl-dm/input")
session.upload_data("prediction-data.csv", bucket="<bucket>", key_prefix="california_housing/onpremises-prediction-data")
```

Output quan trọng của bước này:

- training data cho SageMaker Autopilot,
- test data để validation tùy chọn,
- prediction data đã bỏ target label,
- S3 prefix để QuickSight và SageMaker sử dụng ở các bước sau.

## Bước 2 - Phát triển, huấn luyện, tune và deploy model bằng SageMaker AutoML

Dữ liệu training đặt dưới `sagemaker/automl-dm/input/` được dùng để tạo AutoML job trong SageMaker Studio. SageMaker Autopilot tự động hóa việc chọn candidate, huấn luyện model, tuning và deployment.

![Mở SageMaker AutoML](/images/3-BlogsPosted/blog2-02-sagemaker-automl-start.png)

Luồng cấu hình AutoML:

1. Mở SageMaker Studio và chọn **AutoML**.
2. Đặt tên cho Autopilot experiment.
3. Chọn S3 location chứa training data.
4. Cung cấp S3 output location cho artifact của AutoML.

![Cấu hình input cho AutoML experiment](/images/3-BlogsPosted/blog2-03-automl-experiment-input.png)

5. Chọn target column. Với dataset nhà ở, target field là median house value.

![Chọn target column](/images/3-BlogsPosted/blog2-04-target-column.png)

6. Giữ training method và algorithms ở chế độ **Auto**, để SageMaker tự chọn hướng training dựa trên dataset.

![Chọn training method Auto](/images/3-BlogsPosted/blog2-05-training-method-auto.png)

7. Đặt endpoint name và giữ các endpoint setting còn lại theo default trong ví dụ.

![Cấu hình endpoint name](/images/3-BlogsPosted/blog2-06-endpoint-name.png)

Khi AutoML job hoàn tất, SageMaker deploy best performing model tới endpoint đã đặt tên.

![Best performing model đã deploy](/images/3-BlogsPosted/blog2-07-best-model-deployed.png)

Endpoint sẵn sàng khi trạng thái chuyển sang **InService**.

![Endpoint ở trạng thái InService](/images/3-BlogsPosted/blog2-08-endpoint-in-service.png)

Bài viết cũng kiểm tra model từ notebook bằng cách invoke SageMaker endpoint và so sánh actual value với predicted value:

```python
import boto3

endpoint_name = "California-Housing-Pricing"
runtime = boto3.Session().client("runtime.sagemaker")

for row in read_test_rows("automl-test.csv"):
    label, features = split_label_from_features(row)
    response = runtime.invoke_endpoint(
        EndpointName=endpoint_name,
        ContentType="text/csv",
        Accept="text/csv",
        Body=features
    )
    prediction = response["Body"].read().decode("utf-8")
    print("Actual:", label, "Predicted:", prediction)
```

## Bước 3 - Inference và trực quan hóa bằng Amazon QuickSight

QuickSight được cấu hình cùng Region với SageMaker và S3. Dataset mới được tạo từ prediction data lưu trong S3.

![Tạo QuickSight dataset](/images/3-BlogsPosted/blog2-09-quicksight-create-dataset.png)

QuickSight dùng S3 manifest file để biết source CSV nằm ở đâu:

```json
{
  "fileLocations": [
    {
      "URIs": [
        "s3://<bucket>/california_housing/onpremises-prediction-data/prediction-data.csv"
      ]
    },
    {
      "URIPrefixes": [
        "s3://<bucket>/california_housing/onpremises-prediction-data/"
      ]
    }
  ],
  "globalUploadSettings": {
    "format": "CSV",
    "delimiter": ",",
    "textqualifier": "'",
    "containsHeader": "true"
  }
}
```

![Cấu hình S3 manifest dataset](/images/3-BlogsPosted/blog2-10-s3-manifest-dataset.png)

Sau khi dataset được tạo, QuickSight có thể dùng **Augment with SageMaker** để gọi model và thêm output dự đoán vào analysis.

![Chọn Augment with SageMaker](/images/3-BlogsPosted/blog2-11-augment-with-sagemaker.png)

SageMaker model schema định nghĩa các input field của model và output prediction field:

```json
{
  "inputContentType": "CSV",
  "outputContentType": "CSV",
  "input": [
    {"name": "longitude", "type": "DECIMAL"},
    {"name": "latitude", "type": "DECIMAL"},
    {"name": "housing_median_age", "type": "INTEGER"},
    {"name": "total_rooms", "type": "INTEGER"},
    {"name": "total_bedrooms", "type": "INTEGER"},
    {"name": "population", "type": "INTEGER"},
    {"name": "households", "type": "INTEGER"},
    {"name": "median_income", "type": "DECIMAL"},
    {"name": "ocean_proximity", "type": "STRING"}
  ],
  "output": [
    {"name": "House Median Value", "type": "DECIMAL"}
  ],
  "version": "v1",
  "instanceCount": 1,
  "defaultInstanceType": "ml.c5.2xlarge"
}
```

![Khai báo SageMaker schema](/images/3-BlogsPosted/blog2-12-sagemaker-schema.png)

Bước tiếp theo là review field mapping. Mỗi input field trong schema phải khớp với field trong QuickSight dataset.

![Review input data mapping](/images/3-BlogsPosted/blog2-13-input-data-mapping.png)

Output mapping xác định predicted field `House Median Value` sẽ xuất hiện trong QuickSight như thế nào.

![Review output mapping](/images/3-BlogsPosted/blog2-14-output-mapping.png)

Sau khi inference, QuickSight có thể trực quan hóa predicted value. Bài viết minh họa table view của `House Median Value` cùng các input attributes.

![QuickSight table visual](/images/3-BlogsPosted/blog2-15-quicksight-table-visual.png)

QuickSight AutoGraph cũng được dùng để trực quan hóa predicted house median value theo latitude và longitude.

![QuickSight map visual](/images/3-BlogsPosted/blog2-16-quicksight-map-visual.png)

## Bước 4 - Augment dữ liệu SAP mới ingest

Bước cuối dùng lại SageMaker model ở Bước 2 cho dữ liệu mới đến từ SAP. Amazon AppFlow capture record mới từ SAP và ghi vào S3. S3 event có thể trigger Lambda, và Lambda bắt đầu QuickSight ingestion refresh. Với SageMaker batch transformation / augmentation, predicted data được đưa vào dashboard với độ trễ thấp.

![Luồng data augmentation](/images/3-BlogsPosted/blog2-17-data-augmentation-architecture.png)

Pattern Lambda:

```python
import boto3
import uuid

def lambda_handler(event, context):
    ingestion_id = str(uuid.uuid4())
    quicksight = boto3.client("quicksight")

    quicksight.create_ingestion(
        AwsAccountId="<account-id>",
        DataSetId="<quicksight-dataset-id>",
        IngestionId=ingestion_id
    )

    status = wait_until_ingestion_completes(quicksight, ingestion_id)

    sns = boto3.client("sns")
    sns.publish(
        TopicArn="arn:aws:sns:<region>:<account-id>:<topic-name>",
        Message=f"QuickSight ingestion status: {status}",
        Subject="Data Ingestion Status"
    )
```

SNS configuration được dùng để thông báo failure hoặc completion event từ Lambda.

![Cấu hình SNS notification](/images/3-BlogsPosted/blog2-18-sns-failure-notification.png)

S3 configuration kết nối object creation event tới Lambda function.

![S3 trigger cho Lambda](/images/3-BlogsPosted/blog2-19-s3-lambda-trigger.png)

## Kết luận

Bài viết minh họa một pattern thực tế để kết hợp dữ liệu SAP với analytics và AI/ML trên AWS. SageMaker AutoML giảm lượng công việc ML thủ công khi chuẩn bị, huấn luyện và deploy model. QuickSight đưa predicted data tới business user thông qua dashboard. AppFlow, S3, Lambda và SNS hoàn thiện luồng ingestion và refresh quanh dữ liệu SAP.

Pattern này phù hợp khi SAP vẫn là system of record nhưng analytics, prediction và visualization cần mở rộng ra ngoài SAP environment. Các dịch vụ ML khác như Amazon Forecast, Amazon Textract, Amazon Translate và Amazon Comprehend cũng có thể tích hợp với SAP cho những use case khác.

## Minh chứng chia sẻ Facebook

Bài tóm tắt kỹ thuật đã được chia sẻ lên cộng đồng AWS Study Group VN và hiện đang trong trạng thái chờ quản trị viên phê duyệt. Ảnh dưới đây được dùng làm minh chứng tạm thời cho đến khi có URL bài đăng công khai.

![Minh chứng Facebook đang chờ duyệt cho Blog 2](/images/3-BlogsPosted/blog2-fb-pending.png)

## Tác giả

Trang AWS Blog ghi **Sourav Sadhu** là tác giả. Bài này không có phần ảnh chân dung hoặc author bio riêng.
