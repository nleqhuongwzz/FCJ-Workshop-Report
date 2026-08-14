---
title : "Tạo Cognito User Pool"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7 </b> "
---

Cognito User Pool tạo ở 5.3.4 cung cấp xác thực. Trong phần này bạn tạo **người dùng** và gán vào các **nhóm** để test các hành vi vai trò khác nhau (`ADMIN`, `MANAGER`, `USER`).

### 5.4.7.1 Mở User Pool của bạn

1. Mở **Cognito console** → **User pools**.
2. Chọn user pool của bạn (từ 5.3.4).
3. Mở tab **Users** ở bên trái.

### 5.4.7.2 Tạo user test

1. Bấm **Create user**.
2. Điền email làm **username** và đặt một **mật khẩu tạm**.
3. Tick **Mark as verified** cho email (tùy chọn, tránh flow xác minh khi test).
4. Bấm **Create user**.


Lặp lại cho ít nhất ba user: mỗi user cho một vai trò (`ADMIN`, `MANAGER`, `USER`).

### 5.4.7.3 Gán user vào một nhóm

1. Trong danh sách user, bấm vào email của user vừa tạo.
2. Mở tab **Groups** của user đó.
3. Bấm **Add user to group**.
4. Chọn một trong `ADMIN`, `MANAGER`, `USER` rồi bấm **Add user to group**.


Lặp lại để mỗi user test thuộc nhóm vai trò dự định.

### 5.4.7.4 Xác minh qua AWS CLI (tùy chọn)

Bạn có thể xác nhận users và groups từ terminal:

```bash
aws cognito-idp list-users --user-pool-id <COGNITO_USER_POOL_ID>
aws cognito-idp admin-list-groups-for-user \
  --user-pool-id <COGNITO_USER_POOL_ID> \
  --username admin@edms.vn
```

> **Ghi chú:** Tạo ít nhất một user trong mỗi nhóm (`ADMIN`, `MANAGER`, `USER`) để sau này test các hành vi vai trò khác nhau trong smoke test phê duyệt (5.4.13).
