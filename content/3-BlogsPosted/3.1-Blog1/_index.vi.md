---
title: "Blog 1"
date: 2026-02-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Xây dựng hệ thống phát hiện gian lận bằng AWS Entity Resolution và Amazon Neptune Analytics

**Nguồn:** [AWS Database Blog](https://aws.amazon.com/blogs/database/build-fraud-detection-systems-using-aws-entity-resolution-and-amazon-neptune-analytics/)

**Tác giả:** Jessica Hung, Isaac Owusu, Ross Gabay

**Ngày đăng:** 05/02/2026

**Nhóm nội dung:** Amazon Neptune, Amazon Neptune Analytics, AWS Entity Resolution, Financial Services, Intermediate (200), Technical How-to

**Dịch vụ chính:** AWS Entity Resolution, Amazon Neptune Analytics, Amazon S3, AWS Glue, Amazon SageMaker AI notebooks, Neptune Workbench

---

## Giới thiệu

Bài viết tập trung vào phát hiện gian lận cho ngân hàng, đơn vị xử lý thanh toán và đơn vị bán hàng trực tuyến. Use case chính là Card Not Present (CNP) fraud, tức giao dịch thẻ tín dụng hoặc thẻ ghi nợ được xử lý khi thẻ vật lý không được xuất trình trực tiếp cho merchant. Mô hình này thường gặp trong mua hàng online, đặt hàng qua điện thoại hoặc mail-order, nên rủi ro gian lận cao hơn vì merchant không xác minh trực tiếp được thẻ và chủ thẻ.

Giải pháp kết hợp hai hướng xử lý:

- **Entity resolution:** AWS Entity Resolution tìm quan hệ giữa các bản ghi dựa trên thuộc tính dùng chung như tên, địa chỉ, email, số điện thoại và ngày sinh.
- **Graph analytics:** Amazon Neptune Analytics lưu khách hàng, tài khoản, thuộc tính PII, lần khởi tạo giao dịch và giao dịch thất bại dưới dạng graph, sau đó dùng thuật toán graph để phát hiện quan hệ ẩn.

Điểm quan trọng là tín hiệu gian lận thường không nằm ở một bản ghi đơn lẻ. Một khách hàng hoặc một giao dịch thất bại có thể chưa đủ bất thường, nhưng graph có thể cho thấy nhiều lần thất bại, nhiều định danh dùng chung và các cụm thực thể có liên hệ với nhau.

## Tổng quan giải pháp

AWS Entity Resolution chuẩn hóa và đối sánh dữ liệu khách hàng từ nhiều nguồn. Kết quả là một góc nhìn khách hàng nhất quán hơn, có persistent identifier. Phần output sau khi resolve, cùng với dữ liệu giao dịch, được chuyển thành vertex và edge rồi nạp vào Amazon Neptune Analytics. Investigator có thể dùng Neptune Workbench chạy trên Amazon SageMaker AI notebooks để truy vấn và trực quan hóa graph.

![Phạm vi giải pháp](/images/3-BlogsPosted/blog1-entity-resolution-neptune-scope.png)

Luồng xử lý chính gồm:

1. Dữ liệu khách hàng nguồn được đưa vào AWS Entity Resolution để matching và chuẩn hóa.
2. Entity đã resolve được ghi vào output bucket trên Amazon S3.
3. Entity và transaction được transform trong Neptune Workbench, nạp vào Neptune Analytics và truy vấn để phân tích theo graph.

## Mô hình dữ liệu

Graph model biểu diễn cả dữ liệu định danh và hoạt động giao dịch. Node được dùng cho resolved group, customer, thuộc tính liên hệ, account và event của CNP transaction. Edge mô tả cách các đối tượng này liên kết với nhau.

![Mô hình dữ liệu graph](/images/3-BlogsPosted/blog1-entity-resolution-neptune-data-model.png)

| Loại | Label | Vai trò | Thuộc tính chính |
| --- | --- | --- | --- |
| Node | `Group` | Match group cố định do AWS Entity Resolution tạo ra | `MatchId` |
| Node | `Email` | Email identifier dùng làm thuộc tính matching | `Email` |
| Node | `Customer` | Thông tin khách hàng từ hệ thống nguồn của tổ chức tài chính | `FirstName`, `MiddleName`, `LastName`, `DateOfBirth` |
| Node | `CreditCardAccount` | Định danh tài khoản thẻ tín dụng được liên kết với customer | `AccountNumber` |
| Node | `Address` | Giá trị địa chỉ dùng trong quá trình entity matching | `Address` |
| Node | `Phone` | Số điện thoại được cung cấp khi onboarding | `PhoneNumber` |
| Node | `CnpCreditCardTxInit` | Sự kiện khởi tạo giao dịch CNP | `InitiationDate`, `InitId` |
| Node | `CreditCardTx` | Giao dịch CNP thành công | `TransactionId`, `Amount`, `TransactionDate` |
| Node | `CnpInitFail` | Giao dịch CNP bị từ chối kèm lý do thất bại | `TransactionId`, `Amount`, `TransactionDate`, `ReasonCode` |
| Edge | `HAS_CNP_TX_INIT` | Liên kết tài khoản thẻ hoặc initiation node tới bước tiếp theo của CNP transaction | N/A |
| Edge | `HAS_FAIL` | Liên kết CNP initiation tới failure record | N/A |
| Edge | `HAS_CUSTOMER` | Liên kết resolved group với customer node và mang thông tin confidence | `ConfidenceScore` |
| Edge | `HAS_ACCOUNT` / `HAS_CC_ACCOUNT` | Liên kết customer với tài khoản thẻ | N/A |
| Edge | `HAS_PHONE` | Liên kết customer với phone identifier | N/A |
| Edge | `HAS_ADDRESS` | Liên kết customer với address identifier | N/A |
| Edge | `HAS_EMAIL` | Liên kết customer với email identifier | N/A |

## Điều kiện cần có

Ví dụ trong bài có thể phát sinh chi phí AWS. Trước khi chạy, cần kiểm tra pricing của AWS Entity Resolution, Amazon Neptune Analytics, SageMaker AI notebooks, Amazon S3 và AWS Glue. AWS Pricing Calculator có thể dùng để ước lượng chi phí trước khi tạo tài nguyên.

Môi trường cần có:

- AWS account,
- AWS CLI,
- Amazon S3 bucket để lưu file trung gian,
- AWS Glue Crawler và Glue Table để cung cấp schema metadata cho Entity Resolution,
- IAM role có quyền đọc/ghi S3, chạy Glue Crawler, tạo Glue Table, deploy và execute Entity Resolution workflow, thao tác với Neptune Analytics graph,
- SageMaker execution role có quyền đọc/ghi S3 bucket,
- Neptune notebook đã cấu hình cho Neptune Analytics.

## Chuẩn bị AWS Entity Resolution ML workflow

AWS Entity Resolution hỗ trợ nhiều kiểu workflow. Bài viết dùng ML matching workflow, trong đó model do AWS cung cấp xử lý sai khác giữa các trường như name, address, email, phone và date of birth. Output có confidence score để thể hiện khả năng các record trong cùng match group là bản ghi trùng. Rule-based workflow cũng có thể dùng khi logic matching cần bám theo rule nghiệp vụ rõ ràng.

Dữ liệu thử nghiệm có thể lấy từ FEBRL hoặc tạo mock records bằng thư viện Faker của Python. Dataset nên có ít nhất ba trong năm trường matching mà ML workflow hỗ trợ. Trong ví dụ, các field giả lập gồm address, date of birth, email, first name, last name, full name và middle name.

Sau khi input data được upload lên Amazon S3 và crawl bằng AWS Glue, cần tạo schema mapping để AWS Entity Resolution hiểu field nguồn nào được dùng trong matching workflow.

![Ví dụ schema mapping của AWS Entity Resolution](/images/3-BlogsPosted/blog1-entity-resolution-schema-mapping.png)

Workflow definition dùng JSON config gồm:

- `workflowName` và description,
- `inputSourceConfig` trỏ tới AWS Glue table ARN và schema name,
- `outputSourceConfig` khai báo output attributes và output S3 path,
- `resolutionTechniques` đặt ở chế độ ML matching,
- `roleArn` của IAM role dùng bởi Entity Resolution.

Mẫu cấu hình quan trọng:

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

Hai thao tác AWS CLI chính:

```bash
aws entityresolution create-matching-workflow --region <region> --cli-input-json file://ml-workflow.json
aws entityresolution start-matching-job --region <region> --workflow-name <workflow-name>
```

## Transform output của Entity Resolution

Khi ML matching job hoàn tất, output đã resolve cần được chuyển sang định dạng bulk load mà Neptune hỗ trợ. Bài viết dùng Neptune notebook và Python để tạo các file OpenCypher bulk load.

Quá trình transform bắt đầu bằng việc đọc output của Entity Resolution từ S3 vào dataframe. Từ dataframe này, notebook ghi ra các file CSV riêng cho graph node và graph edge.

Phần transform node tạo các file:

- `groups.csv`: `Group` node từ các giá trị `MatchID` khác nhau.
- `login.csv`: `Email` node từ email duy nhất.
- `customer.csv`: `Customer` node từ customer ID, các trường tên và ngày sinh.
- `phone.csv`: `Phone` node từ số điện thoại duy nhất.
- `address.csv`: `Address` node từ địa chỉ duy nhất.

Có thể tóm tắt logic tạo node như sau:

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

Phần transform edge tạo các file relationship:

- `hasCustomer.csv`: `Group` tới `Customer`, kèm confidence score từ ML workflow.
- `hasPhone.csv`: `Customer` tới `Phone`.
- `hasEmail.csv`: `Customer` tới `Email`.
- `hasAddress.csv`: `Customer` tới `Address`.

Pattern tạo relationship:

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

Các file CSV sau khi transform được ghi lại vào S3 theo thư mục riêng như `neptune/nodes/` và `neptune/edges/`. Cách tách này giúp node định danh, node liên hệ và file relationship sẵn sàng cho Neptune Analytics loader.

## Nạp thêm dữ liệu giao dịch

Dữ liệu định danh khách hàng được bổ sung bằng dữ liệu giao dịch giả lập để mô phỏng luồng CNP transaction. Bài viết dùng các thư viện Python như `random`, `uuid`, `csv`, `boto3`, `datawangler` và `Faker`.

Dataset giao dịch bổ sung:

- `CreditCardAccount` node gắn với một phần customer,
- `CnpCreditCardTxInit` node cho giao dịch CNP được khởi tạo,
- `CreditCardTx` node cho giao dịch CNP thành công,
- `CnpInitFail` node cho giao dịch CNP thất bại,
- các relationship `HAS_CC_ACCOUNT`, `HAS_CNP_TX_INIT`, `HAS_TX` và `HAS_FAIL`.

Transaction generator tạo ngẫu nhiên một hoặc nhiều CNP initiation event cho mỗi card account. Một số initiation trở thành giao dịch thành công, một số trở thành failure với reason code. Trong bài, reason code `3` được dùng ở phần sau như mã lỗi có rủi ro cao nhất.

Logic tạo transaction có thể viết lại như sau:

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

Các file graph được tạo trong bài gồm:

- `blog_cc_nodes.csv`
- `blog_member_to_cc_acct_rels.csv`
- `blog_cnp_init_nodes.csv`
- `blog_cc_to_cnp_init_rels.csv`
- `blog_cnp_tx_nodes.csv`
- `blog_cnp_to_cnp_tx_rels.csv`
- `blog_cnp_fail_nodes.csv`
- `blog_cnp_to_fail_rels.csv`

Sau khi tạo file, dữ liệu được upload lên S3 prefix phục vụ Neptune bulk loading. Node file đặt dưới `neptune/nodes/`; relationship file đặt dưới `neptune/edges/`.

Sau khi file CSV được tạo và upload lên S3, Neptune Analytics có thể nạp dữ liệu bằng batch load:

```sql
CALL neptune.load({
  source: "s3://<bucket-or-prefix>",
  region: "<region>",
  format: "csv"
})
```

## Phân tích output bằng Neptune Analytics

Neptune Analytics được dùng thông qua Neptune Workbench để chạy graph algorithm và OpenCypher query. Bài viết tập trung vào Louvain community detection và weakly connected components (WCC).

Louvain community detection giúp tìm các community có mật độ liên kết cao bằng cách tối ưu modularity. Trong use case này, thuật toán có thể phát hiện các cụm account, transaction initiation và CNP failure liên kết chặt hơn bình thường.

Weakly connected components gom các node có liên kết với nhau qua những edge label được chọn, bỏ qua hướng của cạnh. Cách này giúp tìm các customer đã resolve nhưng có thể chia sẻ ít nhất một thuộc tính matching như phone, email, address, customer group hoặc account relationship.

Các bước phân tích chính:

1. Chạy Louvain trên subgraph liên quan tới CNP failure và ghi community ID vào `CNPFailCommunity`.
2. Chạy WCC trên các quan hệ identity/account và ghi component ID vào `WCC`.
3. Tìm CNP failure community lớn nhất.
4. Lấy customer và PII attributes liên quan tới community đó và cùng WCC.
5. Lọc CNP failure có rủi ro cao, đặc biệt record có `reason_code = "3"`.
6. Đếm số AWS Entity Resolution match group khác nhau gắn với nhóm failure rủi ro.
7. So sánh PII dùng chung và PII không dùng chung để đánh giá hai match group có thể vẫn liên quan hay không.
8. Tính tỷ lệ known bad actor trên tổng số customer trong từng match group để ưu tiên điều tra.

Query đầu tiên thu hẹp graph vào customer, credit card account và CNP failure, sau đó ghi Louvain community property tên `CNPFailCommunity`:

```sql
MATCH (:Customer)-[:HAS_CC_ACCOUNT]->(:CreditCardAccount)-[*1..]->(:CnpInitFail)
CALL neptune.algo.louvain.mutate({
  edgeLabels: ["HAS_CC_ACCOUNT", "HAS_TX", "HAS_CNP_TX_INIT", "HAS_FAIL"],
  writeProperty: "CNPFailCommunity"
})
YIELD success
RETURN success
```

Lời gọi thứ hai tạo weakly connected components trên các quan hệ identity và account. Kết quả được ghi vào property `WCC`:

```sql
CALL neptune.algo.wcc.mutate({
  edgeLabels: ["HAS_ADDRESS", "HAS_EMAIL", "HAS_FAIL", "HAS_CUSTOMER", "HAS_PHONE", "HAS_CC_ACCOUNT"],
  writeProperty: "WCC"
})
YIELD success
RETURN success
```

Sau khi graph đã có các property này, truy vấn tiếp theo tìm CNP failure community lớn nhất:

```sql
MATCH (failure:CnpInitFail)
RETURN failure.CNPFailCommunity AS communityId, count(*) AS failedTransactionCount
ORDER BY failedTransactionCount DESC
LIMIT 1
```

Kết quả đó được dùng để kiểm tra customer và PII attribute nằm trong cùng WCC cluster:

```sql
MATCH (failure:CnpInitFail)
WITH failure.CNPFailCommunity AS communityId
MATCH (customer:Customer {CNPFailCommunity: communityId})
MATCH (:Customer {WCC: customer.WCC})-->(pii)
RETURN customer, pii
```

Sau khi có community, phần phân tích tìm nhóm failure lớn nhất rồi truy xuất các thuộc tính định danh liên quan.

![Kết quả cụm CNP failure lớn nhất](/images/3-BlogsPosted/blog1-neptune-largest-failure-cluster.png)

Ở bước phân tích sâu hơn, workflow đi vào các node `CnpInitFail` có reason code rủi ro cao nhất. Truy vấn kiểm tra liệu nhiều resolved group có cùng liên quan tới một failure community hay không:

```sql
MATCH (failure:CnpInitFail {reason_code: "3"})
WITH failure.CNPFailCommunity AS communityId
MATCH (group:Group)-->(customer:Customer {CNPFailCommunity: communityId})
RETURN group.WCC AS identityCluster, count(DISTINCT group) AS resolvedGroupCount
ORDER BY resolvedGroupCount DESC
```

Tiếp theo là bước so sánh PII dùng chung giữa các resolved group khác nhau. Nếu hai group có email, phone hoặc address khác nhau nhưng vẫn chia sẻ ít nhất một identifier, đó có thể là hoạt động liên quan cần điều tra thêm. Bài gốc diễn giải rằng phần visualization có thể cho thấy hai `Group` node riêng và bốn party, trong đó các group khác nhau ở một số PII nhưng vẫn dùng chung số điện thoại và ít nhất một địa chỉ.

![Trực quan hóa graph trên Neptune](/images/3-BlogsPosted/blog1-neptune-graph-visualization.png)

Bài viết cũng tính phần trăm bad actor trong từng match group:

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

Nhóm có tỷ lệ bad actor cao có thể được ưu tiên, nhưng vẫn phải xem tổng số customer trong group. Ví dụ, tỷ lệ 50% trong nhóm chỉ có 2 customer cần được hiểu khác với tỷ lệ thấp hơn nhưng nằm trong nhóm lớn hơn nhiều.

![Kết quả tỷ lệ bad actor](/images/3-BlogsPosted/blog1-risk-ratio-output.png)

Graph Explorer cũng được nhắc tới như một công cụ trực quan hóa bổ sung, phù hợp khi cần duyệt graph data mà không viết query.

## Cleanup

Cần cleanup để tránh phát sinh chi phí. Các tài nguyên cần xóa gồm:

- AWS Entity Resolution matching workflow,
- AWS Glue Tables,
- AWS Glue Crawlers,
- Neptune notebooks,
- Neptune Analytics graphs,
- S3 bucket hoặc object đã tạo cho bài thực hành.

## Kết luận

Bài viết cho thấy AWS Entity Resolution và Amazon Neptune Analytics có thể kết hợp để xử lý bài toán CNP fraud detection. Entity Resolution chuẩn hóa và match dữ liệu khách hàng, còn Neptune Analytics biến identity đã resolve và transaction thành graph để truy vấn, trực quan hóa và phân tích bằng graph algorithm.

Lợi ích chính nằm ở chiều sâu điều tra. Thay vì chỉ kiểm tra từng record riêng lẻ, analyst có thể xem quan hệ giữa customer, account, contact detail và transaction failure. Neptune Workbench cung cấp môi trường thực tế để chạy query, nhìn các cụm đáng ngờ và ưu tiên điều tra gian lận.

## Về tác giả

<table>
  <tr>
    <td width="130" valign="top">
      <img src="/images/3-BlogsPosted/blog1-author-jessica-hung.jpeg" width="120" alt="Jessica Hung" />
    </td>
    <td valign="top">
      <strong>Jessica Hung</strong><br/>
      Senior Data Architect tại AWS Professional Services. Công việc của cô tập trung vào ứng dụng dữ liệu có khả năng mở rộng, gồm graph database và entity resolution workload cho khách hàng, trong đó có lĩnh vực tài chính.
    </td>
  </tr>
</table>

<table>
  <tr>
    <td width="130" valign="top">
      <img src="/images/3-BlogsPosted/blog1-author-ross-gabay.png" width="120" alt="Ross Gabay" />
    </td>
    <td valign="top">
      <strong>Ross Gabay</strong><br/>
      Principal Data Architect trong AWS Professional Services. Ông làm việc với khách hàng AWS để triển khai các giải pháp enterprise-grade bằng Amazon Neptune và các dịch vụ AWS khác.
    </td>
  </tr>
</table>

<table>
  <tr>
    <td width="130" valign="top">
      <img src="/images/3-BlogsPosted/blog1-author-isaac-owusu.jpeg" width="120" alt="Isaac Kwasi Owusu" />
    </td>
    <td valign="top">
      <strong>Isaac Kwasi Owusu</strong><br/>
      Senior Data Architect tại AWS, có kinh nghiệm thiết kế và triển khai các giải pháp dữ liệu quy mô lớn cho doanh nghiệp, chuyên sâu về NoSQL database và graph database.
    </td>
  </tr>
</table>
