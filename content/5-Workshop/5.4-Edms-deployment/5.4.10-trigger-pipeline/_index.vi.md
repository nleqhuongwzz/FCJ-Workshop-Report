---
title : "Kích hoạt Pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10 </b> "
---

Pipeline chạy tự động mỗi lần push lên `main`. Nó cũng có thể được kích hoạt thủ công bằng `workflow_dispatch`.

### 5.4.10.1 Kích hoạt bằng push code

Trigger `on.push.branches: [main]` của workflow tự khởi động khi bạn push lên `main`. Từ thư mục gốc:

```bash
git add .
git commit -m "chore: trigger deployment"
git push origin main
```

Push lên `main` sẽ tự động khởi động workflow `EDMS CI/CD`. Mở tab **Actions** để theo dõi.

> **Ghi chú:** Chỉ commit vào nhánh `main` mới kích hoạt deploy. Push lên các nhánh khác chạy `test-backend` và `build-frontend`, nhưng job `deploy` bị bỏ qua vì điều kiện `if: github.ref == 'refs/heads/main'` (5.4.2).

### 5.4.10.2 Kích hoạt thủ công (workflow_dispatch)

Vì workflow khai báo `workflow_dispatch` trong khối `on`, bạn có thể khởi động thủ công:

1. Mở tab **Actions** của repository.
2. Chọn workflow `EDMS CI/CD` từ thanh bên trái.
3. Bấm **Run workflow**.
4. Chọn **branch** (ví dụ `main`).
5. Bấm **Run workflow**.


### 5.4.10.3 Theo dõi run

1. Một run mới xuất hiện dưới workflow. Bấm vào nó để theo dõi ba job.
2. Nếu bạn bật **required reviewers** (5.4.6), hãy phê duyệt job `deploy` khi được nhắc.
3. Xác nhận run kết thúc với dấu kiểm xanh và bước SAM deploy báo `Successfully created/updated stack - edms-lambda-stack`.

> **Ghi chú:** Chỉ các job thỏa điều kiện `if` của job deploy mới thực sự deploy. Job deploy chỉ chạy trên nhánh `main`, bất kể workflow được kích hoạt bằng cách nào.
