---
name: firebase-realtime-architecture
description: Bắt buộc sử dụng khi code logic liên quan đến Backend, Database, Auth, và bảo mật (Firebase).
---

# Kỹ năng Kiến trúc Firebase Realtime & Backend

## Nguyên tắc cốt lõi
Dự án có cơ chế kinh tế (Coin) và đối kháng (PVP). Nguyên tắc sống còn là: **Client (Mobile App) luôn có thể bị hack. Không bao giờ tin tưởng Client.**

## Checklist & Yêu cầu Kỹ thuật
- [ ] **Cloud Functions (Logic kinh doanh):**
  - Mọi thao tác làm thay đổi số dư Coin, Kinh nghiệm (EXP), hoặc kết quả trận đấu PVP **bắt buộc** phải được tính toán và xử lý trên Firebase Cloud Functions. Client chỉ gửi yêu cầu (Request).
- [ ] **PVP Database Architecture:**
  - Để đảm bảo tốc độ Real-time độ trễ siêu thấp cho battle, ưu tiên sử dụng **Firebase Realtime Database (RTDB)** cho luồng in-game (đang chiến đấu).
  - Dữ liệu kết quả tĩnh (Lịch sử đấu, hồ sơ người chơi) lưu ở **Firestore**.
- [ ] **Security Rules (Quyền truy cập):**
  - Bắt buộc viết quy tắc bảo mật (Security Rules) chặt chẽ.
  - Ví dụ: User A chỉ được sửa thông tin của User A. User không tham gia room PVP thì không được đọc data của room đó.
- [ ] **Authentication (Đăng nhập):**
  - Tích hợp đăng nhập đa nền tảng (Facebook, Gmail) và chế độ Khách (Guest qua Anonymous Auth).
  - Bắt buộc phải thiết kế sẵn luồng "Merge account" (Chuyển data từ Guest sang Account chính thức khi họ quyết định đăng nhập).
