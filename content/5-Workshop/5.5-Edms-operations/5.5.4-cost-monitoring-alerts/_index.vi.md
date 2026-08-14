---
title : "Giám sát Chi phí & Cảnh báo"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

Vì workshop này xây dựng nhiều dịch vụ AWS, việc **giám sát và kiểm soát chi phí** là rất quan trọng để bạn không bị bất ngờ với hóa đơn. Phần này thiết lập **AWS Budgets** với **cảnh báo email SNS**, khám phá chi phí với **Cost Explorer**, và thêm **CloudWatch alarm** cho lỗi API.

### 5.5.4.1 Tạo AWS Budget với cảnh báo email

1. Mở **Billing** → **Budgets** → **Create budget**.
2. Chọn **Cost budget**.
3. Đặt mức ngân sách, ví dụ **$10/tháng**.
4. Cấu hình **Budget alerts** ở nhiều ngưỡng:
   + **50%** ngân sách — nhắc nhở sớm.
   + **80%** ngân sách — gần giới hạn.
   + **100%** ngân sách — vượt ngân sách.
5. Với mỗi ngưỡng, thêm địa chỉ email để **thông báo** (email được gửi qua **Amazon SNS**).
6. Bấm **Create budget**.

> **Ghi chú:** Nhiều ngưỡng ở 50 / 80 / 100% cung cấp cảnh báo sớm để bạn hành động (stop hoặc xóa tài nguyên) trước khi hóa đơn tăng.

### 5.5.4.2 Khám phá chi phí với Cost Explorer

1. Mở **Billing** → **Cost Explorer**.
2. Chọn **date range** bao phủ hoạt động workshop của bạn.
3. **Group by Service** để xem dịch vụ nào tốn nhiều nhất.
4. Xem xét phân bổ giữa Lambda, API Gateway, S3, Cognito, Step Functions, và Aurora.

> **Mẹo:** Trong kiến trúc này, **Aurora** là nguồn chi phí chính vì nó là database cluster chạy liên tục. Hãy **stop hoặc xóa nó khi không dùng** để giữ hóa đơn gần bằng 0.

### 5.5.4.3 Tạo CloudWatch alarm (tùy chọn)

Tạo CloudWatch alarm trên một metric vận hành, ví dụ ngưỡng **5XX error** trên API:

1. Mở **CloudWatch** → **Alarms** → **Create alarm**.
2. Chọn metric `AWS/ApiGateway` → `5XXError` → API của bạn.
3. Đặt điều kiện, ví dụ **Greater than 0** cho 1 datapoint (bất kỳ lỗi server nào cũng kích hoạt cảnh báo).
4. Chọn một **SNS topic** làm action thông báo.
5. Thêm tên alarm (ví dụ `edms-api-5xx`) và bấm **Create alarm**.

> **Ghi chú:** Cùng với budget, cảnh báo này thông báo cho bạn về các vấn đề vận hành cũng có thể cho thấy một workload đang chạy tốn chi phí.
