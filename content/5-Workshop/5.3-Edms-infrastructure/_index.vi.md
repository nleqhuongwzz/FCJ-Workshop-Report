---
title : "Thiết kế và Xây dựng hạ tầng EDMS trên AWS"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Trong phần này, bạn sẽ thiết kế và xây dựng **hạ tầng lõi** của EDMS trên AWS: lưu trữ, cơ sở dữ liệu, IAM, và xác thực.

#### Kiến trúc triển khai

Hạ tầng EDMS tuân theo mô hình phân lớp, serverless:

- **Amazon S3** — lưu các file tài liệu gốc
- **Amazon Aurora (MySQL)** — chứa metadata quan hệ (users, documents, permissions, approval history)
- **IAM Role + OIDC** — cấp quyền deploy an toàn cho GitHub Actions và role thực thi của Lambda
- **Amazon Cognito** — cung cấp xác thực và phân quyền theo vai trò

#### Nội dung

1. [Khởi tạo và Cấu hình S3](5.3.1-set-up-s3/)
2. [Khởi tạo và Cấu hình Aurora RDS](5.3.2-set-up-rds/)
3. [Khởi tạo và Cấu hình IAM](5.3.3-set-up-iam/)
4. [Khởi tạo và Cấu hình Cognito](5.3.4-set-up-cognito/)
