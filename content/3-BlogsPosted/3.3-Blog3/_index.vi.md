---
title: "Blog 3"
date: 2026-04-02
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Tối ưu chi phí truyền dữ liệu khi sử dụng AWS Network Load Balancer

**Nguồn:** [AWS Networking & Content Delivery Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/optimizing-data-transfer-costs-when-using-aws-network-load-balancer/)

**Tác giả:** Luis Felipe Silveira da Silva, Lucas Rolim

**Ngày đăng:** 02/04/2026

**Nhóm nội dung:** Best Practices, Cloud Cost Optimization, Elastic Load Balancing, Networking & Content Delivery

**Dịch vụ và khái niệm chính:** AWS Network Load Balancer, Elastic Load Balancing, Availability Zones, Amazon Route 53 Resolver, cross-zone load balancing, zonal affinity, Amazon VPC, AWS Transit Gateway, VPC peering, AWS Network Manager

---

## Giới thiệu

Bài viết tập trung vào một vấn đề chi phí khá thực tế trong kiến trúc multi-AZ: chi phí truyền dữ liệu giữa các Availability Zone khi sử dụng AWS Network Load Balancer. NLB thường được chọn cho traffic TCP, TLS hoặc UDP cần hiệu năng cao, nhưng vị trí của client, NLB node và target vẫn có thể làm phát sinh chi phí inter-zone.

Ý chính của bài là: traffic rẻ nhất là traffic nằm trong cùng một Availability Zone. Để tiến gần hơn tới mô hình đó, bài viết kết hợp hai nhóm cấu hình:

- giữ traffic từ client tới NLB ở cùng AZ bằng Availability Zone DNS affinity,
- giữ traffic từ NLB tới target ở cùng AZ bằng cách kiểm soát cross-zone load balancing.

![Tổng quan chi phí data transfer của NLB](/images/3-BlogsPosted/blog3-00-nlb-cost-hero.png)

## Mô hình chi phí inter-zone data transfer

Khi traffic đi qua ranh giới Availability Zone, AWS áp dụng phí inter-zone data transfer. Bài viết dùng mức `0.01 USD mỗi GB` ở mỗi đầu truyền dữ liệu, tức là một lượt đi qua AZ được minh họa thành `0.02 USD mỗi GB`. Khi áp dụng cho môi trường thật, vẫn cần kiểm tra lại AWS pricing page vì giá có thể thay đổi theo thời điểm và Region.

Chi phí phụ thuộc vào vị trí của client, NLB node và target:

- Client ở AZ A, NLB ở AZ B, target ở AZ B: đoạn client-to-NLB đi qua AZ, nên bài viết tính `0.02 USD mỗi GB`. Đoạn NLB-to-target vẫn nằm trong AZ B.
- Client ở AZ A, NLB ở AZ B, target ở AZ A: traffic đi từ client sang NLB rồi quay lại AZ của target. Bài viết tính hai lần đi qua AZ, tương đương `0.04 USD mỗi GB` cho mỗi chiều của luồng này.
- Client, NLB và target cùng nằm trong một AZ: luồng request này không phát sinh inter-zone data transfer charge.

![Client khác AZ, NLB và target cùng AZ](/images/3-BlogsPosted/blog3-01-client-nlb-cross-az.png)

*Hình 1 - Client nằm ở AZ khác, còn NLB và target cùng nằm trong một AZ.*

![Traffic đi qua ranh giới AZ hai lần](/images/3-BlogsPosted/blog3-02-double-cross-az.png)

*Hình 2 - Cách đặt client, NLB và target có thể tạo hai lần cross-AZ hop.*

![Client, NLB và target cùng AZ](/images/3-BlogsPosted/blog3-03-same-az-no-transfer.png)

*Hình 3 - Khi toàn bộ request path nằm trong cùng AZ, luồng này tránh được phí inter-zone.*

## Traffic từ client tới NLB

Phần đầu tiên của kiến trúc là đường đi từ client tới NLB. Bài viết dùng ví dụ internal NLB được truy cập bởi client trong cùng VPC, nhưng logic về vị trí AZ cũng áp dụng khi client truy cập NLB thông qua AWS Transit Gateway hoặc VPC peering.

Mặc định, NLB sử dụng `0 percent zonal affinity` cho hành vi DNS. Với thiết lập này, Amazon Route 53 Resolver có thể trả về các IP khỏe mạnh của NLB ở bất kỳ AZ nào mà NLB đang hoạt động. Cách này thường giúp phân tán traffic tốt hơn, nhưng cũng có nghĩa là client ở AZ A có thể kết nối tới NLB Elastic Network Interface ở AZ B và tạo inter-zone traffic.

