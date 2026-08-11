---
title: "Worklog Tuần 7"
date: 2026-08-14
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:
* Thiết lập luồng triển khai CI/CD tự động hóa toàn bộ bằng GitHub Actions.
* Triển khai tính năng tải ảnh lên S3 và lưu thông tin vào cơ sở dữ liệu Aurora.
* Triển khai các lớp bảo mật nâng cao (AWS WAF, Secrets Manager) và rà soát chính sách phân quyền IAM.

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :--- | :--- | :--- | :--- |
| **1** | **Thiết lập Luồng CI/CD (GitHub Actions)**<br>- Cấu hình quy trình tự động hóa kiểm thử và triển khai hạ tầng khi có mã nguồn mới được hợp nhất. | 03/08/2026 | 04/08/2026 |
| **2** | **Bảo mật CSDL (Secrets Manager)**<br>- Tích hợp AWS Secrets Manager để lưu trữ an toàn và tự động xoay vòng mật khẩu của cơ sở dữ liệu Aurora. | 05/08/2026 | 05/08/2026 |
| **3** | **Bảo vệ Tầng biên (AWS WAF) & Rà soát IAM**<br>- Cấu hình tường lửa AWS WAF để chặn bot spam và áp dụng giới hạn tần suất gọi API.<br>- Tiến hành rà soát toàn bộ AWS IAM Policies, đảm bảo nguyên tắc Quyền tối thiểu. | 06/08/2026 | 07/08/2026 |
| **4** | **Triển khai Tải ảnh lên S3 & Lưu Aurora**<br>- Phát triển API xử lý việc tải ảnh lên Amazon S3.<br>- Cấu hình Lambda để ghi nhận metadata và đường dẫn tệp tin vào cơ sở dữ liệu Aurora Serverless. | 08/08/2026 | 08/08/2026 |
| **5** | **Kiểm thử Tổng thể & Đánh giá**<br>- Thực hiện kiểm thử toàn bộ hệ thống, rà soát log lỗi trên CloudWatch và hoàn thiện báo cáo tuần. | 09/08/2026 | 09/08/2026 |

### Kết quả đạt được tuần 7:

* **Tự động hóa Quy trình:** Xây dựng thành công hệ thống CI/CD giúp tự động hóa quá trình build và deploy, tối ưu hóa thời gian phát triển và vận hành.
* **Hoàn thiện Chức năng Lưu trữ:** Triển khai thành công luồng xử lý tải ảnh lên S3 kết hợp lưu trữ siêu dữ liệu an toàn vào cơ sở dữ liệu Aurora.
* **Bảo mật Doanh nghiệp:** Củng cố toàn diện lớp bảo mật cho hệ thống với AWS Secrets Manager quản lý thông tin nhạy cảm và AWS WAF bảo vệ ứng dụng khỏi lưu lượng độc hại.
