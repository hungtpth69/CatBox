# Quy chuẩn Viết Code & Gamification UI (Flutter)

Tài liệu này ép khuôn các kỹ sư Frontend (Đặc biệt là `mobile_ui_specialist`) tuân thủ các nguyên tắc thiết kế Flutter cực hạn để đảm bảo app mượt mà và dễ bảo trì.

## 1. Cấu trúc Thư mục (Feature-First Architecture)
Sử dụng kiến trúc chia theo Feature (Tính năng) để dự án không bị biến thành một "đống bùi nhùi" (spaghetti code) khi scale lớn.

```text
lib/
├── core/                   # Code dùng chung (Network, Theme, Utils, Di)
│   ├── theme/              # Design Tokens (Colors, Typography)
│   ├── errors/             # Failure, Exception handlers
│   └── constants/          # App constants
├── features/               # Chia nhỏ theo chức năng
│   ├── auth/               # Module Xác thực
│   │   ├── domain/         # Entities, Repositories Interfaces
│   │   ├── data/           # Models, Data Sources (Firebase)
│   │   └── presentation/   # UI (Pages, Widgets, State)
│   ├── map_roadmap/        # Module Bản đồ
│   ├── flashcard/          # Module Học tập
│   ├── pvp_battle/         # Module Đối kháng
│   └── shop/               # Module Cửa hàng
└── main.dart
```

---

## 2. Tiêu chuẩn Quản lý Trạng thái (State Management)
**Bắt buộc sử dụng: BLoC hoặc Riverpod (Khuyến nghị BLoC cho app lớn).**
- **Cấm kỵ:** Không dùng `setState()` ở cấp độ Màn hình (Screen) hoặc các Component vẽ Bản đồ có kích thước lớn. Khi dùng `setState`, Flutter sẽ vẽ lại (re-build) toàn bộ cây Widget, gây giật lag nghiêm trọng ở thiết bị yếu.
- **Thực hành đúng:** Chỉ rebuild các Widget thật sự cần thay đổi (dùng `BlocBuilder` bao bọc vùng nhỏ, hoặc `Selector`).

---

## 3. Tiêu chuẩn Gamification UI (Nghiêm ngặt)

### 3.1. Vẽ Bản đồ Roadmap (The Candy Crush Map)
- Bắt buộc dùng `CustomPainter` để vẽ bản đồ. Các Node (Level) phải được tính toán tọa độ (x, y) thông qua hàm toán học (Sine wave, Bezier curve) thay vì fix cứng.
- Tuyệt đối không dùng `ListView` hay `Column` lồng nhau để xếp vị trí các điểm trên bản đồ.

### 3.2. Micro-Interactions (Hoạt ảnh tương tác nhỏ)
- **Tích hợp Rive / Lottie:** Các hiệu ứng như pháo hoa nổ khi hoàn thành bài học, hộp quà rung lên, linh vật (Mascot) giật mình, bắt buộc phải dùng file `.riv` (Rive) hoặc `.json` (Lottie). 
- *Lý do:* Nhỏ nhẹ, render bằng phần cứng, không tốn resource của CPU điện thoại.

### 3.3. Flashcard Flip & Swipe (Lật và Vuốt thẻ)
- Khi lật thẻ 3D: Phải cấu hình `Transform(transform: Matrix4.identity()..setEntry(3, 2, 0.001)..rotateY(angle))` để tạo chiều sâu 3D (Perspective).
- Khi vuốt thẻ sang trái/phải: Bắt buộc dùng `GestureDetector` kết hợp với `AnimationController` mượt mà, kèm theo tính toán ma trận (Matrix) để nghiêng thẻ một góc nhỏ khi vuốt (giống Tinder swipe).

---

## 4. Anti-Patterns (Cấm kỵ trong code)
- ❌ **Main Thread Blocking (Treo UI):** Cấm chạy vòng lặp xử lý logic lớn (Ví dụ: Parse file JSON từ điển 100MB) trên Main Isolate. Phải dùng `compute()` hoặc `Isolate.run()` để đẩy việc nặng sang Thread khác, giữ cho UI luôn đạt 60FPS.
- ❌ **Magic Strings/Numbers:** Cấm nhét thẳng text "Bạn đã thắng" hay size thẻ "120.0" vào code UI. Mọi text phải nằm ở file Localization/i18n, mọi size phải nằm ở Design Tokens (Vd: `AppSizes.cardWidth`).
- ❌ **Quên Firebase Presence:** Khi code PVP, nếu quên tích hợp `onDisconnect()` của Realtime Database, user rớt mạng sẽ trở thành "Bóng ma" (Ghost) trên phòng đấu và hệ thống bị treo. Bắt buộc phải code logic thu dọn phòng khi ngắt mạng.
