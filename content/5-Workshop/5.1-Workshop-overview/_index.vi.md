---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

### 5.1.1 Thứ bạn sẽ xây dựng

**EDMS** là nền tảng cộng tác tài liệu cho phép người dùng tải lên tài liệu, tổ chức vào thư mục, quản lý phiên bản, chia sẻ với quyền kiểm soát, và đưa vào quy trình phê duyệt. Hệ thống hoàn toàn serverless: chỉ trả tiền theo mức sử dụng thực tế, tự động scale, không phải quản lý server.

Hệ thống có ba vai trò tài khoản:

- **ADMIN** — quản lý người dùng, phòng ban và toàn hệ thống.
- **MANAGER** — phê duyệt / từ chối tài liệu.
- **USER** — tạo, tải lên, chỉnh sửa và nộp tài liệu.

### 5.1.2 Kiến trúc

Sơ đồ dưới đây mô tả kiến trúc nền tảng chúng ta sẽ xây dựng:

{{% mermaid %}}
flowchart LR
    subgraph Client["Client"]
        U[User / Browser]
        A[Amplify - React Frontend]
    end

    subgraph AWS["AWS Cloud"]
        G[API Gateway]
        L[Lambda - Spring Boot]
        DB[(Aurora MySQL)]
        S3[(S3 - Documents)]
        C[Cognito - Auth]
        SF[Step Functions - Approval]
        SNS[SNS - Notification]
        CW[CloudWatch]
    end

    U -->|sign-in| C
    U --> A
    A -->|HTTPS + JWT| G
    G --> L
    L -->|validate token| C
    L <-->|read / write| DB
    L <-->|store / get files| S3
    L -->|submit for approval| SF
    SF -->|notify| SNS
    L -.->|logs / metrics| CW
{{% /mermaid %}}

Hệ thống bao gồm các dịch vụ sau:

| Lớp | AWS Service | Trách nhiệm |
|-----|-------------|-------------|
| Frontend | **AWS Amplify** | Phục vụ React SPA qua HTTPS |
| API | **Amazon API Gateway** | REST endpoint chuyển tiếp request đến Lambda |
| Compute | **AWS Lambda** | Chạy backend monolith Spring Boot (Java 17) |
| Cơ sở dữ liệu | **Amazon Aurora MySQL** | Lưu toàn bộ metadata quan hệ |
| Lưu trữ | **Amazon S3** | Lưu các file tài liệu gốc |
| Xác thực | **Amazon Cognito** | Đăng nhập và phân quyền theo vai trò |
| Workflow | **AWS Step Functions** | Điều phối quy trình phê duyệt tài liệu |
| Thông báo | **Amazon SNS** | Gửi email khi duyệt / từ chối |
| Giám sát | **Amazon CloudWatch** | Log và metric |
| CI/CD | **GitHub Actions + AWS SAM** | Build, test, deploy hạ tầng as code |

### 5.1.3 Các dịch vụ phối hợp như thế nào

1. Người dùng đăng nhập với **Cognito**, nhận **JWT token**.
2. Frontend React gọi **API Gateway** kèm token.
3. **API Gateway** chuyển tiếp request đến **Lambda**, Lambda xác thực token.
4. **Lambda** đọc / ghi metadata trong **Aurora** và lưu file trong **S3**.
5. Khi tài liệu được nộp để phê duyệt, **Lambda** khởi động một execution của **Step Functions**.
6. **Step Functions** điều phối phê duyệt và gửi thông báo qua **SNS** (email).

> **Ghi chú:** Ở các phần tiếp theo, chúng ta sẽ xây dựng từng dịch vụ một, theo đúng thứ tự này.
