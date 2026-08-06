---
title: "Event 3"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# BÁO CÁO TÓM TẮT: AGENT FORGE - DEEPDIVE DAY 1

## Mục tiêu sự kiện

- **Mục tiêu:** Giới thiệu có hệ thống về Amazon Bedrock AgentCore và các thành phần nền tảng cần thiết để xây dựng, triển khai và bảo vệ AI agent trên AWS.
- **Trọng tâm học tập:** Hiểu mô hình runtime của AgentCore, gateway tích hợp công cụ, lớp identity và các bước hands-on đầu tiên để triển khai agent có kết nối external tools, Knowledge Bases, Web UI và Amazon Cognito authentication.

## Thông tin sự kiện

- **Tên sự kiện:** Agent Forge - Deepdive Day 1
- **Thời gian:** 01/08/2026, 09:00 - 12:00
- **Địa điểm:** Bitexco Financial Tower, Thành phố Hồ Chí Minh
- **Vai trò trong sự kiện:** Người tham dự
- **Chủ đề chính:** Amazon Bedrock AgentCore, agent runtime, gateway, identity, external tool integration, Knowledge Bases, Web UI và Amazon Cognito authentication

## Diễn giả

- **Nghia Tran:** Agentic SA. Phụ trách phần lý thuyết, bao gồm tổng quan Amazon Bedrock AgentCore và các thành phần cốt lõi dùng để vận hành AI agent trên AWS.
- **Anh Pham:** Cloud Consultant, G-AsiaPacific VietNam. Hỗ trợ góc nhìn triển khai và cloud implementation, kết nối các khái niệm AgentCore với phần thực hành triển khai và tích hợp.

## Nội dung chương trình

### Phiên 1: Introduction to Amazon Bedrock AgentCore

**Thời gian:** 09:00 - 10:00

Phiên đầu tiên trình bày nền tảng kỹ thuật của Amazon Bedrock AgentCore và giải thích vì sao một ứng dụng agentic không chỉ dừng lại ở việc gọi foundation model. Một agent có khả năng dùng trong thực tế cần runtime, cơ chế truy cập công cụ có kiểm soát, xử lý identity và các điểm tích hợp với hệ thống bên ngoài.

Các nội dung chính:

- **Giới thiệu workshop:** Tổng quan về chuỗi AgentForge 3 ngày và mục tiêu của Day 1.
- **Amazon Bedrock AgentCore Overview:** Định vị AgentCore như một lớp nền tảng để xây dựng và vận hành AI agent.
- **Runtime:** Cách agent được thực thi, cách request được xử lý và cách hành vi của agent được tổ chức khi chạy.
- **Gateway:** Cách external tools và services được expose cho agent thông qua một lớp tích hợp có kiểm soát.
- **Identity:** Vai trò của authentication và identity context khi agent tương tác với dữ liệu người dùng hoặc hệ thống nghiệp vụ được bảo vệ.

### Phiên 2: Hands-on Lab

**Thời gian:** 10:00 - 11:00

Phần hands-on tập trung vào luồng triển khai đầu tiên cho một ứng dụng agent-based. Lab giúp nối phần lý thuyết ở phiên đầu với một hướng triển khai cụ thể.

Các hoạt động chính:

- **Triển khai basic agent trong AgentCore:** Tạo và chạy agent ban đầu để hiểu luồng deployment.
- **Kết nối external tools và Knowledge Bases:** Mở rộng agent để có thể truy xuất thông tin và thực hiện hành động thông qua tài nguyên được kết nối.
- **Xây dựng Web UI:** Tạo giao diện cơ bản để người dùng tương tác với agent.
- **Tích hợp Amazon Cognito authentication:** Bổ sung lớp xác thực để kiểm soát truy cập người dùng tốt hơn.

## Điểm nổi bật

### AgentCore như lớp vận hành cho AI Agent

Sự kiện làm rõ rằng một AI agent không nên được xem chỉ là prompt kết nối với foundation model. Một hệ thống agent hữu ích cần hạ tầng hỗ trợ xung quanh việc thực thi, tích hợp, xác thực và kiểm soát vận hành. Amazon Bedrock AgentCore đi theo hướng này bằng cách cung cấp các thành phần giúp chuẩn hóa cách agent được xây dựng và vận hành.

### Runtime, Gateway và Identity là các khối nền tảng

Ba khái niệm có vai trò đặc biệt quan trọng:

- **Runtime:** Xác định cách agent chạy và xử lý request.
- **Gateway:** Cung cấp cơ chế có kiểm soát để kết nối tools và external systems.
- **Identity:** Đảm bảo hành động của agent gắn với đúng user context và access control.

