---
title: "Worklog Tuần 6"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:
* Tiếp tục phát triển và hoàn thiện các module nghiệp vụ nâng cao của hệ thống.
* Tự động hóa quy trình phê duyệt tài liệu bằng AWS Step Functions kết hợp Amazon SNS gửi email thông báo.
* Đóng gói và triển khai toàn bộ backend Serverless bằng mã nguồn cơ sở hạ tầng (AWS SAM).

### Các công việc cần triển khai trong tuần này:

| Ngày | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành |
| :--- | :--- | :--- | :--- |
| **1** | **Phát triển Module & Quy trình Phê duyệt (Step Functions)**<br>- Mở rộng và phát triển các module chức năng phụ trợ cho hệ thống EDMS.<br>- Xây dựng State Machine trên AWS Step Functions để điều phối luồng phê duyệt tài liệu nghiêm ngặt. | 27/07/2026 | 27/07/2026 |
| **2** | **Thông báo Sự kiện (Amazon SNS)**<br>- Tích hợp Amazon SNS để kích hoạt gửi email tự động khi tài liệu được duyệt hoặc được chia sẻ. | 28/07/2026 | 28/07/2026 |
| **3** | **Chia sẻ Tài liệu An toàn & Quản lý Vòng đời**<br>- Lập trình chức năng chia sẻ có kiểm soát, tạo ra các đường link Pre-signed URL có giới hạn thời gian.<br>- Triển khai cơ chế Soft Delete, đưa tài liệu vào trạng thái Thùng rác và cho phép khôi phục trong 30 ngày. | 29/07/2026 | 30/07/2026 |
| **4** | **Tự động Xóa vĩnh viễn (Hard Delete)**<br>- Cấu hình thuộc tính TTL trên DynamoDB để hệ thống tự động xóa vĩnh viễn các tài liệu quá hạn mà không cần can thiệp thủ công. | 31/07/2026 | 31/07/2026 |
| **5** | **Hạ tầng bằng Code & Triển khai Đám mây (AWS SAM)**<br>- Viết file `template.yaml` bằng AWS SAM để khai báo toàn bộ Lambda, API Gateway và DynamoDB.<br>- Thực thi lệnh build và deploy của SAM để tự động khởi tạo toàn bộ kiến trúc EDMS lên AWS Cloud. | 01/08/2026 | 02/08/2026 |

### Kết quả đạt được tuần 6:

* **Mở rộng Module & Tự động hóa Nghiệp vụ:** Hoàn thiện các module chức năng mở rộng, điều phối thành công các quy trình doanh nghiệp phức tạp thông qua Step Functions và SNS giúp người dùng nhận thông báo tức thì.
* **Triển khai Tối ưu:** Thay thế thao tác thủ công trên giao diện bằng AWS SAM. Toàn bộ dự án giờ đây có thể được khởi tạo hoặc gỡ bỏ chỉ trong vài phút bằng code, giảm thiểu tối đa chi phí vận hành và dọn dẹp hệ thống.
