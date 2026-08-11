---
title: "Worklog Tuần 4"
date: 2026-07-24
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:
* Nghiên cứu, phân tích và lựa chọn đề tài dự án thực tế phù hợp để tổng hợp, áp dụng toàn diện các kiến thức AWS nền tảng đã tích lũy từ các tuần trước vào dự án.
* Xây dựng và thiết kế kiến trúc Serverless sơ bộ, đồng thời nghiên cứu kỹ lưỡng để xác định chính xác các dịch vụ và công nghệ cốt lõi cấu thành hệ thống.

### Các công việc đã thực hiện:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :--- | :--- | :--- | :--- |
| **1** | **Lên ý tưởng dự án**<br>- Khảo sát kỹ lưỡng các xu hướng công nghệ và tìm hiểu sâu các bài toán thực tế cần ứng dụng điện toán đám mây trong doanh nghiệp.<br>- Thảo luận chi tiết, đánh giá tính khả thi và đi đến quyết định xây dựng Hệ thống Quản lý Tài liệu Điện tử (EDMS) theo mô hình Serverless hiện đại. | 12/07/2026 | 12/07/2026 |
| **2** | **Lựa chọn công nghệ**<br>- Đánh giá toàn diện và lựa chọn các dịch vụ AWS tối ưu nhất cho backend EDMS: Amazon API Gateway, AWS Lambda và Amazon S3.<br>- Nghiên cứu tích hợp Amazon Aurora Serverless nhằm đảm bảo khả năng quản lý, lưu trữ siêu dữ liệu (metadata) quan hệ một cách linh hoạt, tự động co giãn theo tải. | 13/07/2026 | 13/07/2026 |
| **3** | **Thiết kế kiến trúc**<br>- Phác thảo sơ đồ kiến trúc hệ thống tổng quan, thể hiện rõ ràng các tầng giao tiếp và cách kết nối liền mạch giữa các dịch vụ AWS đã chọn.<br>- Lập kế hoạch chi tiết, phân chia các mốc thời gian, khối lượng công việc và lộ trình cụ thể cho toàn bộ các tuần phát triển tiếp theo. | 14/07/2026 | 14/07/2026 |
| **4** | **Nghiên cứu luồng xử lý sự kiện**<br>- Nghiên cứu luồng tích hợp hướng sự kiện giữa việc tải file lên S3, Lambda trigger và ghi nhật ký database.<br>- Xác định các phương thức bảo mật và xác thực cho các endpoint của API Gateway. | 15/07/2026 | 15/07/2026 |
| **5** | **Hoàn thiện lộ trình và Đánh giá**<br>- Hoàn thiện tài liệu thiết kế và phân công trách nhiệm cho các giai đoạn của dự án.<br>- Chuẩn bị tài liệu cho buổi họp đánh giá kiến trúc hệ thống. | 16/07/2026 | 16/07/2026 |

### Kết quả đạt được:

* **Định hướng dự án:** Hoàn tất việc lựa chọn đề tài Hệ thống Quản lý Tài liệu Điện tử (EDMS) và xây dựng kế hoạch triển khai chi tiết cho giai đoạn hiện thực hóa sản phẩm.
* **Mô hình kiến trúc:** Xây dựng thành công bản vẽ thiết kế hệ thống sử dụng các dịch vụ Serverless tối ưu, giúp hệ thống dễ dàng mở rộng và tiết kiệm chi phí vận hành.
