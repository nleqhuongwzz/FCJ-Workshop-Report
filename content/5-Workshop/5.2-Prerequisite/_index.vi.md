---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Trước khi bắt đầu workshop, hãy chuẩn bị những điều sau.

### 5.2.1 Tài khoản AWS

Bạn cần một **tài khoản AWS** với một user có quyền tạo các tài nguyên dùng trong workshop. Khuyến nghị sử dụng region **ap-southeast-1 (Singapore)**.

> **Ghi chú:** Để giữ chi phí ở mức thấp nhất, hãy dọn dẹp toàn bộ tài nguyên cuối workshop (xem mục 5.5.7). Aurora tính phí ngay cả khi không sử dụng, nên hãy stop hoặc xóa cluster khi xong.

### 5.2.2 Quyền IAM

Đảm bảo user của bạn có quyền làm việc với các dịch vụ trong workshop. Danh sách đầy đủ tối thiểu:

```
s3:CreateBucket, s3:PutObject, s3:GetObject, s3:ListBucket
cognito-idp:CreateUserPool, cognito-idp:CreateUserPoolClient
lambda:CreateFunction, lambda:UpdateFunctionCode, lambda:InvokeFunction
apigateway:CreateRestApi, apigateway:CreateDeployment
rds:CreateDBCluster, rds:CreateDBInstance
states:CreateStateMachine, states:StartExecution
sns:CreateTopic, sns:Subscribe, sns:Publish
iam:CreateRole, iam:CreatePolicy, iam:AttachRolePolicy, iam:PassRole
cloudformation:CreateStack, cloudformation:UpdateStack, cloudformation:DeleteStack
amplify:CreateApp, amplify:CreateBranch
```


> **Thực hành tốt:** **Không** dùng tài khoản root AWS. Tạo IAM user riêng và chỉ cấp những quyền cần thiết (least-privilege).

### 5.2.3 Công cụ

Backend EDMS là project Spring Boot (Java 17) deploy qua **AWS SAM** và **GitHub Actions**. Với workshop này bạn cần:

- **JDK 17** (khuyến nghị Amazon Corretto 17).
- **Maven 3.8+**.
- **AWS SAM CLI** ≥ 1.100.
- **AWS CLI v2**.
- **Node.js 18+** (cho frontend React).
- **VS Code** (IDE khuyến nghị).
- **Git** + tài khoản **GitHub**.


### 5.2.4 Mã nguồn

Clone repository của project:

```bash
git clone https://github.com/<account-cua-ban>/Enterprise-Document-Collaboration-Platform.git
cd Enterprise-Document-Collaboration-Platform
```

Repository chứa:

```
backend/       Backend Spring Boot (Java 17), SAM template.yaml
frontend/      React 18 SPA
.github/       GitHub Actions CI/CD workflow
```

