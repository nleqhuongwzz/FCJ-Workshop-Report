---
title : "Khởi tạo và Cấu hình Aurora RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

EDMS dùng **Amazon Aurora (MySQL)** để lưu toàn bộ metadata quan hệ (người dùng, thư mục, tài liệu, bình luận, lịch sử phiên bản). Trong phần này bạn tạo Aurora cluster, đợi nó khả dụng, tạo database `edms` ban đầu và ghi lại endpoint để cấu hình sau.

### 5.3.2.1 Mở RDS console

1. Trong AWS console, đảm bảo region là **ap-southeast-1**.
2. Mở **Amazon RDS console**.
3. Trong menu bên trái, bấm **Databases** để xem danh sách cluster (hiện đang trống).

### 5.3.2.2 Tạo Aurora MySQL cluster

1. Bấm **Create database**.


2. Trong trang **Create database**, cấu hình:
+ **Engine options:** chọn **Amazon Aurora** và edition **MySQL**.
+ **Capacity type:** **Serverless v2** (khuyến nghị cho workshop — tự co giãn theo mức sử dụng) hoặc **Provisioned**.
+ **DB cluster identifier:** `edms-cluster`.
+ **Credentials:** đặt **Master username** (ví dụ `admin`) và **Master password** mạnh. Lưu mật khẩu ở nơi an toàn.
+ **Instance configuration:** nếu chọn **Provisioned**, chọn instance nhỏ như `db.t3.medium`.
+ **Connectivity:** chọn **Don't connect to an EC2 compute resource**.
+ Giữ nguyên các cài đặt còn lại.
3. Bấm **Create database**.
![Figure 4. Tạo database](/images/5-Workshop/5.3-Edms-infrastructure/create-database.png)


> **Ghi chú:** Aurora dùng mô hình **cluster** có thể chứa một hoặc nhiều instance. Một instance là đủ cho workshop này. Sau này Aurora có thể tạo thêm read replica nếu bạn cần tăng hiệu năng đọc.

### 5.3.2.3 Đợi khả dụng

Cluster mất vài phút để provision.

1. Quay lại danh sách **Databases**.
2. Tìm `edms-cluster` và theo dõi cột **Status**.
3. Đợi trạng thái đổi từ *Creating* sang **Available**.
4. (Tùy chọn) Bấm vào tên cluster để xem writer endpoint và reader endpoint.

![Figure 6. Cluster khả dụng](/images/5-Workshop/5.3-Edms-infrastructure/aurora-available.png)

> **Ghi chú:** Đừng tiếp tục cho đến khi cluster hiển thị **Available** — endpoint chưa dùng được trước lúc đó.

### 5.3.2.4 Tạo database ban đầu

Amazon Aurora tạo sẵn một database mặc định. Ứng dụng cần một database riêng tên `edms`.

1. Mở **Query Editor v2** (trong RDS console) hoặc kết nối bằng MySQL client.
2. Chạy SQL sau để tạo database ứng dụng dùng:

```sql
CREATE DATABASE IF NOT EXISTS edms
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

3. Xác minh database đã tồn tại:

```sql
SHOW DATABASES;
```

> **Ghi chú:** Backend tự động áp dụng schema bằng **Flyway** migrations từ `backend/src/main/resources/db/migration`, nên bạn chỉ cần tạo database rỗng ở đây.

### 5.3.2.5 Lấy endpoint

1. Trong danh sách **Databases**, bấm `edms-cluster`.
2. Mở tab **Connectivity & security**.
3. Sao chép giá trị **Endpoint** (hostname) — ví dụ `edms-cluster.cluster-xxxx.ap-southeast-1.rds.amazonaws.com`.

![Figure 7. Endpoint](/images/5-Workshop/5.3-Edms-infrastructure/endpoint.png)

4. Lưu endpoint, user và mật khẩu database vào cấu hình `.env` / SAM của project:

```
AURORA_ENDPOINT=<endpoint>
DB_USER_AWS=admin
DB_PASS_AWS=<mat-khau>
DB_NAME=edms
```

> **Ghi chú chi phí:** Aurora tính phí ngay cả khi không sử dụng. Hãy stop hoặc xóa cluster khi hoàn thành workshop (xem 5.5.7) để tránh phát sinh chi phí kéo dài.
