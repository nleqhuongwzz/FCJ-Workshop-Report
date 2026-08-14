---
title : "Dọn dẹp Tài nguyên"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

Để tránh chi phí phát sinh, hãy xóa mọi tài nguyên bạn đã tạo trong workshop này. Làm theo thứ tự bên dưới để các tài nguyên phụ thuộc được gỡ trước — xóa backend stack trước, rồi đến các dịch vụ tạo thủ công, cuối cùng xác minh không còn gì chạy.

### 5.5.7.1 Xóa SAM / CloudFormation stack

Các tài nguyên Lambda, API Gateway, Step Functions, và IAM do SAM tạo đều được xóa khi xóa CloudFormation stack. Có hai cách:

**Cách A — AWS CLI (nhanh nhất):**

Mở terminal trong thư mục chứa `template.yaml` của bạn và chạy:

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

Lệnh này:
+ Xóa **CloudFormation stack** (`edms-lambda-stack`).
+ Xóa **Lambda function** và code của nó.
+ Xóa **REST API** của API Gateway.
+ Xóa **state machine** Step Functions.
+ Xóa các **IAM roles** do SAM tạo.
+ Xóa các **artifacts** (code bundles) trong SAM deployment bucket.

**Cách B — AWS Console:**

1. Mở **CloudFormation console**.
2. Chọn stack **`edms-lambda-stack`**.
3. Bấm **Delete**.
4. AWS yêu cầu xác nhận — bấm **Delete stack**.
5. Đợi trạng thái stack đổi từ `DELETE_IN_PROGRESS` sang **`DELETE_COMPLETE`**.

> **Ghi chú:** Nếu stack không xóa được (ví dụ S3 bucket chưa trống), hãy xem tab **Events** để biết lỗi, gỡ tài nguyên đang chặn, rồi thử lại.

### 5.5.7.2 Xóa các tài nguyên còn lại thủ công

Các dịch vụ sau được tạo **ngoài SAM**, nên bạn phải xóa từng cái trong console. Làm theo thứ tự này:

**1. Xóa S3 bucket (làm trước Cognito/Aurora vì nó chứa file)**

1. Mở **S3 console**.
2. Chọn bucket `edms-docs-bucket-...`.
3. Bấm **Empty** → gõ `permanently delete` → xác nhận. Thao tác này xóa mọi object **và** mọi object version (nếu không bucket không thể xóa).
4. Quay lại danh sách bucket, chọn bucket → **Delete** → gõ tên bucket → xác nhận.

**2. Xóa Aurora cluster (nguồn chi phí chính)**

1. Mở **RDS console** → **Databases**.
2. Chọn cluster **`edms-cluster`**.
3. Bấm **Actions** → **Delete**.
4. Bỏ tick **Create final snapshot** (trừ khi bạn muốn lưu backup).
5. **Quan trọng:** đánh dấu **"I acknowledge that I will lose..."** và **tắt delete protection** nếu đang bật (bạn đã bật ở 5.3.2).
6. Bấm **Delete**. Đợi trạng thái chuyển thành `DELETED`.

> **Ghi chú:** Aurora tính phí ngay cả khi stop, nên điều quan trọng là **xóa hẳn** (không chỉ stop) cluster khi bạn đã xong.

**3. Xóa Cognito User Pool**

1. Mở **Cognito console** → **User pools**.
2. Chọn **`edms-user-pool`**.
3. Bấm **Delete**.
4. Xác nhận — thao tác này xóa pool, app client, và toàn bộ người dùng.

**4. Xóa Step Functions state machine**

1. Mở **Step Functions console** → **State machines**.
2. Chọn **`DocumentApprovalStateMachine`**.
3. Bấm **Delete** → xác nhận. (Mọi execution đang chạy sẽ bị hủy.)

**5. Xóa SNS topic**

1. Mở **SNS console** → **Topics**.
2. Chọn **`edms-notifications`**.
3. Bấm **Delete** → xác nhận. Thao tác này cũng xóa các subscription (email subscription).

**6. Xóa AWS Amplify app (frontend)**

1. Mở **Amplify console** → **All apps**.
2. Chọn app.
3. Bấm **Actions** → **Delete app**.
4. Gõ `delete` để xác nhận. Thao tác này xóa frontend đã deploy và URL domain Amplify.

**7. Xóa các IAM roles**

1. Mở **IAM console** → **Roles**.
2. Xóa **`github-actions-deploy-role`** và **`edms-lambda-role`** (sau khi CloudFormation stack đã gỡ, chúng không còn cần nữa).
3. Cũng xóa **OIDC provider** (`token.actions.githubusercontent.com`) trong **IAM → Identity providers** nếu bạn không còn cần.

### 5.5.7.3 Xác minh chi phí về 0

1. Mở **Billing** → **Cost Explorer**.
2. Xác nhận không còn dịch vụ nào đang tính phí cho tài khoản của bạn.
3. (Tùy chọn) Đặt cảnh báo ngân sách **$0** để được email thông báo nếu còn bất kỳ chi phí nào phát sinh.

> **Thực hành tốt:** Sau khi dọn dẹp, xác nhận danh sách **CloudFormation stack**, **Amplify apps**, và danh sách **RDS databases** đều trống để không để lại một hóa đơn đang chạy. Dữ liệu budget trễ tới 24 giờ, nên hãy kiểm tra lại vào ngày hôm sau.
