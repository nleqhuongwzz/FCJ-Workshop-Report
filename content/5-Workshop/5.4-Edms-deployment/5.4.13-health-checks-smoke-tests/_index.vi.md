---
title : "Health Checks và Smoke Tests sau Deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13 </b> "
---

Sau khi deploy, chạy health checks và smoke tests để xác minh API hoạt động end-to-end: từ API Gateway qua Lambda, vào Aurora, và xuyên suốt quy trình phê duyệt Step Functions.

### 5.4.13.1 Test endpoint health

1. Xác nhận API truy cập được bằng một health check đơn giản:

```bash
curl https://<invoke-url>/health
```

2. Mong đợi trả về `200 OK` với JSON từ Spring Boot Actuator (ví dụ `{"status":"UP"}`).

### 5.4.13.2 Test endpoint đăng nhập

1. Gọi endpoint đăng nhập với một trong các user Cognito bạn tạo ở 5.4.7:

```bash
curl -X POST https://<invoke-url>/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@edms.vn","password":"mat-khau-cua-ban"}'
```

2. Phản hồi thành công trả về một **token** và **vai trò** của người dùng (ví dụ `ADMIN`).


> **Ghi chú:** Dùng đúng mật khẩu tạm bạn đặt trong Cognito. Nếu tài khoản yêu cầu đổi mật khẩu, hãy chạy flow change-password trước.

### 5.4.13.3 Smoke test quy trình phê duyệt

Giờ test toàn bộ điều phối serverless (Step Functions + SNS):

1. Dùng token tài khoản **USER**, gọi `POST /approval/submit` với một `documentId`. Việc này khởi động state machine và để nó chờ ở `CaptureToken`.
2. Xác nhận tài liệu trở thành `PENDING`.
3. Dùng token tài khoản **MANAGER**, gọi `POST /approval/approve` với cùng `documentId`. Lambda gọi `SendTaskSuccess`, tiếp tục workflow.
4. Xác nhận tài liệu trở thành `APPROVED`.
5. Kiểm tra hộp thư để nhận **email SNS** `EDMS - Approval`.

![Figure 37. Smoke test phê duyệt](/images/5-Workshop/5.4-Edms-deployment/approval-test.png)

### 5.4.13.4 Kiểm tra Step Functions execution

1. Mở **Step Functions console** → state machine `DocumentApprovalStateMachine` của bạn → **Executions**.
2. Bạn sẽ thấy một execution **SUCCEEDED** cho tài liệu đã duyệt.
3. Mở nó và xác minh các state đã đi qua: `CaptureToken` → `Decision` → `MarkApproved` → `NotifyApproved`.

![Figure 38. Execution thành công](/images/5-Workshop/5.4-Edms-deployment/execution-succeeded.png)

> **Ghi chú:** Định nghĩa ASL đầy đủ nằm trong repository (`backend/template.yaml`, resource `DocumentApprovalStateMachine`). Nó dùng `waitForTaskToken` để workflow có thể tạm dừng chờ quyết định của manager, rồi tiếp tục qua `SendTaskSuccess`.
