---
title : "Chuẩn bị Mã nguồn và Cấu trúc Project"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

EDMS được lưu trong một Git repository gồm **backend** (monolith Spring Boot đóng gói thành một Lambda), **frontend** (React), và **CI/CD** workflow. Trong phần này bạn clone mã nguồn, xem cấu trúc thư mục, và xác minh backend biên dịch được ở local.

### 5.4.1.1 Clone repository

Mở terminal và clone fork của bạn về:

```bash
git clone https://github.com/<account-cua-ban>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

Thay `<account-cua-ban>` bằng username GitHub của bạn. Xác nhận clone thành công bằng cách liệt kê file ở thư mục gốc:

```bash
ls -la
```

### 5.4.1.2 Xem cấu trúc project

Bố cục repository được sắp xếp để backend, frontend, và công cụ triển khai nằm cạnh nhau:

```
Enterprise-Document-Collaboration-Platform/
├── backend/
│   ├── pom.xml                    # Maven build (Java 17)
│   ├── template.yaml              # AWS SAM - infrastructure as code
│   ├── src/main/java/com/edms/    # Backend Spring Boot
│   └── src/main/resources/        # config + Flyway migrations + seed data
├── frontend/
│   └── src/                       # React 18 SPA
├── .github/workflows/deploy.yml   # CI/CD pipeline
└── .env                           # config local-only (gitignored)
```


Các file quan trọng cần nhớ:

+ `backend/pom.xml` — khai báo Maven build cho **Java 17** và tạo ra fat jar.
+ `backend/template.yaml` — template **AWS SAM** định nghĩa toàn bộ tài nguyên AWS (Lambda, API Gateway, Step Functions, SNS).
+ `.github/workflows/deploy.yml` — **CI/CD pipeline** chạy trên GitHub Actions.
+ `.env` — cấu hình local; file này được **gitignore** và không bao giờ đẩy lên.

### 5.4.1.3 Build fat jar backend ở local

Xác minh backend biên dịch được trước khi đụng vào AWS. Từ thư mục gốc:

```bash
cd backend
mvn clean package -DskipTests
```

Lệnh này tạo một **fat jar** duy nhất ở `backend/target/`:

```
backend/target/backend-java-1.0.0-SNAPSHOT.jar
```

Tên jar rất quan trọng: đây là artifact mà AWS SAM đóng gói và deploy thành một hàm Lambda.

### 5.4.1.4 Hiểu cách monolith ánh xạ sang Lambda

Backend là một **monolith**: một ứng dụng Spring Boot chứa toàn bộ REST controllers, services, repositories, và logic xử lý phê duyệt. Nó không được tách thành nhiều hàm nhỏ. Thay vào đó, toàn bộ ứng dụng được bọc bởi một Lambda **handler** duy nhất (`StreamLambdaHandler` hoặc `RequestHandler`) mà AWS SAM gắn vào API Gateway.

> **Ghi chú:** Đánh đổi "serverless monolith" này là chủ ý cho workshop. Nó giữ cho việc deploy đơn giản (một Lambda) trong khi vẫn có một nguồn sự thật duy nhất cho backend. `template.yaml` định nghĩa cách jar được chuyển thành một hàm Lambda.

