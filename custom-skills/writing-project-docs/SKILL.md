---
name: writing-project-docs
description: Bắt buộc sử dụng khi được yêu cầu viết, cập nhật hoặc thiết kế tài liệu dự án (SRS, Architecture, API, Guidelines...).
---

# Kỹ năng Viết Tài Liệu Dự Án (Writing Project Docs)

<EXTREMELY-IMPORTANT>
Tài liệu dự án là xương sống của phát triển phần mềm. Sự hời hợt trong tài liệu sẽ dẫn đến sai lệch khi code. Kỹ năng này ép AI phải đóng vai trò là một **Technical Writer / Systems Analyst** xuất sắc, tỉ mỉ và cấu trúc.
</EXTREMELY-IMPORTANT>

## Các loại tài liệu hỗ trợ (Document Types)

Khi bắt đầu, hãy xác định loại tài liệu cần viết thuộc nhóm nào dưới đây để áp dụng tiêu chuẩn tương ứng:

### 1. SRS (Software Requirements Specification - Đặc tả Yêu cầu)
- **Bắt buộc phải có:** Business Context, User Personas, Functional Requirements (được viết dưới dạng User Stories & Acceptance Criteria), Non-functional Requirements (Hiệu năng, Bảo mật, Mở rộng), Data Models sơ bộ.
- **Tiêu chuẩn:** Không dùng từ ngữ mơ hồ (vd: "hệ thống chạy nhanh", "bảo mật tốt"). Phải định lượng rõ ràng ("thời gian phản hồi < 200ms", "mật khẩu mã hóa bcrypt"). Bắt buộc có kịch bản xử lý lỗi (Edge cases / Unhappy paths).

### 2. Architecture & System Design (Kiến trúc Hệ thống)
- **Bắt buộc phải có:** Context Diagram (Tổng quan), Component Architecture (Các thành phần), Data Flow (Luồng dữ liệu), Database Schema cơ bản, Tech Stack. Quan trọng nhất là phần **Trade-offs** (Lý do chọn giải pháp công nghệ này thay vì giải pháp khác).
- **Tiêu chuẩn:** Khuyến khích sử dụng cú pháp `mermaid` để vẽ sơ đồ trực quan (Flowchart, Sequence Diagram, ER Diagram).

### 3. API Documentation (Tài liệu API)
- **Bắt buộc phải có:** Base URL, Authentication method (Bearer token, API Key...), Endpoints (Method + Path).
- **Chi tiết từng Endpoint:** Request (Headers, Body schema, Path/Query params), Response (Success 200, Errors 400/401/403/404/500). Bắt buộc có cURL examples và JSON mock response.

### 4. Coding Standards & Guidelines (Tiêu chuẩn Code)
- **Bắt buộc phải có:** Naming conventions (cách đặt tên biến, file), Folder structure (cấu trúc thư mục), Best practices, Anti-patterns (Những cách code tệ cần tránh), Code snippets minh họa (Gồm cả ví dụ DO và DON'T).

---

## Quy trình Thực thi (Workflow Checklist)

Khi được yêu cầu viết tài liệu, AI BẮT BUỘC tuân thủ 4 bước sau. Không được nhảy cóc:

- [ ] **Bước 1: Khảo sát (Context Gathering)**
  - Xác định mục tiêu của tài liệu và đối tượng độc giả (Dev, C-level, hay End-user).
  - Trích xuất thông tin từ cuộc hội thoại. Nếu thiếu thông tin quan trọng (ví dụ: mô hình kinh doanh chưa rõ, logic tính tiền chưa có), **bắt buộc phải hỏi lại người dùng. Tuyệt đối không tự bịa (hallucinate) nghiệp vụ.**

- [ ] **Bước 2: Trình bày Mục lục (Outline Approval)**
  - Phác thảo ra Table of Contents (Mục lục) chi tiết các phần sẽ có.
  - Dừng lại và xin feedback: *"Đây là cấu trúc tài liệu tôi dự định viết. Bạn có muốn điều chỉnh hay bổ sung phần nào trước khi tôi đi vào chi tiết không?"*

- [ ] **Bước 3: Soạn thảo Chi tiết (Drafting)**
  - Viết nội dung cho từng phần dựa trên mục lục đã chốt. 
  - Nếu tài liệu quá dài, hãy viết thành nhiều chunk (nhiều lần gửi) để tránh quá tải.
  - Bắt buộc sử dụng Markdown chuẩn: Dùng Bảng (Tables) cho dữ liệu có cấu trúc, Code blocks cho JSON/Snippet, Alerts (Tip, Note, Warning) để nhấn mạnh.

- [ ] **Bước 4: Lập chỉ mục (Indexing)**
  - Tài liệu viết xong không được để trôi nổi. Bắt buộc phải thêm link của file tài liệu mới vào một file `index.md` (hoặc `README.md`) tổng của dự án để dễ tra cứu.

---

## Anti-Patterns (Những điều cấm kỵ)

- ❌ **Tường chữ (Wall of text):** Viết một đoạn văn dài 20 dòng không xuống dòng khiến người đọc hoa mắt. => *Giải pháp: Chia nhỏ, dùng gạch đầu dòng (bullet points), in đậm các từ khóa.*
- ❌ **Thiếu Acceptance Criteria (Tiêu chí nghiệm thu):** Mô tả chức năng chung chung nhưng dev không biết thế nào là "xong". => *Giải pháp: Luôn có Checklist hoặc Given-When-Then cho mỗi tính năng.*
- ❌ **Happy Path Only:** Chỉ viết luồng thao tác đúng của user mà quên mất các trường hợp lỗi mạng, nhập sai dữ liệu, lỗi server.
- ❌ **Đoán mò nghiệp vụ:** Gặp một logic mập mờ và tự ý quyết định thay vì hỏi lại Product Owner (người dùng).
