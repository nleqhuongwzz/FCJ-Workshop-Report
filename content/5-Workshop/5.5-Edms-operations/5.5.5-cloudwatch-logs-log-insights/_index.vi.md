---
title : "CloudWatch Logs và Log Insights"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

Mỗi Lambda function ghi output của nó vào **CloudWatch Logs**. Bạn có thể xem các log đó và chạy **Logs Insights** queries để xử lý sự cố trong API EDMS.

### 5.5.5.1 Xem Lambda logs

1. Mở **CloudWatch console** → **Log groups**.
2. Tìm log group `/aws/lambda/edms-lambda-stack-EdmsApiFunction...` (tên chính xác kết thúc bằng tên function).
3. Mở **log stream** mới nhất (mỗi stream tương ứng với một execution environment).
4. Đọc các log events để xem output của function, timestamp, và mọi message được code in ra.

![Figure 48. Log groups](/images/5-Workshop/5.5-Edms-operations/log-groups.png)

> **Ghi chú:** Mặc định logs được giữ vô thời hạn. Bạn có thể đặt **retention period** trong **Log groups** → **Actions** → **Edit retention** để kiểm soát chi phí lưu trữ.

### 5.5.5.2 Chạy Logs Insights query

Logs Insights cho phép bạn tìm kiếm và tổng hợp dữ liệu log với cú pháp giống SQL:

1. Mở **CloudWatch** → **Logs Insights**.
2. Chọn Lambda log group từ dropdown.
3. Đặt **time range** để bao phủ lỗi bạn muốn điều tra.
4. Chạy một query lọc các lỗi:

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

5. Xem lại các log events khớp, được sắp xếp mới nhất trước.

> **Ghi chú:** Truy vấn log giúp bạn hiểu vì sao một request thất bại — ví dụ **token Cognito không hợp lệ** (401) hoặc **lỗi kết nối database** trong Lambda gọi Aurora.

### 5.5.5.3 Các ví dụ xử lý sự cố phổ biến

+ **Lỗi Throttling / concurrency** — tìm các message về concurrency hoặc throttle trong log.
+ **Cognito 401** — access token bị thiếu, hết hạn, hoặc từ sai client.
+ **Database timeout** — lỗi kết nối Aurora thường xuất hiện dưới dạng exception stack trace.
+ **Step Functions failure** — kiểm tra lịch sử execution của chính state machine và Lambda logs mà nó gọi.
