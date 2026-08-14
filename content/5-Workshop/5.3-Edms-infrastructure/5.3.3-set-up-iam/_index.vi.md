---
title : "Khởi tạo và Cấu hình IAM"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

IAM cung cấp các định danh và quyền cần thiết để EDMS hoạt động an toàn. Cần hai role:

1. **`github-actions-deploy-role`** — role **OIDC** (Web identity) mà GitHub Actions assume khi deploy, để **không** phải lưu AWS key dài hạn trong repository.
2. **`edms-lambda-role`** — role thực thi cho các hàm Lambda (S3, Cognito, SNS, Step Functions, CloudWatch Logs).

### 5.3.3.1 Tạo OIDC provider

Để GitHub Actions assume một role (thay vì giữ AWS key vĩnh viễn), hãy đăng ký OIDC provider của GitHub với IAM:

1. Mở **IAM console** → **Identity providers** → **Add provider**.


2. **Provider type:** chọn **OpenID Connect**.
3. **Provider URL:** nhập `https://token.actions.githubusercontent.com`.
4. IAM hiển thị **Get thumbprint** — bấm để lấy thumbprint chứng chỉ.
5. **Audience:** nhập `sts.amazonaws.com`.
6. Bấm **Add provider**.
![Figure 8. Thêm OIDC provider](/images/5-Workshop/5.3-Edms-infrastructure/oidc-provider.png)

> **Ghi chú:** Bước này chỉ thực hiện một lần cho mỗi tài khoản AWS. Provider URL phải khớp chính xác `https://token.actions.githubusercontent.com`.

### 5.3.3.2 Tạo deploy role cho GitHub Actions

1. Mở **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** chọn **Web identity**.
3. **Identity provider:** chọn OIDC provider GitHub vừa tạo.
4. **Audience:** chọn `sts.amazonaws.com`.
5. Bấm **Next**.
6. Thêm trust policy chỉ cho phép **repository của bạn** assume role (xem 5.4 để biết policy chính xác).
7. Trong **Permissions**, đính kèm policy deploy rộng bao gồm CloudFormation, Lambda, S3, API Gateway, IAM và các dịch vụ khác mà EDMS deploy.
8. Đặt tên role `github-actions-deploy-role`.
9. Bấm **Create role**.


> **Ghi chú:** Vì role được assume qua OIDC, workflow GitHub dùng `aws-actions/configure-aws-credentials` với `role-to-assume` — không bao giờ phải commit access key ID hay secret access key.

### 5.3.3.3 Tạo role thực thi Lambda

1. Mở **IAM** → **Roles** → **Create role**.
2. **Trusted entity type:** chọn **AWS service** → **Lambda**.
3. Bấm **Next**.
4. Đính kèm policy (tùy chỉnh hoặc managed) cho phép tối thiểu:
+ `s3:PutObject`, `s3:GetObject`, `s3:DeleteObject`, `s3:ListBucket`
+ `cognito-idp:InitiateAuth`, `cognito-idp:GetUser`
+ `sns:Publish`
+ `states:StartExecution`, `states:SendTaskSuccess`, `states:SendTaskFailure`
+ `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`
5. Đặt tên role `edms-lambda-role`.
6. Bấm **Create role**.


> **Thực hành tốt:** Chỉ cấp những quyền cần thiết cho mỗi role (**least-privilege**). Không bao giờ đặt AWS key trong code hoặc file cấu hình.

### 5.3.3.4 Xác minh các role

1. Mở **IAM** → **Roles**.
2. Xác nhận cả `github-actions-deploy-role` và `edms-lambda-role` đều xuất hiện.
3. Mở từng role và kiểm tra các tab **Trust relationships** và **Permissions** khớp với cấu hình bạn đã đặt.
4. Ghi lại **role ARN** của deploy role — workflow GitHub và SAM template sẽ tham chiếu tới nó.
