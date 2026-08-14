---
title: "Blog 3"
date: 2026-08-14
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Khám Phá Mô Hình Multi-Agent Tự Động "Đọc Code Ra Doc": Amazon Bedrock AgentCore Và MCP

Nếu có một công việc mà 99% kỹ sư phần mềm hay kỹ sư dữ liệu đều ngại làm, thì đó chính là viết và cập nhật tài liệu. Code thì đã refactor sang phiên bản mới, API đã thêm 5 tham số, nhưng cuốn Wiki trên Confluence hay Notion thì vẫn dừng lại ở câu chuyện của... năm ngoái. Tài liệu lỗi thời còn nguy hiểm hơn không có tài liệu, vì nó trực tiếp làm tốn thời gian của các thành viên mới và gây ra vô số hiểu lầm khi bàn giao hệ thống.

Vừa qua, mình có dành thời gian tìm hiểu một giải pháp cực kỳ thú vị từ hệ sinh thái AWS: Sử dụng kiến trúc Multi-Agent kết hợp **Amazon Bedrock AgentCore** và giao thức **MCP** để tự động tạo và bảo trì tài liệu kỹ thuật theo thời gian thực.

## 1. TẠI SAO BÀI TOÁN NÀY LẠI CẦN MULTI-AGENT?

Nếu chỉ dùng một Chatbot AI thông thường và quăng cả folder code vào, bạn sẽ nhận lại một đoạn tóm tắt rất chung chung và hay bị hụt ngữ cảnh. Để tạo ra một bộ tài liệu chuẩn chỉnh cho doanh nghiệp, hệ thống cần chia nhỏ công việc cho các đại lý chuyên biệt:

- **Code Analyzer Agent**: Đọc cấu trúc thư mục, phân tích luồng chạy của hàm và trích xuất các endpoint API/Data Pipeline.
- **Architecture Diagram Agent**: Tự động đọc cấu trúc hạ tầng/code để vẽ lại sơ đồ luồng (Flowchart, Sequence Diagram) dưới dạng mã Mermaid.js.
- **Technical Writer Agent**: Tổng hợp thông tin, viết lại bằng văn phong chuẩn mực, dễ hiểu cho cả Dev lẫn Product Owner.
- **Doc Sync Agent**: So sánh tài liệu hiện có với code mới nhất trong Pull Request để chỉ cập nhật đúng những phần có sự thay đổi (Delta update).

## 2. ĐIỂM SÁNG TỪ BỘ CÔNG CỤ AMAZON BEDROCK AGENTCORE

Khi tìm hiểu sâu vào cách triển khai trên AWS, mình thấy 3 thành phần này giải quyết rất mượt các rào cản kỹ thuật:

- **Kết nối đa nền tảng qua AgentCore Gateway (MCP)**: Nhờ chuẩn Model Context Protocol (MCP), các Agent có thể vừa đọc từ GitHub/GitLab, vừa viết trực tiếp vào Notion, Confluence hay MkDocs mà không cần viết các đoạn code tích hợp rườm rà.
- **Ghi nhớ ngữ cảnh nhờ AgentCore Memory**: AI không viết lại tài liệu từ đầu mỗi khi có commit mới. Nó nhớ cấu trúc tài liệu cũ và chỉ bổ sung/chỉnh sửa những phần code thực sự có thay đổi.
- **An toàn tuyệt đối cho Codebase**: Toàn bộ quá trình đọc code diễn ra trong môi trường isolated của Bedrock Runtime. Code nội bộ của công ty hoàn toàn không bị rò rỉ hay bị dùng để huấn luyện mô hình công cộng.

## 3. GÓC NHÌN RÚT RA KHI TÌM HIỂU CHỦ ĐỀ NÀY

Điều mình thích nhất ở mô hình này là nó đổi mới hoàn toàn tư duy làm tài liệu: Từ tài liệu tĩnh (Static Doc) sang tài liệu sống (Living Doc).

Tài liệu giờ đây trở thành một phần của CI/CD Pipeline. Khi Dev gộp code (Merge PR), AI Agent sẽ tự động chạy ngầm, rà soát thay đổi và gửi một Pull Request cập nhật lại file README.md hoặc trang Confluence tương ứng. Lập trình viên chỉ việc bấm "Approve" là xong.

## TÀI LIỆU THAM KHẢO

- AWS Agentic Workflows: Amazon Web Services (2025). _Building Living Documentation Pipelines using Amazon Bedrock AgentCore._ AWS Architecture Center.
- Giao thức & Framework: Anthropic (2024). _Model Context Protocol (MCP): Connecting AI Models to Enterprise Knowledge Bases & Developer Tools._
- LangChain / LangGraph Docs. _Multi-Agent Orchestration for Code Graph Analysis._

Thành phố Hồ Chí Minh, tháng 8 năm 2026
Huỳnh Minh Quân

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/multi_permalinks/2234417337323226/)