Để giảm chi phí này, bài viết đề xuất bật `100 percent zonal affinity` khi workload được thiết kế để ưu tiên zone-local routing. Khi đó, DNS response sẽ ưu tiên IP của NLB trong cùng AZ với client. Nếu AZ của client không còn IP NLB khỏe mạnh, DNS vẫn có thể trả IP ở AZ khác để ứng dụng tiếp tục hoạt động.

### Điểm cần cân nhắc

Zonal affinity giúp giảm cross-AZ traffic, nhưng cũng thay đổi cách traffic được phân phối. Nếu phần lớn client nằm trong một AZ, NLB node và target trong AZ đó có thể nhận nhiều traffic hơn các AZ còn lại. Sự lệch tải này càng rõ hơn khi cross-zone load balancing bị tắt.

Cách xử lý nằm ở capacity planning theo AZ:

- giữ phân bố client tương đối đều giữa các AZ khi có thể,
- áp dụng best practice về DNS và TCP connection reuse để tránh việc client ghim traffic ngoài dự kiến,
- sizing target capacity theo traffic dự kiến của từng AZ,
- dùng Amazon EC2 Auto Scaling, Amazon ECS, Amazon EKS hoặc AWS Elastic Beanstalk để điều chỉnh capacity.

### Bật zonal affinity

Trong AWS Management Console, cấu hình này nằm trong thuộc tính của NLB:

1. Mở Amazon EC2 console.
2. Vào **Load Balancers**.
3. Chọn NLB cần cấu hình.
4. Mở tab **Attributes** và chọn **Edit**.
5. Trong **Availability Zone routing configuration**, đặt **Client routing policy (DNS record)** thành **Availability Zone affinity** hoặc **Partial Availability Zone affinity**.
6. Lưu thay đổi.

![Cấu hình Availability Zone affinity cho NLB](/images/3-BlogsPosted/blog3-04-nlb-az-affinity.png)

*Hình 4 - Chính sách client routing cho DNS record của NLB.*

Với AWS CLI, cấu hình tương ứng dùng thuộc tính `dns_record.client_routing_policy`:

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn <nlb-arn> \
  --attributes Key=dns_record.client_routing_policy,Value=availability_zone_affinity
```

## Traffic từ NLB tới target

Phần thứ hai là đường đi từ NLB node tới các target đã đăng ký. Đường đi này được kiểm soát bằng cross-zone load balancing ở cấp load balancer và target group.

Khi cross-zone load balancing được bật, một NLB node có thể gửi traffic tới target ở bất kỳ AZ nào đang được bật. Điều này có thể làm phân bố tải giữa target đều hơn, nhưng cũng tạo inter-zone data transfer khi target được chọn nằm ở AZ khác.

Đối với Network Load Balancer, cross-zone load balancing mặc định bị tắt ở cả cấp load balancer và target group. Mặc định này hỗ trợ zone-local routing, nhưng cách bố trí target phải được tính kỹ. Nếu một AZ có ít target khỏe mạnh hơn AZ khác, việc tắt cross-zone load balancing có thể làm AZ đó bị quá tải.

### Cân nhắc về capacity

Với thiết kế NLB tối ưu chi phí, mỗi AZ được bật nên có đủ target local để phục vụ lượng traffic dự kiến trong AZ đó. Số lượng target giữa các AZ không nhất thiết luôn bằng nhau, nhưng nên tỷ lệ với phân bố client và kế hoạch capacity của ứng dụng.

Các điểm cần kiểm tra:

- kiểm tra target health theo từng AZ,
- so sánh request volume theo Availability Zone của NLB,
- so sánh CPU, memory, connection và error của target theo AZ,
- scale target theo zone nếu traffic client không phân bố đều,
- quyết định rõ target group sẽ kế thừa cấu hình của load balancer hay override.

### Tắt cross-zone load balancing

Ở cấp load balancer, thao tác trên console như sau:

1. Mở Amazon EC2 console.
2. Trong **Load Balancing**, chọn **Load Balancers**.
3. Chọn NLB.
4. Mở **Attributes** và chọn **Edit**.
5. Tắt **Cross-zone load balancing**.
6. Lưu thay đổi.

![Tắt cross-zone load balancing cho NLB](/images/3-BlogsPosted/blog3-05-disable-cross-zone-nlb.png)

*Hình 5 - Cross-zone load balancing bị tắt ở cấp NLB.*

Lệnh AWS CLI tương ứng dùng `load_balancing.cross_zone.enabled`:

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn <nlb-arn> \
  --attributes Key=load_balancing.cross_zone.enabled,Value=false
```

