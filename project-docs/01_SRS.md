# Đặc tả Yêu cầu Phần mềm (Software Requirements Specification - SRS)

## 1. Business Context (Ngữ cảnh Kinh doanh)
- **Tầm nhìn sản phẩm:** Ứng dụng học tiếng Trung kết hợp cơ chế Gamification (trò chơi hóa). Giúp biến việc nạp từ vựng khô khan thành một trải nghiệm giải trí liên tục, kích thích nỗ lực học tập thông qua cạnh tranh và khen thưởng.
- **Chân dung người dùng (Personas):**
  - Người mới học tiếng Trung muốn có một cách học đỡ nhàm chán.
  - Người đi làm bận rộn, chỉ có 10-15 phút rảnh rỗi trên xe bus/tàu điện cần học theo session ngắn.
- **Core Game Loop:** Học Bài ➡️ Thu thập Coin ➡️ Mở khóa Bản đồ ➡️ Mua sắm Avatar/Vật phẩm ➡️ Thi đấu PVP ➡️ Rớt hạng/Tăng hạng ➡️ Quay lại Học Bài.

---

## 2. Phạm vi Phase 1 (Scope)
- **Mục tiêu:** Phase 1 sẽ chỉ tập trung vào việc **Học Từ vựng (Vocabulary)** để hoàn thiện Core Game Loop.
- **Quy chuẩn logic cốt lõi:**
  1. **Giới thiệu từ mới:** Mỗi từ vựng (vocab) sẽ được miêu tả qua **4 Flashcard** trước khi đi vào phần học (Learn).
  2. **Phần Learn:** Bao gồm các câu hỏi thực hành liên quan đến từ vựng vừa xem trên Flashcard.
  3. **Phần Battle:** Thi đấu cho 1 từ vựng sẽ bao gồm đúng **4 câu hỏi**.
  4. **Ôn tập (Review):** Khi sang màn (level) tiếp theo, sẽ có cơ chế tự động ôn lại từ vựng cũ (Thuật toán ôn tập chi tiết sẽ được bổ sung sau).

---

## 3. User Stories & Acceptance Criteria

### Epic 1: Xác thực & Định danh (Identity)
- **Story:** Là người dùng mới, tôi muốn chơi thử ngay mà không cần tạo tài khoản để trải nghiệm app nhanh nhất.
  - **Acceptance Criteria (AC):**
    - [ ] App cung cấp nút "Chơi ngay" sinh ra một UID dạng Guest (Anonymous Auth).
    - [ ] Dữ liệu (Coin, Level) của Guest được lưu trữ bình thường như User.
- **Story:** Là người dùng Guest, tôi muốn liên kết tài khoản Gmail/Facebook để không mất dữ liệu khi đổi điện thoại.
  - **AC:**
    - [ ] Khi chọn "Liên kết tài khoản", Firebase Auth thực hiện `linkWithCredential`.
    - [ ] Mọi UID và dữ liệu cũ phải được giữ nguyên.

### Epic 2: Học tập & Flashcard (Learning - Phase 1)
- **Story:** Là học viên, tôi muốn học từ vựng thông qua các thẻ Flashcard có phát âm và hình ảnh trước khi thực hành.
  - **AC:**
    - [ ] **Quy trình tiếp cận:** Mỗi từ vựng mới phải đi qua **4 thẻ Flashcard** miêu tả (chữ Hán, Pinyin, hình ảnh, âm thanh) trước khi bước vào phần Learn.
    - [ ] **Phần Learn:** Sau Flashcard, hệ thống hiển thị các câu hỏi thực hành để kiểm tra từ vựng vừa học.
    - [ ] **Cơ chế ôn tập (Review):** Khi chuyển sang màn (level) tiếp theo, hệ thống tự động chèn cơ chế ôn lại các từ vựng cũ.

### Epic 3: Bản đồ Tiến trình (Roadmap)
- **Story:** Là người chơi, tôi muốn thấy sự tiến bộ của mình hiển thị trên một bản đồ sinh động.
  - **AC:**
    - [ ] Màn hình chính là Bản đồ học tập (Roadmap) dạng cuộn dọc.
    - [ ] Các chặng (Levels) được nối với nhau bằng đường cong.
    - [ ] Yêu cầu vượt qua Node N với tối thiểu 1 Sao mới được chơi Node N+1.

### Epic 4: Giao đấu PVP (Battle - Phase 1)
- **Story:** Là người chơi hiếu thắng, tôi muốn thi đấu kiến thức trực tiếp với người khác để cướp Coin.
  - **AC:**
    - [ ] **Matchmaking:** Bấm tìm trận, hệ thống tự ghép cặp với người cùng trình độ (Elo) trong vòng 5 giây. Nếu quá 5 giây sẽ ghép với Bot.
    - [ ] **Quy chuẩn Battle:** Một trận thi đấu sẽ xoay quanh 1 từ vựng (vocab) và bao gồm đúng **4 câu hỏi**. Mỗi câu đếm ngược x giây (Server time). 
    - [ ] **Luật tính điểm:** Trả lời đúng VÀ nhanh sẽ được nhiều điểm hơn (Ví dụ: 100 điểm cho đúng + 10 x số giây còn lại).
    - [ ] **Xử lý ngắt kết nối:** Nếu người dùng thoát app giữa trận, lập tức xử thua 0 điểm.

### Epic 5: Kinh tế & Cửa hàng (Economy)
- **Story:** Là người chơi, tôi muốn dùng Coin kiếm được để trang bị cho nhân vật.
  - **AC:**
    - [ ] Cửa hàng bán các vật phẩm trợ giúp (Ví dụ: Xóa 2 đáp án sai trong PVP).
    - [ ] Không cho phép số dư Coin bị âm. Giao dịch mua sắm phải được xử lý bảo mật trên Server.

---

## 4. Non-Functional Requirements (Yêu cầu phi chức năng)
1. **Hiệu năng UI (Performance):** Ứng dụng phải luôn duy trì ở mức **60FPS hoặc 120FPS** trên các thiết bị đời mới. Tuyệt đối không giật lag khi cuộn bản đồ.
2. **Hỗ trợ Offline (Offline-first):** Các gói bài học (Flashcards, Audio) phải được tải sẵn về Cache (Hive/Isar) để người dùng học khi không có kết nối mạng Internet. Khi có mạng, tự động Sync dữ liệu (Coin, tiến độ) lên Firebase.
3. **Bảo mật (Security):** Toàn bộ API thay đổi trạng thái nhạy cảm (Cộng/Trừ Coin, Ghi nhận kết quả PVP) phải có Token và Anti-cheat mechanism.
