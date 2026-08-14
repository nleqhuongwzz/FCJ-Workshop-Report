---
title : "Tạo API Gateway"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.4.11 </b> "
---

**API Gateway** phơi backend Lambda thành một REST API công khai. Trong production điều này được định nghĩa trong `template.yaml` và được SAM tạo tự động. Ở đây bạn tạo thủ công để hiểu các thành phần, dùng **REST API** với resource catch-all `{proxy+}`.

### 5.4.11.1 Tạo REST API

1. Mở **API Gateway console** → **Create API**.
2. Trong **REST API** (không phải HTTP API), bấm **Build**.


3. **Choose the protocol:** REST. **Create new API.**
4. **API name:** `edms-api`.
5. Giữ **Endpoint Type** là **Regional** (hoặc chọn Edge cho dùng CDN).
6. Bấm **Create API**.

### 5.4.11.2 Thêm resource proxy catch-all

Một resource trong REST API tương ứng với một path. Để chuyển tiếp **mọi** path tới Lambda, thêm resource `{proxy+}`:

1. Trên trang **Resources** của API, bấm **Actions** → **Create Resource**.
2. Tick **Configure as proxy resource**.
3. Trong **Resource Path**, giá trị trở thành `{proxy+}`.
4. Bấm **Create Resource**.

### 5.4.11.3 Cấu hình method ANY

Proxy resource tự động có một method **ANY**. Cấu hình nó trỏ tới Lambda:

1. Chọn method `ANY` dưới `{proxy+}`.
2. Trong **Integration Request**, đặt:
   + **Integration type:** Lambda Function
   + **Lambda Region:** `ap-southeast-1`
   + **Lambda Function:** Lambda EDMS của bạn (ví dụ `EdmsBackendFunction`)
3. Bấm **Save**.


4. API Gateway sẽ nhắc cấp quyền invoke Lambda — bấm **OK** để cho phép.

### 5.4.11.4 Thêm method ở root

Để health check hoặc root API, thêm method **ANY** (hoặc `GET`) trên resource gốc `/`:

1. Chọn resource gốc `/`.
2. Bấm **Actions** → **Create Method** → chọn **ANY** (hoặc `GET`).
3. Đặt cùng Lambda integration và bấm **Save**.


> **Ghi chú:** Resource `{proxy+}` cho phép API Gateway chuyển tiếp mọi path (`/auth/login`, `/documents`, ...) đến Lambda, nơi nó route bên trong ứng dụng Spring Boot. Đây là điều làm cho toàn bộ backend truy cập được qua một API duy nhất.
