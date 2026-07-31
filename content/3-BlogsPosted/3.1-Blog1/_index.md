---
title: "Blog 1"
date: 2026-02-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Build fraud detection systems using AWS Entity Resolution and Amazon Neptune Analytics

**Source:** [AWS Database Blog](https://aws.amazon.com/blogs/database/build-fraud-detection-systems-using-aws-entity-resolution-and-amazon-neptune-analytics/)

**Authors:** Jessica Hung, Isaac Owusu, Ross Gabay

**Published:** February 05, 2026

**Categories:** Amazon Neptune, Amazon Neptune Analytics, AWS Entity Resolution, Financial Services, Intermediate (200), Technical How-to

**Main services:** AWS Entity Resolution, Amazon Neptune Analytics, Amazon S3, AWS Glue, Amazon SageMaker AI notebooks, Neptune Workbench

---

## Introduction

The article focuses on fraud detection for financial institutions, payment processors and online merchants. A key use case is Card Not Present (CNP) fraud, where a credit or debit card transaction is processed without the physical card being shown to the merchant. This pattern is common in online, phone or mail-order purchases, and it has higher fraud risk because the merchant cannot directly verify the card or the cardholder.

The solution combines two ideas:

- **Entity resolution:** AWS Entity Resolution finds possible links between records by comparing shared attributes such as name, address, email, phone number and date of birth.
- **Graph analytics:** Amazon Neptune Analytics stores customers, accounts, PII attributes, transaction attempts and failures as a graph, then runs graph algorithms to detect hidden relationships.

The important point is that fraud indicators are often indirect. A single customer record or a single failed transaction may not be enough, but a graph can show repeated failures, shared contact details and suspicious clusters across multiple entities.

## Solution overview

AWS Entity Resolution first standardizes and matches customer records from different sources. The result is a more consistent customer view with a persistent identifier. That resolved output, along with transaction data, is then transformed into graph vertices and edges and loaded into Amazon Neptune Analytics. Investigators can use Neptune Workbench hosted on Amazon SageMaker AI notebooks to query and visualize the graph.

![Solution scope](/images/3-BlogsPosted/blog1-entity-resolution-neptune-scope.png)

The workflow has three main stages:

1. Source customer data is sent to AWS Entity Resolution for matching and standardization.
2. Resolved entities are written to an output Amazon S3 bucket.
3. Resolved entities and transaction records are transformed in Neptune Workbench, loaded into Neptune Analytics and queried for graph-based investigation.

## Data model

The graph model represents both identity data and transaction activity. It uses nodes for resolved groups, customers, contact attributes, accounts and CNP transaction events. Edges describe how those objects are connected.

![Graph data model](/images/3-BlogsPosted/blog1-entity-resolution-neptune-data-model.png)

| Type | Label | Purpose | Main properties |
| --- | --- | --- | --- |
| Node | `Group` | Persistent match group produced by AWS Entity Resolution | `MatchId` |
| Node | `Email` | Email identifiers used as matching attributes | `Email` |
| Node | `Customer` | Source customer information from the financial institution | `FirstName`, `MiddleName`, `LastName`, `DateOfBirth` |
| Node | `CreditCardAccount` | Linked credit card account identifiers | `AccountNumber` |
| Node | `Address` | Address values used during entity matching | `Address` |
| Node | `Phone` | Phone numbers provided during onboarding | `PhoneNumber` |
| Node | `CnpCreditCardTxInit` | CNP transaction initiation event | `InitiationDate`, `InitId` |
| Node | `CreditCardTx` | Successful CNP transaction | `TransactionId`, `Amount`, `TransactionDate` |
| Node | `CnpInitFail` | Failed CNP transaction initiation with failure reason | `TransactionId`, `Amount`, `TransactionDate`, `ReasonCode` |
| Edge | `HAS_CNP_TX_INIT` | Links a credit card account or initiation node to the next CNP transaction step | N/A |
| Edge | `HAS_FAIL` | Links a CNP initiation to a failure record | N/A |
| Edge | `HAS_CUSTOMER` | Links a resolved group to customer nodes and carries match confidence | `ConfidenceScore` |
| Edge | `HAS_ACCOUNT` / `HAS_CC_ACCOUNT` | Links customers to their credit card accounts | N/A |
| Edge | `HAS_PHONE` | Links customers to phone identifiers | N/A |
| Edge | `HAS_ADDRESS` | Links customers to address identifiers | N/A |
| Edge | `HAS_EMAIL` | Links customers to email identifiers | N/A |

## Prerequisites

The example can generate AWS cost. Before running it, pricing should be checked for AWS Entity Resolution, Amazon Neptune Analytics, SageMaker AI notebooks, Amazon S3 and AWS Glue. AWS Pricing Calculator is useful for estimating the cost before creating resources.

The environment needs:

- an AWS account,
- AWS CLI installed,
- an Amazon S3 bucket for intermediate files,
- AWS Glue Crawler and Glue Table to provide schema metadata for Entity Resolution,
- IAM roles with permission to read/write S3, run Glue Crawler, create Glue Table, deploy and execute Entity Resolution workflows, and interact with a Neptune Analytics graph,
- a SageMaker execution role that can read/write the S3 bucket,
- a Neptune notebook configured for Neptune Analytics.

## Prepare the AWS Entity Resolution ML workflow

AWS Entity Resolution supports different workflow styles. The article uses an ML matching workflow, where an AWS-provided model handles variations in fields such as name, address, email, phone and date of birth. The output includes a confidence score that indicates how likely records inside the same match group are duplicates. Rule-based workflows are also possible when matching logic must follow explicit business rules.

For test data, the article suggests either using a dataset such as FEBRL or generating mock records with the Faker Python library. The dataset should include at least three of the five matching fields supported by the ML workflow. In the example, the mock fields include address, date of birth, email, first name, last name, full name and middle name.

After the input data is uploaded to Amazon S3 and crawled with AWS Glue, a schema mapping is created so AWS Entity Resolution can understand which source fields should be used for matching.

![Example AWS Entity Resolution schema mapping](/images/3-BlogsPosted/blog1-entity-resolution-schema-mapping.png)

The workflow definition uses a JSON configuration with:

- `workflowName` and description,
- `inputSourceConfig` pointing to the AWS Glue table ARN and schema name,
- `outputSourceConfig` defining the output attributes and output S3 path,
- `resolutionTechniques` set to ML matching,
- `roleArn` for the IAM role used by Entity Resolution.

The important configuration pattern is:

```json
{
  "workflowName": "<workflow-name>",
  "inputSourceConfig": [
    {
      "applyNormalization": true,
      "inputSourceARN": "arn:aws:glue:<region>:<account-id>:table/<database>/<table>",
      "schemaName": "<schema-name>"
    }
  ],
  "outputSourceConfig": [
    {
      "applyNormalization": true,
      "outputS3Path": "s3://<bucket>/entityresolution/output/",
      "output": [
        {"name": "address", "hashed": false},
        {"name": "date_of_birth", "hashed": false},
        {"name": "email", "hashed": false},
        {"name": "full_name", "hashed": false},
        {"name": "lastname", "hashed": false},
        {"name": "middle", "hashed": false},
        {"name": "phone_number", "hashed": false}
      ]
    }
  ],
  "resolutionTechniques": {
    "resolutionType": "ML_MATCHING"
  },
  "roleArn": "arn:aws:iam::<account-id>:role/<entity-resolution-role>"
}
```

The key AWS CLI actions are:

```bash
aws entityresolution create-matching-workflow --region <region> --cli-input-json file://ml-workflow.json
aws entityresolution start-matching-job --region <region> --workflow-name <workflow-name>
```

## Transform Entity Resolution output

When the ML matching job finishes, the resolved output has to be transformed into a Neptune-compatible bulk load format. The article uses a Neptune notebook and Python data processing to create OpenCypher bulk load files.

The transformation starts by reading the Entity Resolution output from S3 into a dataframe. From that dataframe, the notebook writes separate CSV files for graph nodes and graph edges.

The node transformation creates these files:

- `groups.csv`: `Group` nodes from distinct `MatchID` values.
- `login.csv`: `Email` nodes from unique email values.
- `customer.csv`: `Customer` nodes from customer IDs, name fields and date of birth.
- `phone.csv`: `Phone` nodes from unique phone values.
- `address.csv`: `Address` nodes from unique address values.

A compact pseudocode version of the node creation flow is:

```python
df = read_entity_resolution_output_from_s3()

write_node_file(
    label="Group",
    id_prefix="Group-",
    source_column="MatchID",
    output="s3://<bucket>/neptune/nodes/groups.csv"
)

write_node_file(
    label="Customer",
    id_prefix="Customer-",
    source_column="customer_id",
    properties=["firstname", "lastname", "middle", "date_of_birth"],
    output="s3://<bucket>/neptune/nodes/customer.csv"
)

write_identifier_nodes(["email", "phone", "address"])
```

The edge transformation creates these relationship files:

- `hasCustomer.csv`: `Group` to `Customer`, including confidence score from the ML workflow.
- `hasPhone.csv`: `Customer` to `Phone`.
- `hasEmail.csv`: `Customer` to `Email`.
- `hasAddress.csv`: `Customer` to `Address`.

The relationship creation pattern is:

```python
write_edge_file(
    label="HAS_CUSTOMER",
    from_id="Group-" + MatchID,
    to_id="Customer-" + customer_id,
    properties={"confidence": ConfidenceLevel},
    output="s3://<bucket>/neptune/edges/hasCustomer.csv"
)

write_edge_file(label="HAS_PHONE", from_id="Customer-*", to_id="Phone-*")
write_edge_file(label="HAS_EMAIL", from_id="Customer-*", to_id="Email-*")
write_edge_file(label="HAS_ADDRESS", from_id="Customer-*", to_id="Address-*")
```

The transformed CSV files are written back to S3 under separate folders such as `neptune/nodes/` and `neptune/edges/`. This separation keeps identity nodes, contact nodes and relationship files ready for the Neptune Analytics loader.

## Load additional transaction datasets

The customer identity data is supplemented with generated transaction data so the graph can simulate a CNP transaction workflow. The article uses Python libraries such as `random`, `uuid`, `csv`, `boto3`, `datawangler` and `Faker`.

The generated dataset adds:

- `CreditCardAccount` nodes attached to a subset of customers,
- `CnpCreditCardTxInit` nodes for initiated CNP transactions,
- `CreditCardTx` nodes for successful CNP transactions,
- `CnpInitFail` nodes for failed CNP transaction attempts,
- `HAS_CC_ACCOUNT`, `HAS_CNP_TX_INIT`, `HAS_TX` and `HAS_FAIL` relationships.

The transaction generator randomly creates one or more CNP initiation events per card account. Some initiations become successful transactions, while others become failures with reason codes. In the article, reason code `3` is later treated as the highest-risk failure reason for analysis.

The transaction generation logic can be written as:

```python
for customer in resolved_customers:
    if customer_is_selected_for_credit_card_account:
        create CreditCardAccount
        create HAS_CC_ACCOUNT from Customer to CreditCardAccount

        for each generated CNP initiation:
            create CnpCreditCardTxInit
            create HAS_CNP_TX_INIT from CreditCardAccount to CnpCreditCardTxInit

            if the generated transaction fails:
                create CnpInitFail with reason_code in [1, 2, 3]
                create HAS_FAIL from CnpCreditCardTxInit to CnpInitFail
            else:
                create CreditCardTx
                create HAS_TX from CnpCreditCardTxInit to CreditCardTx
```

The article writes the generated graph files using names such as:

- `blog_cc_nodes.csv`
- `blog_member_to_cc_acct_rels.csv`
- `blog_cnp_init_nodes.csv`
- `blog_cc_to_cnp_init_rels.csv`
- `blog_cnp_tx_nodes.csv`
- `blog_cnp_to_cnp_tx_rels.csv`
- `blog_cnp_fail_nodes.csv`
- `blog_cnp_to_fail_rels.csv`

After the files are created, they are uploaded to S3 prefixes for Neptune bulk loading. Node files are placed under `neptune/nodes/`; relationship files are placed under `neptune/edges/`.

After the CSV files are generated and uploaded to S3, Neptune Analytics can load them from S3 using batch load:

```sql
CALL neptune.load({
  source: "s3://<bucket-or-prefix>",
  region: "<region>",
  format: "csv"
})
```

## Analyze output with Neptune Analytics

Neptune Analytics is used through Neptune Workbench to run graph algorithms and OpenCypher queries. The article focuses on Louvain community detection and weakly connected components (WCC).

Louvain community detection helps find dense communities by optimizing modularity. In this context, it can identify clusters of accounts, transaction attempts and failed CNP events that are more connected than expected.

Weakly connected components group nodes that are connected through the selected edge labels, even if direction is ignored. This helps find resolved customers that share at least one matching attribute such as phone, email, address, customer group or account relationship.

The analysis uses these main steps:

1. Run Louvain on the CNP failure subgraph and persist the community ID as `CNPFailCommunity`.
2. Run WCC across identity and account relationships and persist the component ID as `WCC`.
3. Find the largest CNP failure community.
4. Retrieve customers and PII attributes associated with that community and the same WCC.
5. Filter high-risk CNP failures, especially records with `reason_code = "3"`.
6. Count how many distinct AWS Entity Resolution match groups are associated with the risky failures.
7. Compare shared and non-shared PII to evaluate whether two match groups may still be related.
8. Calculate the ratio of known bad actors to total customers in each match group for investigation prioritization.

The first query pattern narrows the graph to customers, credit card accounts and CNP failures, then writes a Louvain community property called `CNPFailCommunity`:

```sql
MATCH (:Customer)-[:HAS_CC_ACCOUNT]->(:CreditCardAccount)-[*1..]->(:CnpInitFail)
CALL neptune.algo.louvain.mutate({
  edgeLabels: ["HAS_CC_ACCOUNT", "HAS_TX", "HAS_CNP_TX_INIT", "HAS_FAIL"],
  writeProperty: "CNPFailCommunity"
})
YIELD success
RETURN success
```

The second algorithm call creates weakly connected components across identity and account relationships. This creates an additional property named `WCC`:

```sql
CALL neptune.algo.wcc.mutate({
  edgeLabels: ["HAS_ADDRESS", "HAS_EMAIL", "HAS_FAIL", "HAS_CUSTOMER", "HAS_PHONE", "HAS_CC_ACCOUNT"],
  writeProperty: "WCC"
})
YIELD success
RETURN success
```

After those properties exist on the graph, the next query finds the largest CNP failure community:

```sql
MATCH (failure:CnpInitFail)
RETURN failure.CNPFailCommunity AS communityId, count(*) AS failedTransactionCount
ORDER BY failedTransactionCount DESC
LIMIT 1
```

The result is then used to inspect customers and PII attributes that belong to the same WCC cluster:

```sql
MATCH (failure:CnpInitFail)
WITH failure.CNPFailCommunity AS communityId
MATCH (customer:Customer {CNPFailCommunity: communityId})
MATCH (:Customer {WCC: customer.WCC})-->(pii)
RETURN customer, pii
```

The investigation then looks for the largest failure community and retrieves related identity attributes.

![Largest CNP failure cluster result](/images/3-BlogsPosted/blog1-neptune-largest-failure-cluster.png)

For deeper analysis, the workflow drills into `CnpInitFail` nodes with the highest-risk reason code. It checks whether multiple resolved groups are connected to the same risky failure community:

```sql
MATCH (failure:CnpInitFail {reason_code: "3"})
WITH failure.CNPFailCommunity AS communityId
MATCH (group:Group)-->(customer:Customer {CNPFailCommunity: communityId})
RETURN group.WCC AS identityCluster, count(DISTINCT group) AS resolvedGroupCount
ORDER BY resolvedGroupCount DESC
```

The next inspection compares shared PII across different resolved groups. If two groups have different email, phone or address values but share at least one identifier, they may represent related activity that deserves further investigation. The original article notes that the visualization can show two separate `Group` nodes and four parties, where the groups differ on some PII values but still share a number and at least one address.

![Neptune graph visualization](/images/3-BlogsPosted/blog1-neptune-graph-visualization.png)

The article also calculates the percentage of bad actors per match group:

```sql
MATCH (group:Group)-->(customer:Customer)
MATCH (group)-->(badActor:Customer)-[:HAS_CC_ACCOUNT]->()
      -[:HAS_CNP_TX_INIT]->()-[:HAS_FAIL]->(:CnpInitFail {reason_code: "3"})
RETURN group.MatchId AS matchId,
       count(DISTINCT badActor) AS badActorCount,
       count(DISTINCT customer) AS totalCustomerCount,
       count(DISTINCT badActor) / toFloat(count(DISTINCT customer)) AS badActorRatio
ORDER BY badActorRatio DESC
```

Groups with higher bad-actor ratios can be prioritized, but the total number of customers in the group must also be considered. For example, a 50% bad-actor ratio in a two-customer group needs a different interpretation than a lower ratio in a much larger group.

![Bad actor ratio output](/images/3-BlogsPosted/blog1-risk-ratio-output.png)

Graph Explorer is also mentioned as an additional visualization option for browsing graph data without writing queries.

## Clean up

Cleanup is required to avoid unnecessary cost. The resources to remove include:

- AWS Entity Resolution matching workflows,
- AWS Glue Tables,
- AWS Glue Crawlers,
- Neptune notebooks,
- Neptune Analytics graphs,
- S3 buckets or objects created for the exercise.

## Conclusion

The article shows how AWS Entity Resolution and Amazon Neptune Analytics can work together for CNP fraud detection. Entity Resolution standardizes and matches customer records, while Neptune Analytics turns the resolved identities and transactions into a graph that can be searched, visualized and analyzed with graph algorithms.

The main advantage is investigation depth. Instead of only checking individual records, analysts can inspect relationships between customers, accounts, contact details and transaction failures. Neptune Workbench then provides a practical environment for running queries, visualizing suspicious clusters and prioritizing fraud investigation.

## About the authors

<table>
  <tr>
    <td width="130" valign="top">
      <img src="https://lamelihuynh.github.io/aws-fcaj-report/images/3-BlogsPosted/blog1-author-jessica-hung.jpeg" width="120" alt="Jessica Hung" />
    </td>
    <td valign="top">
      <strong>Jessica Hung</strong><br/>
      Senior Data Architect at AWS Professional Services. Her work focuses on scalable data applications, including graph database and entity resolution workloads for customers such as financial services organizations.
    </td>
  </tr>
</table>

<table>
  <tr>
    <td width="130" valign="top">
      <img src="https://lamelihuynh.github.io/aws-fcaj-report/images/3-BlogsPosted/blog1-author-ross-gabay.png" width="120" alt="Ross Gabay" />
    </td>
    <td valign="top">
      <strong>Ross Gabay</strong><br/>
      Principal Data Architect in AWS Professional Services. He works with AWS customers on enterprise-grade solutions using Amazon Neptune and other AWS services.
    </td>
  </tr>
</table>

<table>
  <tr>
    <td width="130" valign="top">
      <img src="https://lamelihuynh.github.io/aws-fcaj-report/images/3-BlogsPosted/blog1-author-isaac-owusu.jpeg" width="120" alt="Isaac Kwasi Owusu" />
    </td>
    <td valign="top">
      <strong>Isaac Kwasi Owusu</strong><br/>
      Senior Data Architect at AWS with experience in large-scale enterprise data solutions and NoSQL database design, specializing in graph databases.
    </td>
  </tr>
</table>
