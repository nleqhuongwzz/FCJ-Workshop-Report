---
title: "Worklog Tuần 6"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6
- Phát triển và hoàn thiện các module nghiệp vụ nâng cao (Phê duyệt, Chia sẻ link, Thùng rác).
- Tự động hóa quy trình phê duyệt tài liệu bằng AWS Step Functions kết hợp Amazon SNS gửi email thông báo.
- Đóng gói và triển khai toàn bộ hệ thống Serverless bằng Infrastructure as Code sử dụng AWS SAM.

### Các công việc cần triển khai trong tuần này
| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :---: | :--- | :---: | :---: |
| 1 | Phát triển Module & Quy trình Phê duyệt (Step Functions)<br>- Xây dựng State Machine DocumentApprovalStateMachine trên AWS Step Functions theo pattern human approval với waitForTaskToken.<br>- Lập trình luồng submit, capture token, choice (approved/rejected), mark status và notify. | 27/07/2026 | 27/07/2026 |
| 2 | Thông báo Sự kiện (Amazon SNS)<br>- Tích hợp Amazon SNS topic edms-notifications để kích hoạt gửi email thông báo tự động khi tài liệu hoàn tất quy trình duyệt (approve/reject). | 28/07/2026 | 28/07/2026 |
| 3 | Chia sẻ Tài liệu An toàn & Quản lý Vòng đời<br>- Lập trình chức năng chia sẻ tài liệu bằng link có kiểm soát thời gian.<br>- Triển khai các tính năng phụ trợ như tìm kiếm, OCR trích xuất văn bản và quản lý danh mục thư mục, phòng ban. | 29/07/2026 | 30/07/2026 |
| 4 | Dashboard Thống kê & Audit Log<br>- Phát triển module Dashboard thống kê tổng quan theo phòng ban phục vụ công tác quản lý của doanh nghiệp.<br>- Xây dựng hệ thống ghi nhận nhật ký thao tác (audit log) qua CloudWatch. | 31/07/2026 | 31/07/2026 |
| 5 | Hạ tầng bằng Code & Triển khai Đám mây (AWS SAM)<br>- Cấu hình file `template.yaml` bằng AWS SAM để khai báo toàn bộ tài nguyên (Lambda, API Gateway, Aurora, S3, Cognito, Step Functions, SNS).<br>- Thực thi build và deploy cấu hình hạ tầng lên AWS Cloud. | 01/08/2026 | 02/08/2026 |

### Kết quả đạt được tuần 6
- Vận hành thành công quy trình phê duyệt bất đồng bộ qua Step Functions và SNS, giúp xử lý các tác vụ chờ con người mà không bị giới hạn timeout 15 phút của Lambda.
- Toàn bộ kiến trúc hạ tầng được định nghĩa bằng mã nguồn, cho phép khởi tạo hoặc quản lý đồng bộ toàn bộ hệ thống EDMS trên AWS một cách nhanh chóng.
