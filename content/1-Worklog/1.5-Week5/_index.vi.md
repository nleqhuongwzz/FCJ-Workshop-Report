---
title: "Worklog Tuần 5"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5
- Thiết kế và triển khai kiến trúc cơ sở dữ liệu quan hệ Aurora MySQL kết hợp lưu trữ đối tượng an toàn trên S3.
- Cấu hình xác thực người dùng tập trung với Amazon Cognito User Pool và phân quyền nhóm (ADMIN/MANAGER/USER).
- Phát triển các API quản lý tài liệu và thư mục cốt lõi (CRUD) bằng Java 17 (Spring Boot monolith) trên AWS Lambda.
- Triển khai kiểm soát phiên bản tài liệu, tính năng rollback và phân quyền truy cập chi tiết (OWNER, EDITOR, VIEWER).

### Các công việc cần triển khai trong tuần này:
| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :---: | :--- | :---: | :---: |
| 1 | Thiết kế Lược đồ Cơ sở dữ liệu Aurora MySQL<br>- Thiết kế schema chuẩn hóa lưu trữ metadata quan hệ: Users, Departments, Documents, Versions, Folders, Permissions, Tags, Shares, ApprovalHistory, AuditLog, OcrResult (sử dụng Spring Data JPA / Hibernate và Flyway migration). | 19/07/2026 | 19/07/2026 |
| 2 | Quản lý Danh tính với Cognito & Lưu trữ S3<br>- Tích hợp Amazon Cognito User Pool để xử lý đăng nhập, xác thực JWT và phân nhóm người dùng (ADMIN/MANAGER/USER).<br>- Thiết lập S3 bucket để lưu trữ tệp vật lý bằng Pre-signed URLs an toàn, không lộ credentials. | 20/07/2026 | 20/07/2026 |
| 3 | Phát triển API Cốt lõi & Hexagonal Architecture<br>- Lập trình backend Spring Boot (Java 17, fat-jar) theo cấu trúc Hexagonal (Ports & Adapters) với các package controller, service, domain, và infrastructure.<br>- Tích hợp StreamLambdaHandler để xử lý sự kiện từ Amazon API Gateway và Step Functions. | 21/07/2026 | 21/07/2026 |
| 4 | Kiểm soát Phiên bản & Phân quyền (RBAC)<br>- Triển khai logic quản lý phiên bản tài liệu (version + rollback) và gắn thẻ (tags).<br>- Hoàn thiện module phân quyền tài liệu theo cấp độ (OWNER, EDITOR, VIEWER) kết hợp cơ chế bảo mật API theo role của Cognito. | 22/07/2026 | 22/07/2026 |
| 5 | Kiểm thử Unit Test & Hoàn thiện Module<br>- Viết unit test và integration test với JUnit 5 + Mockito (MVC Test) đảm bảo các luồng API hoạt động ổn định. | 23/07/2026 | 23/07/2026 |

### Kết quả đạt được tuần 5:
- Hoàn thiện schema Aurora MySQL chuẩn hóa toàn bộ metadata và thiết lập vành đai bảo mật chặt chẽ giữa Cognito JWT cùng S3 Pre-signed URLs.
- Xây dựng thành công hệ thống Spring Boot monolith chạy trên AWS Lambda, đáp ứng đầy đủ các tiêu chuẩn kiến trúc Hexagonal, kiểm soát phiên bản và phân quyền chi tiết.
