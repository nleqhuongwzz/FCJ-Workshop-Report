---
title : "Khởi tạo và Cấu hình Cognito"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Amazon Cognito cung cấp xác thực và phân quyền theo vai trò cho EDMS. Người dùng đăng nhập và nhận **JWT** (JSON Web Token) mà backend xác thực trên mỗi request. Phần này tạo **User Pool**, **app client** không có secret, và ba nhóm tương ứng với các vai trò ứng dụng.

### 5.3.4.1 Mở Cognito console

1. Trong AWS console, đảm bảo region là **ap-southeast-1**.
2. Mở **Amazon Cognito console** → **User pools**.

### 5.3.4.2 Tạo User Pool

1. Bấm **Create user pool**.
![Figure 11. Tạo user pool](/images/5-Workshop/5.3-Edms-infrastructure/create-userpool.png)


2. Trong **Configure sign-in experience**:
+ **Sign-in options:** chọn **Email**.
+ Bấm **Next**.
3. Trong **Configure security requirements**:
+ Đặt **password policy** (tối thiểu 8 ký tự, yêu cầu ít nhất một chữ số).
+ Bấm **Next**.
4. Trong **Configure sign-up experience**:
+ Giữ **Self-service sign-up** được bật (hoặc tắt nếu bạn chỉ muốn admin tạo tài khoản).
+ Bấm **Next**.
5. Trong **Configure message delivery**:
+ Chọn **Send email with Cognito**.
+ Bấm **Next**.
6. Trong **Integrate your app**:
+ **User pool name:** `edms-user-pool`.
+ **App client name:** `edms-client`; **bỏ tick "Generate a client secret"** — backend cần client công khai cho luồng đăng nhập.
+ Bấm **Next**.
7. Xem lại cấu hình và bấm **Create user pool**.

![Figure 12. User pool đã tạo](/images/5-Workshop/5.3-Edms-infrastructure/userpool-created.png)

> **Ghi chú:** App client **không có secret** để dùng được trong các luồng chạy trên trình duyệt. Không bật client secret cho client công khai này.

### 5.3.4.3 Tạo các nhóm (vai trò)

Các Cognito group ánh xạ trực tiếp tới vai trò ứng dụng (`ADMIN` / `MANAGER` / `USER`).

1. Mở User Pool của bạn (`edms-user-pool`).
2. Mở tab **Groups**.
3. Bấm **Create group** và tạo ba nhóm:
+ `ADMIN`
+ `MANAGER`
+ `USER`

![Figure 13. Groups](/images/5-Workshop/5.3-Edms-infrastructure/groups.png)

4. Gán người dùng vào các nhóm. Người dùng trong một nhóm kế thừa vai trò của nhóm đó trong ứng dụng.

> **Ghi chú:** `ADMIN` có toàn quyền, `MANAGER` quản lý tài liệu của nhóm mình, còn `USER` là thành viên thường với quyền xem/bình luận.

### 5.3.4.4 Ghi lại các ID

Backend và frontend cần các giá trị sau — hãy ghi lại:

```
COGNITO_USER_POOL_ID=<pool-id>     ví dụ ap-southeast-1_XXXXX
COGNITO_CLIENT_ID=<client-id>
COGNITO_REGION=ap-southeast-1
```

> **Ghi chú:** Backend ánh xạ một Cognito group thành vai trò ứng dụng (`ADMIN` / `MANAGER` / `USER`) và xác thực JWT trên mỗi request. Giữ pool ID và client ID để dùng trong cấu hình `.env` / SAM ở các phần sau.
