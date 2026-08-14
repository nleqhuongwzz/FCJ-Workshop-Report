---
title : "Liên kết Lambda với API Gateway"
date : 2024-01-01
weight : 12
chapter : false
pre : " <b> 5.4.12 </b> "
---

Phần này hoàn tất tích hợp API Gateway ↔ Lambda và **deploy** API lên một stage để có invoke URL công khai.

### 5.4.12.1 Cấu hình Lambda integration

1. Trong API Gateway console, mở `edms-api` và chọn method `ANY` dưới `{proxy+}`.
2. Mở **Integration Request**.
3. Xác nhận **Integration type = Lambda Function**, đúng **region**, và tên Lambda EDMS.
4. Bấm **Save**.
5. Nếu được nhắc, cấp quyền cho API Gateway invoke Lambda bằng cách bấm **OK**.


> **Ghi chú:** Quyền invoke (một tài nguyên `AWS::Lambda::Permission`) là thứ cho phép API Gateway gọi hàm. Thiếu nó, các lệnh gọi trả về `403 Forbidden` với lỗi `Missing Authentication Token` hoặc access-denied.

### 5.4.12.2 Deploy API

Một API Gateway cần được **deploy lên một stage** trước khi có URL dùng được:

1. Bấm **Deploy API**.
2. **Stage:** chọn `Prod`, hoặc tạo **New stage** tên `Prod`.
3. (Tùy chọn) Thêm mô tả stage.
4. Bấm **Deploy**.


### 5.4.12.3 Lấy invoke URL

1. Sau khi deploy, console hiển thị **Invoke URL** cho stage `Prod`:

```
https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod
```


2. Sao chép URL này — nó là điểm vào công khai cho toàn bộ backend.

### 5.4.12.4 Test bằng curl

Xác nhận API phản hồi trước khi nối frontend:

```bash
curl https://xxxx.execute-api.ap-southeast-1.amazonaws.com/Prod/health
```

Trả về `200 OK` (hoặc JSON health của Spring Boot) nghĩa là liên kết hoạt động.

> **Ghi chú:** Đặt URL này làm `REACT_APP_API_URL` cho frontend và làm API base công khai của backend để ứng dụng biết nơi gửi request.
