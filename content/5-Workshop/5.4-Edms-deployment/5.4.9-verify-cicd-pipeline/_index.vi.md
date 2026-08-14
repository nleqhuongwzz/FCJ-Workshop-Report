---
title : "Xác minh CI/CD Pipeline"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9 </b> "
---

Sau khi push code và cấu hình workflow, hãy xác minh pipeline chạy và deploy thành công. Điều này xác nhận OIDC role, các secret, và template SAM đều được kết nối đúng.

### 5.4.9.1 Mở các workflow runs

1. Trong repository của bạn, mở tab **Actions**.
2. Bạn sẽ thấy workflow `EDMS CI/CD`. Nếu có run, bấm vào run gần nhất.

![Figure 25. Workflow runs](/images/5-Workshop/5.4-Edms-deployment/workflow-runs.png)

> **Ghi chú:** Nếu không có run nào, hãy chắc chắn file `deploy.yml` đã được commit và push lên `main`. Workflow chỉ kích hoạt trên push vào nhánh đó (5.4.2).

### 5.4.9.2 Xác minh từng job

Bấm vào run gần nhất và xác nhận cả ba job đều thành công:

+ `test-backend` — ✅ chạy `mvn test`.
+ `build-frontend` — ✅ chạy `npm ci && npm run build`.
+ `deploy` — ✅ xác thực qua OIDC và chạy `sam deploy`.

![Figure 26. Các job pass](/images/5-Workshop/5.4-Edms-deployment/jobs-pass.png)

Nếu bạn đã cấu hình **required reviewers** ở 5.4.6, job `deploy` sẽ hiện nút **Review deployments** — hãy phê duyệt để job tiếp tục.

### 5.4.9.3 Xác minh stack được cập nhật

1. Mở **CloudFormation console** → **Stacks**.
2. Tìm `edms-lambda-stack` (stack mà SAM deploy tới).
3. Xác nhận trạng thái của nó là **UPDATE_COMPLETE** (hoặc **CREATE_COMPLETE** ở lần chạy đầu).

![Figure 27. Stack được cập nhật](/images/5-Workshop/5.4-Edms-deployment/stack-updated.png)

4. Mở tab **Resources** và xác nhận hàm Lambda cùng các tài nguyên khác (API Gateway, Step Functions) đã được tạo.

> **Ghi chú:** Nếu một job fail, bấm vào nó để xem log và sửa lỗi (thường là thiếu secret, trust policy OIDC sai, hoặc lỗi template SAM), rồi push lại để chạy lại pipeline.
