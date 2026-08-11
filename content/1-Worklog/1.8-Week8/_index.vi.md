---
title: "Worklog Tuần 8"
date: 2026-08-11
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:
* Thực hiện kiểm thử và rà soát toàn diện lại toàn bộ các tính năng của hệ thống trên môi trường thực tế.
* Hoàn thiện hồ sơ tài liệu kỹ thuật và biên soạn báo cáo tổng kết đồ án/thực tập.
* Tiến hành báo cáo nghiệm thu sản phẩm và thực hiện dọn dẹp hạ tầng đám mây.

### Các công việc cần triển khai trong tuần này:

| Ngày | Nội dung công việc chuyên môn | Thời gian bắt đầu | Thời gian hoàn thành |
| :--- | :--- | :--- | :--- |
| **1** | **Kiểm thử và Rà soát Hệ thống**<br>- Tiến hành chạy lại toàn bộ các kịch bản test (Unit test và Integration test), kiểm tra độ ổn định của các luồng API và cơ sở dữ liệu. | 10/08/2026 | 11/08/2026 |
| **2** | **Xử lý Lỗi & Tối ưu Trải nghiệm**<br>- Khắc phục các vấn đề phát sinh trong quá trình kiểm thử, tinh chỉnh lại hiệu năng phản hồi và tối ưu hóa thời gian xử lý của các hàm Lambda. | 12/08/2026 | 12/08/2026 |
| **3** | **Hoàn thiện Báo cáo & Tài liệu**<br>- Tổng hợp kết quả thực hiện toàn bộ các tuần, viết báo cáo tổng kết thực tập chi tiết và chuẩn bị tài liệu bàn giao dự án. | 13/08/2026 | 14/08/2026 |
| **4** | **Báo cáo & Nghiệm thu Dự án**<br>- Thực hiện buổi báo cáo trực tiếp, trình bày các kết quả đạt được và demo hệ thống hoàn chỉnh. | 15/08/2026 | 15/08/2026 |
| **5** | **Dọn dẹp Hạ tầng & Kết thúc**<br>- Xóa bỏ toàn bộ các tài nguyên đã triển khai trên AWS Cloud nhằm tối ưu hóa chi phí vận hành. | 16/08/2026 | 16/08/2026 |

### Tình huống kỹ thuật & Hướng xử lý:

* **Trở ngại gặp phải:** Hiện tượng độ trễ phản hồi xuất hiện ở một số lượt gọi đầu tiên do thời gian khởi tạo tài nguyên của môi trường runtime Java trên nền tảng Serverless.
* **Phương án khắc phục:** Thực hiện gọi kích hoạt sớm (warm-up) các hàm Lambda trước thời điểm kiểm thử và báo cáo, giúp hệ thống luôn ở trạng thái sẵn sàng cao nhất.

### Kết quả đạt được tuần 8:

* **Chất lượng Sản phẩm:** Hệ thống đã vượt qua các bài kiểm định thực tế, đảm bảo vận hành ổn định, bảo mật và đáp ứng trọn vẹn các yêu cầu đặt ra ban đầu.
* **Hoàn thành Mục tiêu:** Hoàn thành xuất sắc toàn bộ quy trình phát triển phần mềm, từ khâu xây dựng, kiểm thử, báo cáo nghiệm thu cho đến bước thanh lý tài nguyên đám mây an toàn.
