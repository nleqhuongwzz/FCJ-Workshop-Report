---
title : "Kiểm thử End-to-End"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5.6 </b> "
---

Thực hiện một **kiểm thử end-to-end** qua ứng dụng web đã deploy để xác minh toàn bộ luồng — đăng nhập, upload tài liệu, nộp, và phê duyệt — hoạt động trên các dịch vụ AWS thật.

### 5.5.6.1 Mở ứng dụng

1. Mở URL **Amplify** (default domain, ví dụ `https://main.d3xxxx.amplifyapp.com`).
2. Bạn sẽ thấy trang **đăng nhập EDMS**, được phục vụ qua HTTPS.

![Figure 50. Trang đăng nhập](/images/5-Workshop/5.5-Edms-operations/login.png)

### 5.5.6.2 Đăng nhập với vai trò USER

1. Đăng nhập với tài khoản **USER** đã tạo trong Cognito (mục 5.4).
2. Xác nhận bạn đến được dashboard tài liệu.

![Figure 51. Đăng nhập](/images/5-Workshop/5.5-Edms-operations/signin.png)

### 5.5.6.3 Tạo và upload tài liệu

1. Bấm **Create / upload** một tài liệu mới.
2. Nhập tiêu đề và đính kèm một file.
3. Lưu tài liệu — trạng thái của nó phải là **`DRAFT`**.

### 5.5.6.4 Nộp để phê duyệt

1. Chọn tài liệu và bấm **Submit**.
2. Xác nhận trạng thái chuyển thành **`PENDING`** (đang chờ quản lý duyệt).
3. Việc nộp này kích hoạt workflow phê duyệt **Step Functions**.

### 5.5.6.5 Duyệt với vai trò MANAGER

1. Đăng xuất khỏi tài khoản USER.
2. Đăng nhập với tài khoản **MANAGER** đã tạo trong Cognito.
3. Mở danh sách **pending approvals**.
4. **Duyệt** tài liệu — xác nhận trạng thái chuyển thành **`APPROVED`**.
5. Kiểm tra hộp thư để nhận **email thông báo SNS** xác nhận việc phê duyệt.

![Figure 52. Tài liệu được duyệt](/images/5-Workshop/5.5-Edms-operations/approved.png)

### 5.5.6.6 Xác minh Step Functions execution

1. Mở **Step Functions console**.
2. Tìm execution được tạo cho tài liệu này.
3. Xác nhận trạng thái của nó là **`SUCCEEDED`** và mỗi state (kể cả state thông báo) đã hoàn tất.

> **Tiêu chí thành công:** Tài liệu đi qua `DRAFT → PENDING → APPROVED`, Step Functions execution **thành công**, và một **email** được gửi đến. Nếu bước nào thất bại, hãy dùng các Logs Insights queries trong mục 5.5.5 để chẩn đoán.
