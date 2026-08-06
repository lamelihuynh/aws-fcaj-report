---
title: "Blog 2"
date: 2024-02-14
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Data insights from SAP with Amazon SageMaker AutoML and QuickSight

**Source:** [AWS for SAP Blog](https://aws.amazon.com/blogs/awsforsap/data-insights-from-sap-with-amazon-sagemaker-automl-and-quicksight/)

**Author:** Sourav Sadhu

**Published:** February 14, 2024

**Categories:** Amazon QuickSight, Amazon SageMaker, SAP on AWS, Technical How-to, Thought Leadership

**Main services:** Amazon AppFlow, Amazon S3, Amazon SageMaker Autopilot, Amazon SageMaker Studio, Amazon QuickSight, AWS Lambda, Amazon SNS

---

## Introduction

Enterprise systems such as SAP generate large amounts of operational and business data. That data becomes more useful when it can be combined with analytics and machine learning outside the transactional system. The article demonstrates an AWS pattern for extracting SAP data, training a machine learning model, producing predictions and visualizing the results in Amazon QuickSight.

The example uses a publicly available housing dataset loaded into an SAP system. The target use case is predicting future housing prices by location and then exposing the predicted value in a QuickSight dashboard.

The technical goal is to break data silos around SAP data:

- SAP provides the business data source.
- Amazon AppFlow extracts SAP data through OData services.
- Amazon S3 stores raw, training and prediction datasets.
- Amazon SageMaker Autopilot builds, trains, tunes and deploys the ML model.
- Amazon QuickSight reads S3 data and augments it with SageMaker predictions.
- AWS Lambda and Amazon SNS support refresh and notification flows for newly ingested data.

## Overview

The architecture starts with data extraction from SAP to Amazon S3, then uses SageMaker AutoML for model building and QuickSight for analysis. A later step uses newly extracted SAP data, invokes the existing model and updates QuickSight with predicted values.

![Data pipelines architecture](/images/3-BlogsPosted/blog2-01-data-pipeline-architecture.png)

The walkthrough is divided into four steps:

1. Data preparation and feature engineering.
2. Model development, training, tuning and deployment with SageMaker AutoML.
3. Data inference and visualization of predicted data with QuickSight.
4. Data augmentation for newly ingested QuickSight Enterprise edition data with SageMaker.

## Prerequisites

The environment requires:

- an AWS account with IAM permissions for Amazon S3, AWS Lambda, SageMaker, QuickSight, Amazon AppFlow and Amazon SNS,
- a SageMaker domain and domain user profile to launch SageMaker Studio,
- an SAP system exposed as a data source,
- OData services for the SAP data extraction path,
- S3 buckets/prefixes for model training input, prediction input and AutoML output,
- a QuickSight account in the same AWS Region as SageMaker and S3.

## Step 1 - Data preparation and feature engineering

### SAP data preparation and extraction

The article describes two sample data preparation options:

- Use SAP NetWeaver Enterprise Procurement Model (EPM) for procurement scenarios. In this path, a SAP HANA CDS view is exposed with SAP OData services and extracted with Amazon AppFlow.
- Use a public dataset, such as a Kaggle CSV dataset. The CSV data is imported into a SAP HANA table, exposed with an ABAP CDS view, and published as an OData service by using the `@OData.publish:true` annotation.

Amazon AppFlow is used as the extraction service. It extracts data from the SAP application layer using SAP OData services, preserves application-level business logic, supports delta capture and can also write data back to SAP.

The article configures two AppFlow flows:

- `SAP - Housing Modeling Data`: on-demand extraction for model preparation, training and retraining.
- `SAP - California Housing Prediction Data`: scheduled extraction for inference from new SAP data.

### ML data preparation

SageMaker notebooks are used for feature engineering and for splitting the extracted dataset into train, test and prediction files. SageMaker Data Wrangler is mentioned as an alternative visual tool for data preparation.

The preparation logic is:

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

Important outputs from this step:

- training data for SageMaker Autopilot,
- test data for optional validation,
- prediction data without the target label,
- S3 prefixes that QuickSight and SageMaker can use later.

## Step 2 - Model development, training, tuning and deployment with SageMaker AutoML

The training data stored under `sagemaker/automl-dm/input/` is used to create an AutoML job in SageMaker Studio. SageMaker Autopilot automates candidate selection, model training, tuning and deployment.

![Open SageMaker AutoML](/images/3-BlogsPosted/blog2-02-sagemaker-automl-start.png)

The AutoML setup flow:

1. Open SageMaker Studio and choose **AutoML**.
2. Provide a name for the Autopilot experiment.
3. Select the S3 location containing training data.
4. Provide an S3 output location for AutoML artifacts.

![Configure AutoML experiment input](/images/3-BlogsPosted/blog2-03-automl-experiment-input.png)

5. Set the target column. In the housing dataset, the target field is the median house value.

![Set the target column](/images/3-BlogsPosted/blog2-04-target-column.png)

6. Keep the training method and algorithms set to **Auto**, so SageMaker can choose the training approach based on the dataset.

![Use Auto training method](/images/3-BlogsPosted/blog2-05-training-method-auto.png)

7. Provide an endpoint name and keep the remaining endpoint settings as default for the example.

![Configure endpoint name](/images/3-BlogsPosted/blog2-06-endpoint-name.png)

When the AutoML job finishes, SageMaker deploys the best performing model to the named endpoint.

![Best performing model deployed](/images/3-BlogsPosted/blog2-07-best-model-deployed.png)

The endpoint is ready when its status becomes **InService**.

![Endpoint in service](/images/3-BlogsPosted/blog2-08-endpoint-in-service.png)

The article also validates the model from a notebook by invoking the SageMaker endpoint and comparing actual and predicted values:

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

## Step 3 - Data inference and visualization with Amazon QuickSight

QuickSight is configured in the same Region as SageMaker and S3. A new dataset is created from the prediction data stored in S3.

![Create QuickSight dataset](/images/3-BlogsPosted/blog2-09-quicksight-create-dataset.png)

QuickSight uses an S3 manifest file to understand where the source CSV data is located:

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

![Configure S3 manifest dataset](/images/3-BlogsPosted/blog2-10-s3-manifest-dataset.png)

After the dataset is created, QuickSight can use **Augment with SageMaker** to call the model and add predicted output to the analysis.

![Choose Augment with SageMaker](/images/3-BlogsPosted/blog2-11-augment-with-sagemaker.png)

The SageMaker model schema defines the model input fields and the output prediction field:

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

![Provide SageMaker schema](/images/3-BlogsPosted/blog2-12-sagemaker-schema.png)

The next step is reviewing field mappings. Each input field in the schema must match a field in the QuickSight dataset.

![Review input data mapping](/images/3-BlogsPosted/blog2-13-input-data-mapping.png)

The output mapping defines where the predicted `House Median Value` field appears in QuickSight.

![Review output mapping](/images/3-BlogsPosted/blog2-14-output-mapping.png)

After inference, QuickSight can visualize the predicted values. The article shows a table view of `House Median Value` together with the input attributes.

![QuickSight table visual](/images/3-BlogsPosted/blog2-15-quicksight-table-visual.png)

QuickSight AutoGraph is also used to visualize predicted house median value by latitude and longitude.

![QuickSight map visual](/images/3-BlogsPosted/blog2-16-quicksight-map-visual.png)

## Step 4 - Data augmentation for newly ingested SAP data

The last step reuses the SageMaker model from Step 2 for new data coming from SAP. Amazon AppFlow captures new records from SAP and writes them to S3. An S3 event can trigger Lambda, and Lambda starts a QuickSight ingestion refresh. With SageMaker batch transformation / augmentation, predicted data becomes available in the dashboard with low latency.

![Data augmentation flow](/images/3-BlogsPosted/blog2-17-data-augmentation-architecture.png)

The Lambda function pattern is:

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

The SNS configuration is used to notify failures or completion events from Lambda.

![SNS notification configuration](/images/3-BlogsPosted/blog2-18-sns-failure-notification.png)

The S3 configuration connects object creation events to the Lambda function.

![S3 trigger for Lambda](/images/3-BlogsPosted/blog2-19-s3-lambda-trigger.png)

## Conclusion

The article demonstrates one practical pattern for combining SAP data with AWS analytics and AI/ML services. SageMaker AutoML reduces the amount of manual ML work needed to prepare, train and deploy a model. QuickSight makes the predicted data available to business users through dashboards. AppFlow, S3, Lambda and SNS complete the ingestion and refresh workflow around SAP data.

This pattern is useful when SAP remains the system of record but analytics, prediction and visualization need to extend beyond the SAP environment. Other AWS ML services such as Amazon Forecast, Amazon Textract, Amazon Translate and Amazon Comprehend can be integrated with SAP for different use cases.

## Facebook Sharing Evidence

The technical summary was shared to the AWS Study Group VN Facebook community and is currently pending group approval. This screenshot is kept as temporary evidence until the public post URL is available.

![Facebook pending approval evidence for Blog 2](/images/3-BlogsPosted/blog2-fb-pending.png)

## Author

The AWS blog page lists **Sourav Sadhu** as the author. The page does not include a separate author portrait or author bio section.