Ở cấp target group, có thể chỉnh trong **Target Groups > Attributes > Edit**. Với pattern tối ưu chi phí theo local-AZ, chính sách của target group cần thống nhất với hành vi cross-zone mong muốn của load balancer.

![Tắt cross-zone load balancing cho target group của NLB](/images/3-BlogsPosted/blog3-06-disable-cross-zone-target-group.png)

*Hình 6 - Cấu hình cross-zone load balancing ở cấp target group.*

Ví dụ AWS CLI cho target group:

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn <target-group-arn> \
  --attributes Key=load_balancing.cross_zone.enabled,Value=false
```

## Availability Zone Independence

Bài viết cũng liên hệ pattern tối ưu chi phí với Availability Zone Independence. AZI nghĩa là ứng dụng có thể tiếp tục phục vụ traffic trong từng AZ mà không phụ thuộc vào AZ khác trên request path.

Với NLB, pattern này gồm:

- bật 100 percent zonal affinity để client ưu tiên IP NLB trong cùng AZ,
- tắt cross-zone load balancing để NLB node gửi traffic tới target cùng AZ,
- duy trì target khỏe mạnh và đủ capacity trong mọi AZ được bật,
- chuẩn bị quy trình zonal evacuation khi một AZ bị lỗi hoặc suy giảm.

![Availability Zone Independence với NLB](/images/3-BlogsPosted/blog3-07-azi-with-nlb.png)

*Hình 7 - Pattern Availability Zone Independence với NLB.*

Cấu hình này có thể giảm chi phí data transfer và cũng có thể giảm packet latency vì request path không đi qua ranh giới AZ không cần thiết. Bài viết nhắc tới Infrastructure Performance trong AWS Network Manager như một cách theo dõi real-time inter-AZ latency.

## Ghi chú dùng cho báo cáo

Bài blog này phù hợp để đưa vào báo cáo vì nó cho thấy cost optimization không chỉ là chọn dịch vụ rẻ hơn. Cách thiết kế network và vị trí traffic có thể làm thay đổi chi phí dù vẫn dùng cùng một nhóm dịch vụ.

Các ý chính có thể dùng lại:

- Khi review chi phí NLB, cần xem vị trí client, subnet của NLB và vị trí target.
- `0 percent zonal affinity` giúp DNS phân phối rộng, còn `100 percent zonal affinity` ưu tiên local-AZ routing.
- Cross-zone load balancing giúp phân phối tải đều hơn nhưng có thể tạo inter-zone data transfer.
- Khi tắt cross-zone load balancing, target capacity phải được cân bằng theo AZ.
- Availability Zone Independence hỗ trợ cả kiểm soát chi phí và khả năng chịu lỗi, nhưng cần monitoring vận hành.

## Checklist triển khai

Với workload thật, nên kiểm tra cấu hình theo thứ tự:

1. Vẽ request path từ client tới NLB rồi tới target.
2. Xác định traffic đang đi qua ranh giới AZ bao nhiêu lần.
3. Kiểm tra NLB DNS client routing policy.
4. Kiểm tra cross-zone load balancing ở cấp load balancer.
5. Kiểm tra cross-zone behavior ở cấp target group.
6. Xác nhận target capacity theo từng AZ.
7. Theo dõi NLB metrics, target health, target utilization và inter-AZ latency.
8. Tính lại chi phí sau khi traffic pattern thay đổi.

## Minh chứng chia sẻ Facebook

Bài tóm tắt kỹ thuật đã được chia sẻ lên cộng đồng AWS Study Group VN và hiện đang trong trạng thái chờ quản trị viên phê duyệt. Ảnh dưới đây được dùng làm minh chứng tạm thời cho đến khi có URL bài đăng công khai.

![Minh chứng Facebook đang chờ duyệt cho Blog 3](/images/3-BlogsPosted/blog3-fb-pending.png)

## Về tác giả

![Luis Felipe Silveira da Silva](/images/3-BlogsPosted/blog3-author-luis-felipe.jpg)

**Luis Felipe Silveira da Silva** là Principal Solutions Architect trong AWS Application Networking team, làm việc tại Dublin. Trọng tâm công việc của ông là hỗ trợ khách hàng thiết kế workload resilient với các dịch vụ networking và load balancing của AWS.

![Lucas Rolim](/images/3-BlogsPosted/blog3-author-lucas-rolim.jpg)

**Lucas Rolim** là Senior Solutions Architect tại AWS, làm việc tại Sydney trong Application Networking team. Trọng tâm kỹ thuật của ông gồm networking và security.
