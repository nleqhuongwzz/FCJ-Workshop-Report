---
title : "Xác minh Kết quả Stack và Lấy Outputs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Sau khi stack được tạo, xác minh các tài nguyên tồn tại và lấy **deploy role ARN** mà pipeline sẽ giả định.

### 5.4.4.1 Xác minh trạng thái stack

1. Mở **CloudFormation console** → **Stacks**.
2. Tìm `edms-iam-stack` và xác nhận **Status** của nó là **CREATE_COMPLETE**.

![Figure 17. Stack hoàn tất](/images/5-Workshop/5.4-Edms-deployment/stack-complete.png)

3. Nếu status là **CREATE_FAILED** hoặc **ROLLBACK_COMPLETE**, bấm **Events** để xem lỗi (thường là vấn đề ThumbprintList hoặc xác nhận IAM), sửa lại, rồi tạo lại stack.

### 5.4.4.2 Xem các tài nguyên đã tạo

1. Chọn stack `edms-iam-stack`.
2. Mở tab **Resources**.
3. Xác nhận các tài nguyên sau tồn tại và có logical ID khớp template:

+ `GithubOidcProvider` — một `AWS::IAM::OIDCProvider`.
+ `GithubActionsDeployRole` — một `AWS::IAM::Role` tên `github-actions-deploy-role`.

### 5.4.4.3 Lấy deploy role ARN

1. Mở tab **Outputs** của stack.
2. Sao chép giá trị `DeployRoleArn`, trông giống như:

```
arn:aws:iam::<account-id>:role/github-actions-deploy-role
```


3. Giữ ARN này — bạn sẽ lưu nó làm secret `AWS_DEPLOY_ROLE_ARN` trên GitHub ở mục tiếp theo.

> **Ghi chú:** Ngoài ra, OIDC role có thể được tạo thủ công trong IAM console (xem 5.3.3). CloudFormation được ưu tiên ở đây vì cấu hình được version hóa và tái tạo được.
