---
name: mobile-ui-ux-guidelines
description: Bắt buộc sử dụng khi thiết kế hoặc code giao diện (UI) và trải nghiệm (UX) cho app mobile, đặc biệt là các màn hình Roadmap, Flashcard, PVP.
---

# Kỹ năng Tiêu chuẩn UI/UX Mobile (App Học Tiếng Trung)

## Nguyên tắc cốt lõi
Ứng dụng học tập này mang hơi hướm của một Game. Tuyệt đối **KHÔNG** code giao diện khô khan như các ứng dụng quản lý/ngân hàng. Phải tạo cảm giác tương tác sống động, có tính chất Gamification cao.

## Checklist & Yêu cầu Kỹ thuật
- [ ] **Micro-interactions (Tương tác nhỏ):** Bắt buộc tích hợp animation vào các tương tác của user (bấm nút, trả lời đúng/sai, nhận thưởng). Khuyến khích sử dụng các thư viện animation mạnh mẽ (VD: Lottie, Rive, Reanimated).
- [ ] **Roadmap UI (Bản đồ tiến trình):** 
  - Giao diện phải tương tự dạng bản đồ Candy Crush.
  - Bắt buộc dùng `Canvas` hoặc `SVG/Path` để vẽ đường nối (lines) giữa các node (level).
  - Phải có trạng thái (States) rõ ràng: Khóa (Locked), Đang chơi (Current), Đã hoàn thành (Completed - kèm số sao đạt được).
- [ ] **Flashcard UI:**
  - Bắt buộc hỗ trợ thao tác vuốt (Swipe left/right) dạng Tinder hoặc lật thẻ 3D (Flip).
  - Hoạt ảnh phải đạt 60fps, không được giật lag.
- [ ] **Trạng thái Trống/Lỗi (Empty/Error states):** Mọi màn hình trống hoặc lỗi mạng phải có hình minh họa dễ thương (Mascot) kèm câu chữ thân thiện.
