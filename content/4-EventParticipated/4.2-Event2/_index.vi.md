---
title: "Sự kiện 2"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# BÀI THU HOẠCH: AWS TECH MEETUP & COMMUNITY KNOWLEDGE SHARING

## Mục đích của sự kiện

- **Mục đích:**
  - Chia sẻ kinh nghiệm thực tế về độ tin cậy hệ thống, monitoring, cảnh báo sự cố và bảo mật ứng dụng web trên AWS.
  - Đưa ra hướng ôn tập rõ ràng cho chứng chỉ AWS Certified Cloud Practitioner CLF-C02.
  - Trao đổi cách Generative AI và AI Agent hỗ trợ DevSecOps qua design review, code review và kiểm thử xâm nhập tự động.
  - Tạo không gian trao đổi kỹ thuật cho thành viên AWS First Cloud AI Journey, kỹ sư hạ tầng và người làm bảo mật.

## Danh sách diễn giả

- **Danh sách diễn giả:**
  - **Nguyễn Huỳnh Sơn:** Infrastructure Support Engineer tại Endava, Ex-Infrastructure Reliability Engineer tại SPS, Thành viên AWS Student Builder Group HUFLIT.
  - **Ngô Lê Tấn Huy:** Speaker chia sẻ lộ trình chứng chỉ AWS Cloud Practitioner.
  - **Nguyễn Tuấn Thịnh:** DevOps/DevSecOps/Cloud Engineer tại Styl Solutions, Thành viên First Cloud AI Journey.

## Nội dung nổi bật

### Chuyên đề 1: SLA and Monitoring - From SLA to Monitoring What Really Matters

**Diễn giả:** Nguyễn Huỳnh Sơn

Chuyên đề đầu tiên tập trung vào một hiểu lầm thường gặp khi vận hành hệ thống: dashboard hạ tầng có thể đang xanh, nhưng người dùng vẫn gặp lỗi. SLA không chỉ là một cam kết trên giấy; với đội kỹ thuật, SLA cần được chuyển thành monitoring, cảnh báo và cải tiến vận hành.

Các ý chính của phần này:

- **SLA:** Cam kết về mức độ dịch vụ giữa bên cung cấp và người dùng hoặc khách hàng.
- **Monitoring:** Một hoạt động trong quản trị rủi ro, giúp phát hiện tín hiệu bất thường trước khi SLA bị ảnh hưởng.
- **Vòng lặp rủi ro:** Xác định rủi ro, giám sát tín hiệu, phản ứng bằng alarm hoặc SOP, sau đó review và cải tiến.
- **Monitoring pyramid:** Cloud provider layer, infrastructure layer, application layer, business layer và customer experience layer.
- **Thông điệp chính:** Hạ tầng khỏe không đồng nghĩa với trải nghiệm người dùng tốt.

Ví dụ đáng nhớ là một hệ thống có CPU bình thường, ALB target healthy và endpoint `/health` trả về `200 OK`, nhưng người dùng thật vẫn đăng nhập thất bại vì ứng dụng không kết nối được tới RDS. Từ đó có thể thấy monitoring nên có thêm business custom metric như login success, order success hoặc payment failure, thay vì chỉ nhìn CPU và memory.

Luồng cảnh báo được đề xuất:

1. Ghi nhận custom metric cho các hành động quan trọng của người dùng, ví dụ `LoginFailure`.
2. Cấu hình CloudWatch Alarm với threshold có ý nghĩa.
3. Gửi cảnh báo qua SNS Topic tới Email hoặc Slack.
4. Sau khi xử lý, review incident và cập nhật lại thiết kế monitoring.

Chuyên đề cũng nhắc lại tư duy vận hành của Dr. Werner Vogels: hệ thống phân tán luôn có khả năng lỗi, vì vậy kiến trúc cần được chuẩn bị trước cho tình huống lỗi.

### Chuyên đề 2: Inside The Exam - AWS Certified Cloud Practitioner CLF-C02

**Diễn giả:** Ngô Lê Tấn Huy

Chuyên đề thứ hai hướng dẫn cách tiếp cận chứng chỉ AWS Certified Cloud Practitioner. Nội dung không chỉ dừng ở cấu trúc bài thi, mà còn đưa ra cách học dịch vụ AWS thông qua keyword, phạm vi trách nhiệm và các bẫy thường gặp trong câu hỏi trắc nghiệm.

