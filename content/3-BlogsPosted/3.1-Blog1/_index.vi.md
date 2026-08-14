---
title: "Blog 1"
date: 2026-08-14
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# XÂY DỰNG HỆ THỐNG NÔNG NGHIỆP THÔNG MINH (SMART AGRICULTURE) VỚI KIẾN TRÚC MULTI-AGENT TRÊN AWS IOT GREENGRASS

Nếu có một lĩnh vực mà việc triển khai công nghệ gặp nhiều rào cản vật lý nhất, thì đó chính là Nông nghiệp thông minh (Smart Agriculture). Nông trại thường nằm ở những khu vực hẻo lánh, internet chập chờn, trong khi dữ liệu từ cảm biến (độ ẩm đất, nhiệt độ) hay camera (phân tích tình trạng cây trồng) lại đòi hỏi phải được xử lý real-time để ra quyết định tưới tiêu ngay lập tức. Đẩy hết dữ liệu thô lên Cloud rồi mới xử lý là một bài toán đốt tiền và có độ trễ lớn (latency).

Vừa qua, mình có dành thời gian tìm hiểu một giải pháp cực kỳ thú vị từ hệ sinh thái AWS: Sử dụng kiến trúc AI Agent kết hợp với AWS IoT Greengrass để mang thẳng khả năng suy luận của AI xuống thiết bị biên (Edge Device) — cụ thể ở đây là một bo mạch Raspberry Pi 5.

## TẠI SAO BÀI TOÁN NÀY LẠI CẦN ĐƯA AI XUỐNG EDGE DEVICE (EDGE AI)?

Nếu chỉ dùng các board mạch vi điều khiển (MCU) thông thường để đọc cảm biến độ ẩm rồi viết logic if-else (ví dụ: `if độ_ẩm < 30% then bật_bơm()`) thì hệ thống sẽ rất cứng nhắc và không thể phân tích hình ảnh bệnh lý của cây trồng. Tuy nhiên, nếu áp dụng mô hình Agentic Workflow ngay tại thiết bị biên, hệ thống có thể chia nhỏ công việc cho các đại lý chuyên biệt:

- **Camera/Vision Agent**: Tự động chụp ảnh cây trồng, phân tích tình trạng sức khỏe lá, sâu bệnh thông qua Amazon Bedrock (sử dụng các mô hình vision).
- **Sensor Agent**: Liên tục đọc dữ liệu real-time từ các cảm biến độ ẩm, nhiệt độ đất.
- **Orchestrator Agent**: Tổng hợp dữ liệu từ cả Camera và Sensor, sau đó tự đưa ra quyết định có nên tưới nước hay không, lượng nước bao nhiêu là đủ.

## ĐIỂM SÁNG TỪ BỘ CÔNG CỤ AWS IOT GREENGRASS & STRANDS AGENTS

Khi tìm hiểu sâu vào cách triển khai kiến trúc này trên AWS IoT Greengrass, mình thấy có 3 thành phần giải quyết rất mượt các rào cản kỹ thuật:

- **Chạy AI Offline/Local (Local Processing)**: Nhờ AWS IoT Greengrass, các component của AI Agent (cụ thể là Strands Agents) có thể chạy trực tiếp trên Raspberry Pi. Dù nông trại có rớt mạng, AI vẫn có thể đọc cảm biến và ra quyết định tưới tiêu tự động.
- **Quản lý vòng đời thiết bị (Device Management)**: Việc deploy code, cập nhật mô hình AI hay thay đổi logic cho hàng ngàn thiết bị Raspberry Pi ở các nông trại khác nhau được thực hiện tự động qua Greengrass (Over-The-Air updates) mà không cần kỹ sư phải lặn lội xuống tận nơi cắm cáp.
- **Tích hợp đa nền tảng linh hoạt**: Agent có thể vừa giao tiếp với phần cứng (GPIO pins để bật tắt bơm nước), vừa có thể dựng một Local Web Dashboard để người nông dân theo dõi trực tiếp tại trang trại.

## GÓC NHÌN RÚT RA KHI TÌM HIỂU CHỦ ĐỀ NÀY

Điều mình thích nhất ở kiến trúc này là nó định nghĩa lại cách chúng ta làm IoT: Từ những thiết bị thu thập dữ liệu thụ động (Dumb Sensors) sang những thiết bị có khả năng tự suy luận và hành động độc lập (Autonomous Edge AI).

Hệ thống giờ đây không chỉ biết "báo cáo" độ ẩm đất là 20%, mà còn biết "nhìn" vào cái cây đang héo, tự đánh giá tổng thể và quyết định bơm nước ngay lập tức. Đây thực sự là bước chuyển mình rất lớn của việc ứng dụng AI vào thực tế sản xuất.

## TÀI LIỆU THAM KHẢO

- AWS Internet of Things Blog. _Build smart agriculture with AWS IoT Greengrass and Strands Agents._
- Amazon Web Services. _AWS IoT Greengrass Developer Guide._

Thành phố Hồ Chí Minh, tháng 8 năm 2026
Huỳnh Phúc Hưng

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2240841880014105/)
