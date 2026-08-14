---
title : "Dọn dẹp Tài nguyên"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7 </b> "
---

Để tránh chi phí phát sinh, hãy xóa mọi tài nguyên bạn đã tạo trong workshop này. Làm theo thứ tự bên dưới để các tài nguyên phụ thuộc được gỡ trước.

### 5.5.7.1 Xóa SAM / CloudFormation stack

Các tài nguyên Lambda, API Gateway, Step Functions, và IAM do SAM tạo có thể được xóa bằng một lệnh. Chạy từ thư mục chứa `template.yaml` của bạn:

```bash
sam delete --stack-name edms-lambda-stack --no-prompts
```

Lệnh này cũng xóa **CloudFormation stack** và các artifacts (Lambda layers, code bundles) được lưu trong deployment bucket mà nó tạo ra.

> **Ghi chú:** Bạn có thể đạt kết quả tương tự trong console bằng cách mở **CloudFormation** → chọn stack → **Delete**. Xác nhận trạng thái stack chuyển thành `DELETE_COMPLETE`.

![Figure 53. Xóa stack](/images/5-Workshop/5.5-Edms-operations/delete-stack.png)

### 5.5.7.2 Xóa các tài nguyên còn lại thủ công

Các tài nguyên sau được tạo ngoài SAM, nên hãy xóa chúng trong console:

1. **Amplify** — xóa app khỏi Amplify console.
2. **Step Functions** — xóa state machine.
3. **SNS** — xóa topic `edms-notifications` (và mọi subscription).
4. **Cognito** — xóa User Pool (và app client).
5. **S3** — **làm trống** và xóa các bucket, kể cả mọi **object versions** và lifecycle policies.
6. **Aurora** — **xóa cluster**; đây là nguồn chi phí chính, đừng bỏ qua.
7. **IAM** — xóa deploy role, sau khi gỡ mọi CloudFormation stack vẫn tham chiếu nó.

![Figure 54. Xóa tài nguyên](/images/5-Workshop/5.5-Edms-operations/delete-resources.png)

### 5.5.7.3 Xác minh chi phí về 0

1. Mở **Billing** → **Cost Explorer**.
2. Xác nhận không còn dịch vụ nào đang tính phí cho tài khoản của bạn.
3. (Tùy chọn) Đặt cảnh báo ngân sách **$0** để được email thông báo nếu còn bất kỳ chi phí nào phát sinh.

> **Thực hành tốt:** Sau khi dọn dẹp, xác nhận danh sách **CloudFormation stack**, **Amplify apps**, và danh sách **EC2/RDS** đều trống để không để lại một hóa đơn đang chạy. Dữ liệu budget trễ tới 24 giờ, nên hãy kiểm tra lại vào ngày hôm sau.
