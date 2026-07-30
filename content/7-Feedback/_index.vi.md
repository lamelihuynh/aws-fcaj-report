---
title: "Chia sẻ, đóng góp ý kiến"
date: 2026-08-14
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

# Chia sẻ và đóng góp ý kiến

Phần này là những chia sẻ cá nhân của em sau khi tham gia chương trình thực tập **First Cloud AI Journey (FCAJ)**. Em viết phần này từ góc nhìn của một sinh viên đã học được nhiều điều, cũng gặp không ít khoảng trống kỹ thuật và dần quen hơn với cách làm việc với AWS thông qua lab, event, tài liệu và project nhóm.

## Cảm nhận chung

FCAJ cho em một môi trường học thực tế hơn nhiều so với các bài tập thông thường ở trường. Ở trường, nhiều bài có yêu cầu rõ và tiêu chí chấm điểm cố định. Trong chương trình này, em phải tiếp cận những việc gần với công việc cloud hơn: chọn dịch vụ, hiểu quyền IAM, suy nghĩ về chi phí, kiểm tra bảo mật, chuẩn bị tài liệu và giải thích lý do đằng sau một quyết định kỹ thuật.

Điều em trân trọng nhất là chương trình không chỉ giới thiệu AWS theo kiểu lý thuyết. Worklog hằng tuần, blog kỹ thuật, event và project workshop buộc em phải nối các ý lại với nhau. IAM không còn chỉ là một chủ đề phân quyền riêng lẻ, mà xuất hiện lại trong ECS task role, Lambda permission, S3 access và CI/CD automation. CloudWatch không chỉ là dịch vụ monitoring, mà liên quan đến reliability, chuẩn bị demo và nhận thức về chi phí.

## Điểm mạnh của chương trình

### 1. Định hướng học thực hành

Chương trình khuyến khích học bằng cách làm thay vì chỉ nghe. Mỗi chủ đề đều cần được nối với lab, reference hoặc project. Điều này giúp em nhớ dịch vụ AWS qua use case thật hơn là chỉ học định nghĩa.

### 2. Không khí cộng đồng tốt

Các event và hoạt động cộng đồng làm chương trình có nhiều năng lượng hơn. Khi nghe speaker chia sẻ và nhìn các đội sinh viên khác làm việc với AWS, em hiểu rằng học cloud không phải hành trình một mình. Nó cần đặt câu hỏi, so sánh cách làm và học từ những người đi trước.

### 3. Tiếp cận tư duy kỹ thuật thực tế

Chương trình giúp em học được một số tư duy quan trọng vượt ra ngoài phạm vi một project:

- bảo mật nên được nghĩ từ sớm, không chỉ kiểm tra ở cuối,
- chi phí phải được theo dõi liên tục, đặc biệt với AWS account của sinh viên,
- monitoring nên hướng tới trải nghiệm người dùng, không chỉ trạng thái hạ tầng,
- tài liệu là một phần của công việc kỹ thuật, không phải việc phụ làm sau cùng.

### 4. Có không gian để tự trưởng thành

FCAJ buộc em đối diện với những điểm yếu mà trong các project nhỏ ở trường em có thể né được. Em phải sắp xếp ghi chú, viết nội dung song ngữ, xử lý thay đổi Git, kiểm tra reference và làm cho báo cáo nhất quán. Những việc này không phải lúc nào cũng thú vị, nhưng rất cần thiết và giúp em rèn tính kỷ luật.

## Khó khăn cá nhân

Phần khó nhất với em là phải nối nhiều khái niệm cùng lúc. Một luồng deployment có thể liên quan đến Docker, ECR, IAM, ECS Fargate, security group, ALB, CloudWatch và cleanup chi phí. Ban đầu, có lúc em hiểu từng dịch vụ riêng lẻ nhưng chưa nhìn được toàn bộ đường đi của hệ thống.

Một khó khăn khác là viết báo cáo sao cho giống việc mình thật sự đã làm. Báo cáo kỹ thuật rất dễ bị quá trang trọng, quá chung chung hoặc quá giống template. Qua quá trình chỉnh sửa, em học được cách viết thật hơn: em đã học gì, hiểu đến đâu, còn thiếu gì và mỗi reference đang hỗ trợ phần nào trong báo cáo.

## Góp ý cho các khóa sau

### Những điểm chương trình làm tốt

- Lộ trình theo tuần giúp sinh viên có hướng đi rõ.
- Workshop và event cộng đồng giúp nối lý thuyết AWS với câu chuyện thực tế.
- Project cuối kỳ tạo lý do để sinh viên kết hợp cloud, DevOps, security và documentation.
- Việc có cả tiếng Anh và tiếng Việt giúp rèn khả năng diễn đạt kỹ thuật song ngữ.

### Đề xuất cải thiện

- Có thể thêm office-hour ngắn mỗi tuần để các nhóm hỏi kỹ thuật trước khi lỗi tích tụ quá nhiều.
- Nên cung cấp checklist AWS account low-cost ngay từ đầu, gồm Budgets, nhắc cleanup và các dịch vụ dễ phát sinh phí.
- Có thể đưa một mẫu reference architecture đơn giản ở giai đoạn đầu, sau đó để từng nhóm tự điều chỉnh theo project.
- Nên có một buổi ngắn về Git workflow, gồm branch strategy, rebase, conflict resolution và thói quen pull/push an toàn.
- Có thể thêm phần luyện thuyết trình trước final demo để sinh viên tự tin hơn khi giải thích quyết định kiến trúc.

## Điều em muốn nhắn với sinh viên khóa sau

Nếu có bạn sinh viên hỏi em FCAJ có đáng tham gia không, câu trả lời của em là có, nhưng nên tham gia với kỳ vọng đúng. FCAJ không phải chương trình mà mọi thứ được đưa sẵn từng bước. Chương trình cho định hướng, cộng đồng và cơ hội, nhưng mỗi người vẫn phải tự đọc, tự thử, tự sai, tự sửa và tự ghi lại quá trình học.

Những bạn nhận được nhiều giá trị nhất từ FCAJ có lẽ là những bạn chịu đặt câu hỏi, giữ ghi chú đều, cleanup sau lab và xem lại vì sao một cấu hình chạy được thay vì chỉ copy command.

## Lời kết

Em biết ơn team AWS, các anh chị mentor, speaker và các bạn trong cộng đồng FCAJ vì đã tạo ra một môi trường để sinh viên được chạm gần hơn với cloud engineering thực tế. Kỳ thực tập này giúp em nghiêm túc hơn với cách học kỹ thuật và cách trình bày kết quả học tập của mình.

Chương trình cũng nhắc em rằng để giỏi hơn trong cloud engineering là một quá trình dài. Báo cáo này không phải điểm kết thúc, mà là một cột mốc cho thấy em đã học được gì và cần tiếp tục cải thiện điều gì sau kỳ thực tập.
