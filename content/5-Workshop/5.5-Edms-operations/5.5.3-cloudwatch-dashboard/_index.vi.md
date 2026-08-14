---
title : "CloudWatch Dashboard"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

Một **CloudWatch Dashboard** cung cấp một cái nhìn tổng hợp về sức khỏe và hiệu năng của ứng dụng EDMS bằng cách gộp các metric từ Lambda, API Gateway, và Step Functions trên một trang.

### 5.5.3.1 Tạo dashboard

1. Mở **CloudWatch console** → **Dashboards** → **Create dashboard**.
2. Đặt tên dashboard, ví dụ `edms-dashboard`, và bấm **Create**.


### 5.5.3.2 Thêm Lambda widget

1. Bấm **Add widget** → chọn **Line**.
2. Trong **Metrics** → **AWS namespaces**, chọn **Lambda**.
3. Chọn dimension metric cho function `edms-lambda-stack-EdmsApiFunction` của bạn và thêm:
   + **Invocations** — tần suất function chạy.
   + **Errors** — số execution thất bại.
4. Cấu hình **period** (ví dụ 5 phút) và bấm **Create widget**.

### 5.5.3.3 Thêm API Gateway widget

1. Bấm **Add widget** → **Line**.
2. Trong **AWS namespaces**, chọn **API Gateway** → API của bạn.
3. Thêm các metric:
   + **Count** — tổng số request.
   + **4XXError** — lỗi client (request sai / lỗi xác thực).
   + **5XXError** — lỗi server.
4. Bấm **Create widget**.

### 5.5.3.4 Thêm Step Functions widget

1. Bấm **Add widget** → **Line**.
2. Trong **AWS namespaces**, chọn **Step Functions** → state machine của bạn.
3. Thêm metric **ExecutionsSucceeded** — số workflow phê duyệt hoàn thành thành công.
4. Bấm **Create widget**.

### 5.5.3.5 Sắp xếp và lưu dashboard

1. Thêm nhiều widgets và kéo thả để sắp xếp bố cục.
2. Bấm **Save dashboard**.
3. Mở lại dashboard sau đó để xem chế độ xem trực tiếp về sức khỏe ứng dụng.


> **Ghi chú:** Dashboard là **read-only** và rất rẻ. Nó không tự đưa ra cảnh báo — nó chỉ giúp bạn phát hiện bất thường trong một cái nhìn trước khi chúng ảnh hưởng đến người dùng.
