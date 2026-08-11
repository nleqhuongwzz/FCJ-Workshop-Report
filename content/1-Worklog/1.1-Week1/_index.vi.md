---
title: "Worklog Tuần 1"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:
* Kết nối, làm quen với các thành viên trong chương trình First Cloud Journey (FCJ) 2026.
* Thiết lập môi trường làm việc AWS và hiểu cách quản trị tài nguyên qua Console & CLI.
* Phân tích yêu cầu dự án cuối khóa EDMS (Enterprise Document Management System) và lập kế hoạch làm việc cho nhóm.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - Nghiên cứu hệ sinh thái AWS Core Services (Compute, Storage, Networking, Database). <br> - Tìm hiểu cấu trúc `fcj-workshop-template` và yêu cầu bài báo cáo. | 21/06/2026 | 21/06/2026 | FCJ Documentation |
| 3 | - Tạo AWS Free Tier account và thiết lập bảo mật cơ bản (IAM/MFA). <br> - Tìm hiểu AWS Console & cài đặt AWS CLI. <br> - Thực hành cấu hình `aws configure`. | 22/06/2026 | 23/06/2026 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4 | - Nghiên cứu sâu về Serverless: Lambda & API Gateway. <br> - Tìm hiểu luồng xử lý sự kiện (Event-driven) trên AWS. | 24/06/2026 | 24/06/2026 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5 | - Thực hành các bài lab cơ bản về S3 Hosting và EC2 Provisioning. <br> - Kiểm thử kết nối giữa Local CLI và AWS Cloud. | 25/06/2026 | 25/06/2026 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Kick-off meeting nhóm 5 người. <br> - Thảo luận, thống nhất đề tài dự án EDMS và phân công vai trò (Architect, Frontend, Backend, Data, DevOps). | 26/06/2026 | 26/06/2026 | Internal Meeting |

### Kết quả đạt được tuần 1:

* **Kiến thức nền tảng:** Hiểu rõ các nhóm dịch vụ cốt lõi của AWS (Compute: EC2/Lambda, Storage: S3/EBS, Networking: VPC, Database: DynamoDB/RDS).
* **Môi trường làm việc:** Đã thiết lập thành công AWS Free Tier account, cấu hình AWS CLI với đầy đủ Access Key, Secret Key và Region mặc định.
* **Kỹ năng thực hành:** Biết cách sử dụng AWS Management Console để quản lý tài nguyên và sử dụng CLI để kiểm tra thông tin tài khoản (`sts get-caller-identity`), quản lý dịch vụ cơ bản.
* **Định hướng dự án:** Nhóm đã thống nhất được lộ trình phát triển cho dự án EDMS trong 4 tuần tới. Đã hoàn thiện việc chia nhóm và nắm rõ quy chuẩn báo cáo (song ngữ, cấu trúc workshop website).
* **Tư duy Cloud-Native:** Bắt đầu tiếp cận tư duy Serverless thông qua việc nghiên cứu Lambda & API Gateway – nền tảng cho hệ thống quản lý tài liệu mà nhóm sẽ thực hiện.