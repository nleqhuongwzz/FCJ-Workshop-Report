---
title: "Worklog Tuần 2"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:
* Chốt phương án kiến trúc (Architecture) và cấu trúc cơ sở dữ liệu (Database Schema) cho dự án EDMS.
* Nghiên cứu chuyên sâu về Serverless Stack (API Gateway, Lambda, S3) và triển khai Demo Proof of Concept (PoC).
* Xây dựng bộ yêu cầu nghiệp vụ chi tiết thông qua User Stories.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **2** | - Họp nhóm: Thống nhất quy trình nghiệp vụ (Business Logic), quyền hạn OWNER/EDITOR/VIEWER. | 06/07/2026 | 06/07/2026 | Internal Meeting |
| **3** | - Thiết kế Architecture & Database Schema: Định nghĩa 8 thực thể (Users, Documents, Versions, Permissions, Files, Tags, OCR, AuditLogs). <br> - Quy hoạch GSI cho DynamoDB. | 07/07/2026 | 07/07/2026 | System Design Docs |
| **4** | - Nghiên cứu chuyên sâu: API Gateway (REST API), Lambda (Node.js). <br> - Phân tích User Stories: Soạn thảo Rich-text, Rollback phiên bản và cơ chế bảo mật Presigned URL. | 08/07/2026 | 08/07/2026 | AWS Documentation |
| **5** | - Phát triển Demo nhỏ (PoC): Tích hợp Lambda, S3 để thực hiện upload file. <br> - Xử lý JSON logic cho trình soạn thảo. | 09/07/2026 | 09/07/2026 | VS Code / AWS CLI |
| **6** | - Debug: Khắc phục lỗi CORS & IAM Permissions. <br> - Tổng kết tài liệu báo cáo tuần 2. | 10/07/2026 | 10/07/2026 | Team Review |

### Kết quả đạt được tuần 2:

* **Tư duy kiến trúc hệ thống:**
    * Hoàn thiện sơ đồ nghiệp vụ và cấu trúc dữ liệu cho dự án EDMS. 
    * Thiết kế 8 thực thể (Entity) cốt lõi trên DynamoDB với chiến lược GSI tối ưu (như `GSI_OwnerDocuments`, `GSI_UserPermissions`) đảm bảo truy vấn nhanh theo nhiều chiều.
* **Chiến lược vận hành & Bảo mật:**
    * Thiết lập thành công cơ chế upload bảo mật thông qua **Presigned URL** (hạn chế thời gian sống 5-10 phút).
    * Áp dụng tính năng **Time-To-Live (TTL)** trên bảng Documents để tự động thực thi Hard Delete sau 30 ngày, tối ưu chi phí lưu trữ doanh nghiệp.
* **Thực hành triển khai (Demo PoC):**
    * Viết hàm Lambda bằng Node.js xử lý logic CRUD cho tài liệu và phiên bản (Versioning Control).
    * Đã giải quyết thành công các thách thức về **CORS policy** giữa Frontend và AWS API Gateway.
    * Xây dựng luồng xử lý bất đồng bộ cho tác vụ OCR (dự kiến tích hợp Amazon Textract).
* **Quản trị nhóm:**
    * Hệ thống hóa toàn bộ yêu cầu qua **User Stories** (theo mô hình AS/I/SO), giúp các thành viên nắm rõ tiêu chí nghiệm thu (Acceptance Criteria) cho từng module từ Soạn thảo đến Audit Trail.
    * Đã chuẩn bị sẵn sàng nền tảng để triển khai Infrastructure as Code (IaC) trong tuần tới.