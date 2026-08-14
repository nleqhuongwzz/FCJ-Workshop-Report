---
title: "Worklog Tuần 8"
date: 2026-08-11
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8
- Thực hiện kiểm thử và rà soát toàn diện lại toàn bộ các tính năng của hệ thống EDMS trên môi trường thực tế.
- Hoàn thiện hồ sơ tài liệu kỹ thuật và báo cáo tổng kết đồ án.
- Tiến hành báo cáo nghiệm thu sản phẩm và rà soát bảo mật mã nguồn (đảm bảo không lộ thông tin nhạy cảm trong file `.env` hay Git).

### Các công việc cần triển khai trong tuần này
| Ngày | Nội dung công việc chuyên môn | Thời gian bắt đầu | Thời gian hoàn thành |
| :---: | :--- | :---: | :---: |
| 1 | - Tiến hành chạy lại toàn bộ các kịch bản test (Unit test, Integration test và Postman collection), kiểm tra độ ổn định của các luồng API, Cognito auth và Step Functions workflow. | 10/08/2026 | 10/08/2026 |
| 2 | - Khắc phục các vấn đề phát sinh, tinh chỉnh cấu hình Lambda và kiểm tra log trên Amazon CloudWatch để đảm bảo hiệu năng phản hồi tối ưu. | 11/08/2026 | 11/08/2026 |
| 3 | - Tổng hợp kết quả thực hiện, hoàn thiện hồ sơ tài liệu chi tiết của đồ án. | 12/08/2026 | 12/08/2026 |
| 4 | - Hoàn thiện các tài liệu báo cáo thực tập, cấu hình file demo. | 13/08/2026 | 14/08/2026 |
| 5 | - Hoàn tất việc bàn giao mã nguồn sạch (bảo mật tuyệt đối các file `.env` và GitHub secrets) và kết thúc quy trình phát triển sản phẩm. | 14/08/2026 | 15/08/2026 |

### Kết quả đạt được tuần 8
- Hệ thống EDMS đã vận hành ổn định, đáp ứng trọn vẹn các mục tiêu ban đầu, đảm bảo tính năng xác thực Cognito, phân quyền tài liệu chặt chẽ và quy trình phê duyệt tự động qua Step Functions.
- Hoàn thành toàn diện chu trình phát triển phần mềm theo mô hình Serverless trên AWS, từ khâu thiết kế kiến trúc, hiện thực hóa mã nguồn Java/React, thiết lập CI/CD cho đến bước báo cáo nghiệm thu thành công.
