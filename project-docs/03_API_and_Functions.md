# Tài liệu API & Serverless Functions

Tài liệu này đặc tả các Callable Functions (Firebase Cloud Functions) dùng để giao tiếp giữa Flutter Client và Server.

## 1. Tiêu chuẩn Giao tiếp & Zero-Trust
1. Toàn bộ Cloud Functions sử dụng giao thức `onCall` (Callable Functions), đảm bảo Context của User (UID, Auth Token) luôn được Firebase tự động đính kèm và verify.
2. Client không gọi REST trực tiếp mà gọi qua thư viện `cloud_functions` của Flutter.
3. Server không bao giờ tin tưởng Parameter `coin` hoặc `score` từ Client gửi lên.

---

## 2. API Endpoints

### 2.1. `findPvpMatch` (Bắt cặp trận đấu)
- **Mô tả:** Đẩy người chơi vào hàng chờ ghép cặp (Matchmaking Queue).
- **Request Body:**
  ```json
  {
    "mapLevel": "hsk1_level1" // Để ghép những người học chung một bài
  }
  ```
- **Response:**
  ```json
  {
    "status": "success",
    "message": "Đã vào hàng đợi. Lắng nghe RTDB tại đường dẫn /matchmaking/{uid}"
  }
  ```

### 2.2. `submitPvpAnswer` (Nộp đáp án PVP)
- **Mô tả:** Gửi đáp án cho một câu hỏi trong trận PVP. Server sẽ chấm điểm dựa trên độ nhanh nhạy.
- **Request Body:**
  ```json
  {
    "roomId": "room_xyz123",
    "questionIndex": 2,
    "answer": "B" // Đáp án người chơi chọn
  }
  ```
- **Xử lý tại Server (Logic):**
  1. Lấy thông tin câu hỏi từ Firestore để biết đáp án đúng.
  2. Lấy `ServerValue.TIMESTAMP` so sánh với thời gian kết thúc (`timeEnd`) để tính điểm. (Trả lời càng sớm, điểm càng cao).
  3. Cập nhật điểm của người chơi vào RTDB `/pvp_rooms/{roomId}`.
- **Response:**
  ```json
  {
    "isCorrect": true,
    "pointsEarned": 120
  }
  ```

### 2.3. `purchaseItem` (Mua vật phẩm trong Shop)
- **Mô tả:** Trừ số dư Coin để mua vật phẩm hỗ trợ. Áp dụng Transaction để chống Race-condition (Click 2 lần cùng lúc).
- **Request Body:**
  ```json
  {
    "itemId": "item_freeze_time",
    "quantity": 1
  }
  ```
- **Xử lý tại Server:** Dùng Firestore Transaction kiểm tra `coinBalance >= item.price * quantity`. Nếu đủ, trừ tiền và cộng item vào `inventory`.
- **Response:**
  ```json
  {
    "status": "success",
    "newCoinBalance": 1350
  }
  ```

### 2.4. `claimLevelReward` (Nhận thưởng khi qua màn)
- **Mô tả:** Khi qua một Node trên bản đồ, Client báo cáo kết quả. Server tính số sao và cộng Coin.
- **Request Body:**
  ```json
  {
    "levelId": "hsk1_level1",
    "starsEarned": 3,
    "mistakes": 1
  }
  ```
- **Response:**
  ```json
  {
    "rewardCoin": 50,
    "bonusCoin": 10 // Bonus do làm sai ít
  }
  ```