Các thành phần này quan trọng vì ứng dụng AI trong doanh nghiệp thường cần gọi công cụ nội bộ, truy xuất tri thức doanh nghiệp và áp dụng phân quyền theo người dùng.

### Từ prototype đến ứng dụng có kiểm soát

Phần lab cho thấy sự khác biệt giữa một AI demo đơn giản và một agent application có kiểm soát hơn. Việc bổ sung Web UI và Amazon Cognito authentication giúp hệ thống gần hơn với một ứng dụng thực tế, nơi access control và identity management cần được thiết kế ngay từ đầu.

## Bài học kỹ thuật

### Agentic Application cần tư duy kiến trúc

Xây dựng agent không chỉ là chọn một model mạnh. Kiến trúc xung quanh phải xác định agent được truy cập gì, xác thực người dùng như thế nào, tools được expose ra sao và Knowledge Bases được quản trị theo nguyên tắc nào.

### Tool Access cần được thiết kế cẩn thận

Khi agent có khả năng gọi external tools, hệ thống cần định nghĩa rõ hành động được phép, ranh giới bảo mật và kết quả mong đợi. Nếu thiếu gateway và identity layer có kiểm soát, phần tool integration sẽ khó audit và khó bảo mật.

### Knowledge Bases cải thiện ngữ cảnh nhưng cần governance

Kết nối Knowledge Bases giúp agent trả lời dựa trên ngữ cảnh chuyên biệt. Tuy nhiên, chất lượng câu trả lời phụ thuộc vào chất lượng nguồn dữ liệu, thiết kế retrieval và permission boundary quanh tri thức được lưu trữ.

### Authentication là một phần của thiết kế agent

Tích hợp Amazon Cognito cho thấy identity không phải là phần phụ trong ứng dụng thực tế. Nếu agent tương tác với dữ liệu theo từng người dùng, identity và authorization nên được đưa vào kiến trúc từ sớm.

## So sánh kỹ thuật

| Tiêu chí | LLM chatbot đơn giản | Agent application dựa trên AgentCore |
| :--- | :--- | :--- |
| **Mô hình thực thi** | Chủ yếu là tương tác prompt-response. | Có runtime có cấu trúc cho việc thực thi và điều phối agent. |
| **Tích hợp công cụ** | Thường hardcode hoặc kết nối thủ công. | Tích hợp thông qua gateway layer có kiểm soát. |
| **Truy cập tri thức** | Phụ thuộc nhiều vào kiến thức của model hoặc prompt context tĩnh. | Có thể kết nối Knowledge Bases để truy xuất tri thức theo miền nghiệp vụ. |
| **Identity Handling** | Thường bị bỏ qua ở prototype. | Thiết kế với authentication và user context qua các dịch vụ như Amazon Cognito. |
| **Mức độ sẵn sàng cho doanh nghiệp** | Phù hợp cho demo và thử nghiệm. | Phù hợp hơn cho phát triển ứng dụng có yêu cầu bảo mật, tích hợp và kiểm soát. |

## Trải nghiệm sự kiện

- **Trải nghiệm thực hành:** Sự kiện kết nối lý thuyết và hands-on theo trình tự rõ ràng: hiểu AgentCore trước, sau đó triển khai agent và kết nối tools, Knowledge Bases, Web UI và authentication.
- **Trải nghiệm kỹ thuật:** Điểm giá trị nhất là thấy được runtime, gateway và identity phối hợp như các building blocks cho kiến trúc agent an toàn hơn.
- **Giá trị cộng đồng:** Sự kiện tạo điểm bắt đầu thực tế cho những người muốn đi từ sử dụng generative AI cơ bản sang phát triển agentic application có cấu trúc trên AWS.
- **Bài học rút ra:** Kiến trúc agent nên được thiết kế với security, identity, integration boundaries và operational control ngay từ giai đoạn đầu, đặc biệt khi agent có quyền truy cập external tools hoặc business data.

## Ứng dụng vào công việc

- Xem AI agent như một cloud application cần thiết kế kiến trúc, không chỉ là prompt gọi model.
- Định nghĩa quyền truy cập tool và ranh giới bảo mật trước khi cho agent thực hiện hành động.
- Sử dụng Knowledge Bases khi agent cần ngữ cảnh chuyên biệt và có kiểm soát.
- Đưa authentication và identity management vào sớm khi thiết kế ứng dụng agent cho người dùng cuối.
- Đánh giá agent không chỉ theo chức năng, mà còn theo security, governance và maintainability.

## Minh chứng tham dự

![Minh chứng tham dự Agent Forge Deepdive Day 1](/images/4-EventParticipated/event3-agent-forge-day1.jpg)
