---
title: "Worklog Tuần 5"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:
* Thiết kế và triển khai kiến trúc cơ sở dữ liệu đa mô hình (Polyglot Persistence) sử dụng Amazon Aurora và DynamoDB.
* Cấu hình kho lưu trữ đối tượng an toàn cho tệp vật lý và thiết lập hệ thống xác thực người dùng tập trung.
* Phát triển các API quản lý tài liệu cốt lõi (CRUD) bằng Java 17 và AWS Lambda.
* Triển khai kiểm soát phiên bản, phân quyền truy cập (RBAC) và tối ưu hóa hiệu năng.

### Các công việc đã thực hiện:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :--- | :--- | :--- | :--- |
| **1** | **Thiết kế Lược đồ Cơ sở dữ liệu**<br>- Thiết kế lược đồ đa mô hình: Aurora Serverless v2 (MySQL) cho dữ liệu quan hệ (Tài liệu, Phiên bản, Nhãn) và DynamoDB cho Nhật ký hệ thống (AuditLogs). | 19/07/2026 | 19/07/2026 |
| **2** | **Quản lý Danh tính với Cognito & Lưu trữ S3**<br>- Tích hợp Amazon Cognito User Pool để xử lý đăng nhập, xác thực và phân nhóm người dùng (VD: HR, SALES) để phân quyền.<br>- Thiết lập S3 bucket để lưu trữ tệp vật lý bằng Presigned URLs với thời gian sống ngắn (5-10 phút) để tránh lạm dụng băng thông. | 20/07/2026 | 20/07/2026 |
| **3** | **Phát triển API Cốt lõi & Lambda Layers**<br>- Lập trình các hàm Lambda bằng Java 17 (Amazon Corretto) để xử lý việc tạo và truy xuất tài liệu.<br>- Trích xuất các thư viện dùng chung thành AWS Lambda Layers để giảm kích thước gói triển khai. | 21/07/2026 | 21/07/2026 |
| **4** | **Quản lý Phiên bản & Phân quyền (RBAC)**<br>- Triển khai logic kiểm soát phiên bản, tự động sinh số phiên bản mới khi có chỉnh sửa và tính năng Rollback để khôi phục.<br>- Hoàn thiện module Phân quyền để thiết lập ranh giới truy cập nghiêm ngặt cho Chủ sở hữu, Người chỉnh sửa và Người xem ngay tại tầng API. | 22/07/2026 | 22/07/2026 |
| **5** | **Khắc phục Khởi động lạnh (SnapStart)**<br>- Giải quyết tình trạng khởi động chậm của Java 17 (1-3s) bằng cách bật AWS Lambda SnapStart, chụp sẵn bộ nhớ JVM để tăng tốc độ phản hồi. | 23/07/2026 | 23/07/2026 |

### Kết quả đạt được:

* **Kiến trúc Cơ sở dữ liệu Mở rộng & Bảo mật:** Tách biệt thành công luồng ghi log sang DynamoDB để giải quyết thắt cổ chai hiệu năng, đồng thời thiết lập vành đai bảo mật vững chắc kết hợp giữa Cognito và S3 Presigned URLs.
* **Logic Backend Vững chắc & Hiệu năng Cao:** Triển khai thành công API backend đầy đủ chức năng, bảo mật, kiểm soát phiên bản bằng Java 17, đồng thời cải thiện đáng kể thời gian phản hồi nhờ tận dụng Lambda Layers và SnapStart.
