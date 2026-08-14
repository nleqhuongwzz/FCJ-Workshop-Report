---
title : "Khai báo Secrets và Variables trên GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

CI/CD pipeline cần các giá trị cấu hình (ARN, ID, credentials). Lưu chúng dưới dạng **GitHub Secrets** để không bao giờ đưa vào repository.

### 5.4.5.1 Các secrets cần thiết

Workflow ở 5.4.2 đọc các secret sau tại thời điểm deploy. Thêm chúng vào **GitHub** → **Settings** → **Secrets and variables** → **Actions**:

| Secret | Giá trị |
|--------|---------|
| `AWS_DEPLOY_ROLE_ARN` | Deploy role ARN từ 5.4.4 |
| `COGNITO_USER_POOL_ID` | Cognito User Pool ID từ 5.3.4 |
| `COGNITO_CLIENT_ID` | Cognito Client ID từ 5.3.4 |
| `AURORA_ENDPOINT` | Aurora endpoint từ 5.3.2 |
| `DB_USER_AWS` | Username master Aurora |
| `DB_PASS_AWS` | Mật khẩu master Aurora |
| `AWS_S3_BUCKET` | Tên bucket S3 từ 5.3.1 |
| `SNS_TOPIC_ARN` | SNS topic ARN (tạo ở 5.4.8) |
| `BACKEND_LAMBDA_ARN` | ARN hàm Lambda (tạo sau deploy ở 5.4.9) |

![Figure 19. GitHub secrets](/images/5-Workshop/5.4-Edms-deployment/secrets.png)

### 5.4.5.2 Thêm một repository secret

1. Mở repository của bạn trên GitHub → **Settings** → **Secrets and variables** → **Actions**.
2. Bấm **New repository secret**.
3. Nhập **Name** (ví dụ `AWS_DEPLOY_ROLE_ARN`) và **Value**.
4. Bấm **Add secret**.

Lặp lại cho từng dòng trong bảng trên.

> **Ghi chú:** Hai secret cuối (`SNS_TOPIC_ARN`, `BACKEND_LAMBDA_ARN`) phụ thuộc vào tài nguyên bạn tạo sau. Bạn có thể thêm các secret khác ngay bây giờ và thêm hai secret cuối sau mục 5.4.8 và 5.4.9.

### 5.4.5.3 Xác minh secret đã lưu

1. Sau khi thêm, secret xuất hiện trong danh sách **Actions secrets and variables**, nhưng giá trị bị **masked**.
2. Bạn không thể đọc lại giá trị — điều này là cố ý. Muốn đổi thì xóa và tạo lại.

> **Best practice:** Không bao giờ commit secret hoặc file `.env`. Tất cả secret được bơm vào tại thời điểm deploy bởi workflow, và GitHub tự động che giá trị của chúng trong log run.
