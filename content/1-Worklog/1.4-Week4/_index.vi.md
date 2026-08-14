---
title: "Worklog Tuần 4"
date: 2026-07-24
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:
- Nghiên cứu, phân tích và lựa chọn đề tài dự án thực tế phù hợp để tổng hợp, áp dụng toàn diện các kiến thức AWS nền tảng đã tích lũy từ các tuần trước vào dự án.
- Xây dựng và thiết kế kiến trúc Serverless sơ bộ, đồng thời nghiên cứu kỹ lưỡng để xác định chính xác các dịch vụ và công nghệ cốt lõi cấu thành hệ thống cho EDMS (Enterprise Document Collaboration Platform).

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :---: | :--- | :---: | :---: |
| 1 | Lên ý tưởng dự án<br>- Khảo sát các bài toán thực tế về quản lý và cộng tác tài liệu nội bộ trong doanh nghiệp vừa và nhỏ.<br>- Thống nhất xây dựng Hệ thống Quản lý & Cộng tác Tài liệu Doanh nghiệp (EDMS) theo mô hình Serverless hiện đại trên AWS nhằm tối ưu chi phí và tự động scale theo tải. | 12/07/2026 | 12/07/2026 |
| 2 | Lựa chọn công nghệ & Dịch vụ AWS<br>- Chọn Amazon Cognito (xác thực & phân quyền nhóm ADMIN/MANAGER/USER), Amazon S3 (lưu trữ file gốc, pre-signed URL), và Aurora MySQL (lưu trữ metadata quan hệ qua Spring Data JPA).<br>- Tích hợp AWS Lambda chạy ứng dụng Monolith Spring Boot (Java 17, fat-jar) thông qua Amazon API Gateway (REST API) và AWS Step Functions điều phối quy trình phê duyệt. | 13/07/2026 | 13/07/2026 |
| 3 | Thiết kế kiến trúc hệ thống<br>- Phác thảo sơ đồ kiến trúc Cloud-Native Serverless tổng thể phân tách rõ ràng tầng Frontend (React 18 trên AWS Amplify), Backend (Lambda) và Database (Aurora MySQL trong VPC).<br>- Lập kế hoạch phân bổ công việc, quy trình Infrastructure as Code bằng AWS SAM và CI/CD tự động hóa qua GitHub Actions (bảo mật qua OIDC). | 14/07/2026 | 14/07/2026 |
| 4 | Nghiên cứu luồng xử lý sự kiện & Phê duyệt (Workflow)<br>- Thiết kế luồng xử lý phê duyệt tài liệu sử dụng AWS Step Functions với cơ chế `waitForTaskToken` (hỗ trợ quy trình nghiệp vụ chờ con người duyệt không giới hạn thời gian).<br>- Tích hợp Amazon SNS để tự động gửi email thông báo kết quả phê duyệt (approve/reject).<br>- Định nghĩa cơ chế phân quyền tài liệu chi tiết theo cấp độ (`OWNER`, `EDITOR`, `VIEWER`). | 15/07/2026 | 15/07/2026 |
| 5 | Hoàn thiện lộ trình, Tài liệu & Đánh giá<br>- Hoàn thiện chi tiết các tài liệu kỹ thuật cốt lõi (`EDMS-Serverless-Roadmap.md`, `EDMS-Master-Checklist.md`, và cấu trúc package Hexagonal Clean Architecture).<br>- Chuẩn bị tài liệu kỹ thuật và sẵn sàng cho buổi họp đánh giá kiến trúc hệ thống trước giai đoạn hiện thực hóa sản phẩm. | 16/07/2026 | 16/07/2026 |

### Kết quả đạt được tuần 4:
- Hoàn tất việc lựa chọn đề tài EDMS (Enterprise Document Collaboration Platform) và xây dựng kế hoạch triển khai chi tiết cho giai đoạn hiện thực hóa sản phẩm.
- Xây dựng thành công bản vẽ thiết kế hệ thống sử dụng các dịch vụ AWS tối ưu (API Gateway, Lambda, Aurora MySQL, S3, Cognito, Step Functions, SNS, Amplify), giúp hệ thống dễ dàng mở rộng tự động và tối ưu chi phí vận hành.
