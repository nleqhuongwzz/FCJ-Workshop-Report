---
title : "Lambda Concurrency & Auto-Scaling"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Lambda là một dịch vụ **serverless**: AWS chạy nó cho bạn và **tự động scale** theo lưu lượng. Bạn không phải quản lý server hay số lượng instance. Phần này giải thích cách scaling hoạt động, cách kiểm soát bằng **reserved concurrency**, và cách giám sát nó.

### 5.5.1.1 Hiểu cách Lambda auto-scaling hoạt động

1. Khi có request đến, AWS Lambda khởi động một **execution environment** để chạy function của bạn.
2. Nếu request thứ hai đến **trong khi request đầu vẫn đang chạy**, Lambda khởi động một **execution đồng thời thứ hai** trong một environment riêng.
3. Khi lưu lượng tăng, Lambda tiếp tục thêm các execution đồng thời cho đến **giới hạn concurrency của tài khoản** (mặc định 1000 execution đồng thời mỗi region).
4. Khi request hoàn tất, các environment không dùng sẽ được **thu hồi tự động**, nên bạn chỉ trả tiền cho compute time thực sự dùng.

Vì **không có server phải vá lỗi hay quản lý**, nền tảng tự động scale lên/xuống dựa hoàn toàn vào tốc độ request.

> **Ghi chú:** Việc scale tự động này khiến Lambda lý tưởng cho các workload khó dự đoán như API EDMS.

### 5.5.1.2 Cấu hình reserved concurrency (tùy chọn)

Mặc định Lambda có thể bùng lên tới giới hạn tài khoản. Nếu bạn muốn **giới hạn** số execution đồng thời function có thể dùng — ví dụ để bảo vệ database downstream như Aurora khỏi một cú sốc lưu lượng — hãy cấu hình **reserved concurrency**:

1. Mở **Lambda console** → chọn function của bạn, ví dụ `edms-lambda-stack-EdmsApiFunction`.
2. Mở tab **Configuration** → **Concurrency**.
3. Bấm **Edit**.
4. Chọn **Reserve concurrency** và đặt một giá trị (ví dụ `5`).
5. Bấm **Save**.


> **Ghi chú:** Trong `template.yaml` điều này được đặt bằng thuộc tính `ReservedConcurrentExecutions` trên resource function. Giá trị `0` vô hiệu hóa function; bất kỳ giá trị nào khác trở thành giới hạn cứng cho các execution đồng thời.

### 5.5.1.3 Hiểu provisioned concurrency (nâng cao)

**Provisioned concurrency** làm nóng sẵn một số environment cố định để sẵn sàng ngay lập tức, tránh **cold starts**:

+ Hữu ích cho các function nhạy cảm với độ trễ.
+ **Không bắt buộc** cho workshop này và **tốn thêm chi phí**, nên hãy tắt trừ khi bạn thực sự cần.

### 5.5.1.4 Giám sát hành vi scaling

1. Trong Lambda console mở tab **Monitor** của function.
2. Xem biểu đồ **Invocations** để biết function đã chạy bao nhiêu lần.
3. Xem biểu đồ **Concurrent executions** để xác nhận Lambda đã scale in/out theo lưu lượng.
4. Tùy chọn so sánh hai biểu đồ với CloudWatch Dashboard bạn xây dựng trong mục 5.5.3.

![Figure 39. Giám sát Lambda](/images/5-Workshop/5.5-Edms-operations/lambda-monitor.png)

> **Ghi chú:** Nếu bạn thấy **Throttles** trên trang giám sát, reserved concurrency của bạn quá thấp hoặc bạn đang chạm giới hạn tài khoản — hãy tăng giá trị tương ứng.