Thông tin chính về bài thi:

- **Số câu hỏi:** 65 câu trắc nghiệm.
- **Thời lượng:** 90 phút, có thể được cộng thêm thời gian cho người không dùng tiếng Anh như ngôn ngữ chính.
- **Điểm đạt:** 700/1000.
- **Thời hạn chứng chỉ:** 3 năm.
- **Các domain:**
  - Cloud Concepts: 24%
  - Security and Compliance: 30%
  - Cloud Technology and Services: 34%
  - Billing, Pricing and Support: 12%

Mô hình Shared Responsibility là một nội dung rất quan trọng:

- AWS chịu trách nhiệm cho **Security OF the Cloud**, gồm cơ sở vật lý, phần cứng và hạ tầng toàn cầu.
- Khách hàng chịu trách nhiệm cho **Security IN the Cloud**, gồm dữ liệu, IAM, OS patching, security group và cấu hình ở tầng ứng dụng.

Kinh nghiệm ôn thi:

- Gắn mỗi dịch vụ AWS với một đến hai keyword, ví dụ **SQS = decoupling**, **CloudFront = content delivery**, **IAM = identity and permission**.
- Khi làm mock test, cần phân tích vì sao đáp án đúng là đúng và vì sao các đáp án còn lại sai.
- Loại trước các dịch vụ không có thật hoặc không liên quan để tăng xác suất chọn đúng.
- Với CLF-C02, nên ưu tiên đáp án đơn giản, đúng ở mức tổng quan thay vì suy diễn kiến trúc quá phức tạp.

### Chuyên đề 3: Securing Your Web Apps With AWS Security Agent

**Diễn giả:** Nguyễn Tuấn Thịnh

Chuyên đề thứ ba giới thiệu cách AI Agent hỗ trợ bảo mật ứng dụng hiện đại. Pentest thủ công thường tốn chi phí, mất nhiều thời gian và khó lặp lại thường xuyên. Phần chia sẻ đưa ra AWS Security Agent, còn được nhắc tới là Frontier Agent, như một hướng tiếp cận AI-assisted cho security review và kiểm thử tự động.

Điểm khác biệt với chatbot thông thường là agent có mục tiêu kiểm chứng phát hiện. Thay vì chỉ nói rằng có thể tồn tại lỗ hổng, agent có thể lập kế hoạch và chạy các bước kiểm thử có kiểm soát để tạo bằng chứng xác thực.

Vòng đời bảo mật được nhắc tới:

- **Design Review:** Kiểm tra tài liệu kiến trúc, ghi chú Markdown hoặc Terraform trước khi triển khai; đối chiếu với PCI DSS, NIST CSF và AWS Well-Architected.
- **Code Review:** Tích hợp với GitHub hoặc GitLab pull request, comment trực tiếp vào dòng code rủi ro và đề xuất bản vá.
- **Automated Pentesting:** Kiểm thử ứng dụng đang chạy bằng chuỗi khai thác nhiều bước như IDOR rồi XSS, sau đó xuất bằng chứng và sơ đồ tấn công.

Phần này cũng nêu rõ bài toán chi phí và giới hạn:

- Pentest truyền thống có thể tốn khoảng 5,000 đến 20,000 USD mỗi lần đánh giá và kéo dài nhiều tuần.
- Mô hình AI Agent dùng pay-as-you-go, được nhắc tới với mức 50 USD mỗi task-hour.
- Một dự án thực tế có thể giảm chi phí kiểm thử xuống khoảng 1,500 đến 2,500 USD tùy phạm vi.
- Agent vẫn có giới hạn với MFA, biometrics, mTLS và các lỗi gian lận logic nghiệp vụ phức tạp.

## Thảo luận so sánh

