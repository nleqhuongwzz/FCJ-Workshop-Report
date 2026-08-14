---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Quản lý tài liệu Serverless với EDMS - Nền tảng tài liệu

### Tổng quan

Workshop này hướng dẫn bạn toàn bộ quy trình xây dựng, phát triển và vận hành **EDMS (Enterprise Document Collaboration Platform)** - một ứng dụng quản lý và cộng tác tài liệu - trên nền tảng Amazon Web Services (AWS). Dự án áp dụng mô hình Cloud & DevOps hiện đại, với CI/CD pipeline hoàn toàn tự động và một kiến trúc serverless toàn diện.

Workshop được chia thành các giai đoạn chính:

- **Hạ tầng**: Thiết lập nền tảng dịch vụ AWS (S3, Aurora, IAM, Cognito)
- **Triển khai**: Xây dựng CI/CD pipeline bằng GitHub Actions và triển khai ứng dụng lên Lambda + API Gateway
- **Vận hành**: Cấu hình auto-scaling, giám sát, cảnh báo chi phí, và kiểm thử end-to-end
- **Hình ảnh minh họa**: Danh sách tham khảo tất cả ảnh chụp màn hình dùng trong workshop

### Tóm tắt kiến trúc

Hệ thống được tổ chức thành các lớp chính:

| Lớp | Thành phần |
|-----|-----------|
| CI/CD | GitHub Actions, OIDC, AWS STS, AWS SAM / CloudFormation |
| Trình bày | AWS Amplify (React Frontend) |
| Ứng dụng | Amazon API Gateway, AWS Lambda (Spring Boot) |
| Dữ liệu | Amazon Aurora MySQL, Amazon S3 |
| Workflow | AWS Step Functions, Amazon SNS |
| Giám sát | Amazon CloudWatch, AWS Budgets |

### Nội dung

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequisite/)
3. [Thiết kế và Xây dựng hạ tầng EDMS trên AWS](5.3-Edms-infrastructure/)
4. [Triển khai EDMS trên AWS](5.4-Edms-deployment/)
5. [Kiểm thử, Vận hành và Triển khai liên tục](5.5-Edms-operations/)
