---
title : "Khởi tạo và Cấu hình S3"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Amazon S3 lưu trữ các file tài liệu gốc của EDMS. Vì EDMS truy cập file qua **pre-signed URLs**, bucket được tạo ở chế độ **private** (chặn hoàn toàn public access).

### 5.3.1.1 Chuẩn bị

Trước khi tạo bucket, chuẩn bị:

1. Mở **AWS Management Console** và đăng nhập tài khoản của bạn.
2. Chọn region **ap-southeast-1** (Singapore) ở góc trên bên phải.
3. Ghi lại **account ID** (12 chữ số) — bạn sẽ dùng nó để đặt tên bucket duy nhất.

> **Ghi chú:** Tên S3 bucket phải **duy nhất toàn cầu** (không chỉ riêng region của bạn). Việc thêm account ID vào tên giúp tránh trùng lặp.

### 5.3.1.2 Tạo S3 bucket

1. Mở **Amazon S3 console**.
2. Trong danh sách **Buckets**, bấm **Create bucket**.


3. Trong trang **Create bucket**, cấu hình:
+ **Bucket name:** tên duy nhất toàn cầu, ví dụ `edms-docs-bucket-<account-id>`.
+ **AWS Region:** `ap-southeast-1`.
+ Trong **Object Ownership**: giữ mặc định **ACLs disabled**.
+ Trong **Block Public Access settings for this bucket**: **giữ cả bốn ô được tick** — bucket phải ở chế độ private vì EDMS truy cập file qua pre-signed URLs.
+ **Bucket Versioning:** **bật** — hỗ trợ lịch sử phiên bản tài liệu (mỗi lần upload mới tạo một version mới thay vì ghi đè).
+ **Tags (optional):** thêm tag, ví dụ `Project=EDMS`.
4. Bấm **Create bucket**.
![Figure 1. Tạo bucket](/images/5-Workshop/5.3-Edms-infrastructure/create-bucket.png)


> **Ghi chú:** Không tạo bất kỳ policy công khai nào cho bucket này. Toàn bộ quyền truy cập file được cấp tạm thời qua pre-signed URLs do backend sinh ra.

### 5.3.1.3 Xác minh bucket

1. Quay lại danh sách **Buckets**, xác nhận bucket của bạn xuất hiện với trạng thái **0 objects**.
2. Mở bucket — nó phải **trống** và **private**.
3. Vào tab **Permissions**, kiểm tra **Block public access (bucket settings)** hiển thị "Block all public access: On".

![Figure 3. Bucket đã tạo](/images/5-Workshop/5.3-Edms-infrastructure/bucket-created.png)

### 5.3.1.4 Ghi lại tên bucket

Backend cần tên bucket để lưu và lấy file. Ghi lại chính xác tên bucket — bạn sẽ đưa nó vào cấu hình `.env` / SAM (biến `AWS_S3_BUCKET`) ở các phần sau.

```
AWS_S3_BUCKET=edms-docs-bucket-<account-id>
AWS_S3_REGION=ap-southeast-1
```

> **Ghi chú:** Với workshop này bucket luôn ở chế độ private. EDMS tạo **pre-signed URLs** có thời hạn ngắn để người dùng upload/download file mà **không** cần mở public access. Đây là cách bảo mật file tốt hơn nhiều so với bucket public.
