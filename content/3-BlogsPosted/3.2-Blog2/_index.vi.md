---
title: "Blog 2"
date: 2026-08-14
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# GIẢI QUYẾT BÀI TOÁN TRAFFIC BIẾN ĐỘNG TRONG GAME VỚI AMAZON DOCUMENTDB SERVERLESS

Làm game, đặc biệt là mấy thể loại chiến thuật hay MOBA, ai cũng mong sản phẩm của mình đông người chơi. Nhưng sự thật phũ phàng là lúc traffic tăng đột biến thì database rất dễ bị nghẽn cổ chai và lăn ra chết. Nếu cấp phát tài nguyên dư thừa thì tốn tiền vận hành những lúc vắng khách, mà cấp ít thì server sập giữa chừng làm trải nghiệm người chơi tệ đi. Gần đây mình có mò mẫm các kiến trúc hạ tầng trên AWS để giải bài toán này và thấy **Amazon DocumentDB Serverless** là một giải pháp cực kỳ đáng thử.

## TẠI SAO BÀI TOÁN NÀY LẠI CẦN SERVERLESS DATABASE?

Nếu dùng một database truyền thống hoặc tự host, anh em sẽ phải liên tục canh metric và tự scale bằng tay hoặc cài đặt các rule auto-scale rất mệt mỏi. Với bản chất của Serverless Database, hệ thống sẽ tự động scale công suất lên xuống theo đúng nhu cầu thực tế của ứng dụng.

Những lúc diễn ra event giờ vàng traffic có thể x10 x100, database tự động phình ra gánh tải mà không cần ai phải túc trực can thiệp. Thay vì tốn thời gian loay hoay config server hay backup, anh em dev có thể dồn 100% công lực vào việc viết logic game hoặc tối ưu luồng chạy.

## ĐIỂM SÁNG TỪ AMAZON DOCUMENTDB SERVERLESS

Khi tìm hiểu sâu vào cách triển khai trên AWS, mình thấy có 3 điểm giải quyết rất mượt các rào cản kỹ thuật:

- **Xài bao nhiêu trả bấy nhiêu**: Nó không tính phí theo dung lượng server bạn thuê cố định, mà dựa trên tài nguyên thực sự tiêu thụ. Hết event, ít người chơi, hệ thống tự động thu nhỏ lại về mức tối thiểu, tiết kiệm được một khoản chi phí hạ tầng khổng lồ.
- **Tương thích mượt mà với MongoDB**: Đây là điểm mình ưng nhất. Ở các project đòi hỏi tính năng realtime như ứng dụng chat, mình hay dùng stack Node.js kết hợp ExpressJS và MongoDB. Sang hệ sinh thái này thì gần như không phải đập đi viết lại code hay đổi driver. Chỉ cần sửa đúng cái connection string là hệ thống chạy bình thường.
- **Độ an toàn dữ liệu cao**: Dữ liệu được tự động phân tán trên 3 Availability Zones khác nhau. Game của bạn không lo bị rollback hay mất mát dữ liệu người chơi nếu lỡ có một data center nào đó gặp sự cố vật lý.

## GÓC NHÌN RÚT RA KHI TÌM HIỂU CHỦ ĐỀ NÀY

Điều mình thích nhất ở kiến trúc Serverless không chỉ nằm ở công nghệ, mà là sự thay đổi về tư duy thiết kế hệ thống. Thay vì tư duy dự đoán mức tải và chuẩn bị sẵn tài nguyên từ trước, chúng ta chuyển sang hướng linh hoạt và đáp ứng tức thời.

Trước đây khi làm các project yêu cầu kết nối liên tục, việc database bị quá tải luôn là nỗi ám ảnh. Việc đẩy phần vận hành khó nhằn này cho một managed service như DocumentDB Serverless giúp kỹ sư rảnh tay hơn hẳn, không lo sập server mỗi đợt release tính năng lớn.

## TÀI LIỆU THAM KHẢO

- AWS for Games Blog. _Game developer's guide to Amazon DocumentDB Serverless._
- AWS Documentation. _Amazon DocumentDB Serverless._

Thành phố Hồ Chí Minh, tháng 8 năm 2026
Huỳnh Phúc Hưng

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2238353500262943/)
