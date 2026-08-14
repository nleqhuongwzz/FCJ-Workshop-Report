---
title : "Hosting Amplify + HTTPS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**AWS Amplify** host frontend React qua **HTTPS** và kết nối với repository **Git** của bạn, nên mỗi lần push lên nhánh sẽ tự động rebuild và deploy. Phần này bao gồm kết nối GitHub, cấu hình build spec cho thư mục `frontend/`, thêm biến môi trường, và triển khai.

### 5.5.2.1 Tạo Amplify app

1. Mở **Amplify console** → **All apps** → **New app** → **Host web app**.
2. Chọn **Git provider** của bạn (ví dụ **GitHub**) và bấm **Continue**.
3. Cấp quyền cho Amplify truy cập tài khoản GitHub, nếu được yêu cầu.
4. Chọn **repository EDMS** và nhánh **`main`**.
5. Bấm **Next**.


> **Ghi chú:** Frontend nằm trong thư mục con `frontend/` của repository, nên bạn phải trỏ build vào thư mục đó.

### 5.5.2.2 Cấu hình build settings

Trong mục **Build settings**, thay build spec bằng một build spec trỏ đúng thư mục `frontend/`. Các điểm chính:

+ `preBuild` — di chuyển vào `frontend` và cài đặt dependencies bằng `npm ci` (cài đặt sạch, dựa trên lock file).
+ `build` — di chuyển vào `frontend` và chạy `npm run build` để tạo bundle tĩnh.
+ `baseDirectory` — đặt thành `frontend/build` để Amplify phục vụ output đã tạo.

```yaml
version: 1
applications:
  - frontend:
      phases:
        preBuild:
          commands:
            - cd frontend && npm ci
        build:
          commands:
            - cd frontend && npm run build
      artifacts:
        baseDirectory: frontend/build
        files:
          - '**/*'
```

> **Ghi chú:** Nếu build không tìm thấy `package-lock.json`, hãy chạy `npm install` trong `frontend/` và commit lock file trước khi deploy.

### 5.5.2.3 Thêm biến môi trường

Trong **Environment variables**, thêm các giá trị từ các phần trước. Các biến này cho ứng dụng đã build biết API và Cognito nằm ở đâu:

```
REACT_APP_API_URL=<API Gateway invoke URL>
REACT_APP_COGNITO_USER_POOL_ID=<pool-id>
REACT_APP_COGNITO_CLIENT_ID=<client-id>
REACT_APP_COGNITO_REGION=ap-southeast-1
```

+ `REACT_APP_API_URL` — API Gateway invoke URL (mục 5.4).
+ `REACT_APP_COGNITO_USER_POOL_ID` — id User Pool Cognito của bạn.
+ `REACT_APP_COGNITO_CLIENT_ID` — id app client Cognito của bạn.
+ `REACT_APP_COGNITO_REGION` — region, `ap-southeast-1`.

### 5.5.2.4 Deploy ứng dụng

1. Bấm **Save and deploy**.
2. Amplify chạy pipeline build (source → preBuild → build → deploy).
3. Chờ trạng thái build trở thành **Available**.

![Figure 42. Amplify deployed](/images/5-Workshop/5.5-Edms-operations/amplify-deployed.png)

> **Ghi chú:** **Default domain** (ví dụ `https://main.d3xxxx.amplifyapp.com`) là URL công khai của ứng dụng và được phục vụ tự động qua **HTTPS** với chứng chỉ hợp lệ.
