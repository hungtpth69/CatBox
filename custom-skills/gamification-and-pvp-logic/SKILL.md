---
name: gamification-and-pvp-logic
description: Sử dụng khi thiết kế luật chơi, kinh tế game (Coin shop), tính điểm, level, và logic đối kháng PVP.
---

# Kỹ năng Thiết kế Gamification & PVP Logic

## Nguyên tắc cốt lõi
Hệ thống phải tạo được tính gây nghiện (Hook), cân bằng kinh tế (Economy Balance), và công bằng tuyệt đối trong thi đấu đối kháng (PVP).

## Checklist & Yêu cầu Kỹ thuật
- [ ] **Hệ thống Level & Roadmap:**
  - Logic mở khóa level: Phải kiểm tra điều kiện khắt khe (VD: Yêu cầu tối thiểu 1 sao ở level trước thì mới cho truy cập level sau).
- [ ] **PVP Logic (Luật đối kháng):**
  - **Đồng bộ thời gian (Time-sync):** Thời gian đếm ngược (Countdown) cho các câu hỏi bắt buộc phải dựa vào **Server Time**, tuyệt đối không dùng Local Time của thiết bị để tránh gian lận.
  - **Xử lý Edge Cases:** Phải thiết kế kịch bản rõ ràng cho các trường hợp: (1) Một bên ngắt kết nối mạng (Disconnect), (2) Người chơi cố tình thoát app giữa chừng (Quit), (3) Hết thời gian chờ mà chưa bắt cặp được (Matchmaking timeout).
- [ ] **Coin Economy (Kinh tế tiền tệ):**
  - Tách bạch rõ ràng nguồn thu Coin (thưởng từ bài học, thắng PVP) và nguồn xả Coin (mua Avatar, trợ giúp trong PVP).
  - Thiết kế chống lạm phát.