| Tiêu chí | Cách tiếp cận truyền thống | Cách tiếp cận AWS / AI-assisted |
| :--- | :--- | :--- |
| **Đối tượng monitoring** | Chủ yếu theo dõi CPU, RAM, disk và trạng thái hạ tầng. | Theo dõi thêm user journey và business metric như login, order, payment success. |
| **Phát hiện sự cố** | Thường bắt đầu sau khi có phản hồi người dùng hoặc điều tra thủ công. | CloudWatch Alarm và SNS giúp báo sớm qua Email hoặc Slack. |
| **Thời điểm review bảo mật** | Thường làm gần cuối vòng đời phát triển. | Shift-left, review từ thiết kế và code sớm hơn. |
| **Công sức pentest** | Thủ công, chậm và phụ thuộc vào lịch của chuyên gia. | Agent-assisted testing có thể lặp lại kiểm tra và tạo finding có bằng chứng. |
| **Cách học chứng chỉ** | Ghi nhớ nhiều tên dịch vụ nhưng thiếu nhóm ý rõ ràng. | Gắn dịch vụ với keyword, phạm vi trách nhiệm và domain bài thi. |

## Những gì học được

### Monitoring và reliability

- Dashboard xanh chưa đủ nếu người dùng vẫn không hoàn thành được tác vụ quan trọng.
- Monitoring nên đi từ hạ tầng lên user-centric và business-centric signal.
- CloudWatch custom metric, alarm và SNS notification có thể tạo luồng cảnh báo sớm trước khi sự cố lan tới khách hàng.

### Security và DevSecOps

- Bảo mật nên bắt đầu từ architecture và code review, không chỉ đợi đến pentest cuối cùng.
- AI Agent có thể giảm công việc lặp lại trong security review, nhưng kỹ sư vẫn phải kiểm chứng context, business logic và rủi ro.
- Shared Responsibility Model là ranh giới bảo mật thực tế, không chỉ là kiến thức để thi chứng chỉ.

### Chứng chỉ và tự học

- Ôn CLF-C02 nên tập trung vào khái niệm, vị trí của dịch vụ và khả năng nhận diện keyword.
- Review câu sai có giá trị hơn việc chỉ nhìn điểm mock test.
- Chiến thuật làm bài cũng hữu ích cho công việc thật vì giúp chọn dịch vụ rõ ràng và hiểu phạm vi trách nhiệm.

## Áp dụng vào công việc

- Bổ sung custom application metric cho dự án cá nhân hoặc dự án nhóm, đặc biệt là login, checkout, order và error-rate.
- Rà soát IAM policy và security group theo nguyên tắc least privilege.
- Dùng GitHub Actions hoặc các bước CI/CD tương tự để quét secret leak và lỗi phổ biến sớm.
- Xây dựng bộ ghi chú ôn chứng chỉ AWS theo keyword dịch vụ, domain bài thi và các kiểu đáp án sai thường gặp.
- Khi thiết kế cloud application, đưa monitoring và security review vào ngay bản kiến trúc đầu tiên.

## Trải nghiệm trong event

- **Trải nghiệm thực tế:** Sự kiện nối ba chủ đề thường học rời rạc: monitoring, chứng chỉ và bảo mật ứng dụng. Khi đặt cạnh nhau, lộ trình học AWS trở nên đầy đủ hơn.
- **Ấn tượng kỹ thuật:** Ví dụ monitoring rất dễ nhớ vì chỉ ra khoảng cách giữa hạ tầng khỏe và trải nghiệm người dùng. Điều này thay đổi cách nhìn về những gì cần đo trong một hệ thống cloud.
- **Giá trị cộng đồng:** Người tham dự được nghe chia sẻ từ các kỹ sư đang làm trong mảng infrastructure và DevSecOps, nên phần trao đổi thực tế hơn so với việc chỉ đọc tài liệu.
- **Bài học cá nhân:** Bài học hữu ích nhất là thiết kế hệ thống từ kết quả của người dùng đi ngược lại: đo đúng thứ người dùng cần, bảo mật workflow sớm và hiểu rõ trách nhiệm của từng dịch vụ.

## Bài học rút ra

- Monitoring nên xoay quanh hành trình thật của người dùng, không chỉ quanh mức sử dụng tài nguyên hạ tầng.
- Học chứng chỉ AWS hiệu quả hơn khi mỗi dịch vụ được gắn với keyword và ranh giới trách nhiệm.
- AI-assisted security testing có thể tăng tốc DevSecOps, nhưng vẫn cần con người kiểm chứng business logic và rủi ro production.
- Shared Responsibility Model nên được dùng cho cả ôn thi lẫn review môi trường AWS thật.

## Một số hình ảnh khi tham gia sự kiện

![event2](/images/event2.jpg)
