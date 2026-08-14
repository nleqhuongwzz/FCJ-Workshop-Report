---
title: "Worklog Tuần 7"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7
- Thiết lập luồng triển khai CI/CD tự động hóa toàn bộ bằng GitHub Actions kết hợp AWS SAM và AWS Amplify.
- Triển khai frontend React 18 SPA lên AWS Amplify Hosting.
- Rà soát chính sách phân quyền IAM theo nguyên tắc least-privilege và tối ưu hóa bảo mật hệ thống.

### Các công việc cần triển khai trong tuần này
| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :---: | :--- | :---: | :---: |
| 1 | Thiết lập Luồng CI/CD (GitHub Actions)<br>- Cấu hình workflow tự động hóa để chạy unit test backend.<br>- Tích hợp OIDC assume role để xác thực an toàn với AWS mà không cần hard-code static AWS keys. | 03/08/2026 | 04/08/2026 |
| 2 | Triển khai Frontend lên AWS Amplify<br>- Xây dựng giao diện React 18 SPA tích hợp Amazon Cognito Identity SDK, React Router và Axios.<br>- Tự động build và deploy frontend lên AWS Amplify. | 05/08/2026 | 05/08/2026 |
| 3 | Tự động hóa Deploy Backend qua SAM<br>- Tích hợp bước gọi lệnh `sam deploy` vào pipeline CI/CD để tự động cập nhật bản Lambda monolith (Spring Boot fat-jar) lên môi trường Production. | 06/08/2026 | 07/08/2026 |
| 4 | Kiểm tra Kết nối VPC & Aurora MySQL<br>- Cấu hình Lambda chạy bên trong VPC cùng Security Group với cụm Aurora MySQL đảm bảo kết nối JDBC bảo mật tuyệt đối. | 08/08/2026 | 08/08/2026 |
| 5 | Kiểm thử Tổng thể Pipeline & Đánh giá<br>- Thực hiện test toàn bộ luồng CI/CD từ lúc commit code đến khi cập nhật thành công lên API Gateway endpoint. | 09/08/2026 | 09/08/2026 |

### Kết quả đạt được tuần 7
- Xây dựng thành công pipeline tự động hóa qua GitHub Actions, giúp mọi thay đổi về code backend và frontend được kiểm thử và phát hành lên AWS một cách liên tục, an toàn.
- Toàn bộ nền tảng EDMS đã chạy ổn định trên các dịch vụ AWS thực tế với endpoint API Gateway và giao diện Amplify hoàn chỉnh.
