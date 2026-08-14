---
title: "Worklog Tuần 1"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:
* Hiểu các khái niệm cốt lõi về Điện toán đám mây và cấu trúc hạ tầng toàn cầu của AWS.
* Thiết lập tài khoản AWS Free Tier an toàn, áp dụng các tiêu chuẩn bảo mật và kiểm soát chi phí.
* Làm quen với các dịch vụ nền tảng và vận hành tài nguyên qua AWS Management Console & CLI.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| 1 | - Nghiên cứu tổng quan về Cloud Computing và mô hình hạ tầng toàn cầu (Regions, Availability Zones) của AWS. <br> - Đăng ký tài khoản AWS Free Tier cá nhân. | 21/06/2026 | 21/06/2026 | AWS Documentation |
| 2 | - Kích hoạt xác thực đa yếu tố (MFA) cho tài khoản Root. <br> - Thiết lập tài khoản IAM Admin cho các tác vụ hàng ngày theo nguyên tắc quyền tối thiểu (Least Privilege). | 22/06/2026 | 22/06/2026 | AWS Documentation |
| 3 | - Nghiên cứu về AWS Billing & Cost Management. <br> - Thiết lập AWS Budgets để gửi cảnh báo email khi chi phí dự kiến chạm ngưỡng $5/tháng. | 23/06/2026 | 23/06/2026 | AWS Billing Guide |
| 4 | - Cài đặt và cấu hình AWS Command Line Interface (CLI) trên máy cục bộ bằng Access Keys/Secret Keys. <br> - Kiểm tra kết nối với AWS Cloud. | 24/06/2026 | 24/06/2026 | AWS CLI Documentation |
| 5 | - Đọc tài liệu và tổng hợp kiến thức về 3 nhóm dịch vụ nền tảng: Compute (Amazon EC2), Storage (Amazon S3) và Networking (Amazon VPC). | 25/06/2026 | 25/06/2026 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 1:

- Thiết lập thành công môi trường AWS an toàn. Tài khoản gốc đã được bảo vệ tuyệt đối và hệ thống cảnh báo ngân sách giúp ngăn ngừa hoàn toàn rủi ro phát sinh chi phí ngoài ý muốn trong quá trình học.
- Hoàn tất chuyển đổi thao tác từ tài khoản Root sang tài khoản IAM. Môi trường phát triển cục bộ đã kết nối thành công với AWS qua CLI, cho phép quản lý tài nguyên trực tiếp từ terminal.
- Hiểu rõ cách thức hoạt động của các dịch vụ cốt lõi, nắm bắt được sự tương quan giữa các nhóm dịch vụ Compute, Storage và Networking trong kiến trúc AWS.
