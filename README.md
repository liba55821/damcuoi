# Tiệc Cưới Online Pro - Đám Cưới Livestream (Render Deployment Guide)

Ứng dụng tương tác đám cưới trên livestream TikTok (kết nối TikFinity).

---

## 📁 Cấu Trúc Dự Án
- `app/server.cjs`: Backend Node.js phục vụ API, SSE, và nhận Webhook.
- `app/public/`: Giao diện Bảng điều khiển (`index.html`) & Màn hình LIVE (`overlay.html`).
- `app/wedding_state.json`: File lưu trữ trạng thái tiệc cưới (khách mời, cô dâu, chú rể...).
- `package.json`: Cấu hình khởi chạy trên Render / máy chủ cloud.
- `render.yaml`: Blueprint cấu hình tự động cho Render.com.
- `BAT_DAU.bat`: Dành cho việc chạy offline trên máy tính Windows cá nhân.

---

## 🚀 Hướng Dẫn Đưa Lên Render.com

### Bước 1: Chuẩn bị mã nguồn lên GitHub
1. Truy cập [github.com](https://github.com) và tạo một Repository mới (ví dụ đặt tên: `dam-cuoi-live`).
2. Tải toàn bộ các file trong thư mục này lên GitHub (sử dụng Git CLI, GitHub Desktop hoặc nút **Upload files** trên GitHub).
   > **LƯU Ý QUAN TRỌNG:** KHÔNG tải thư mục `runtime/` lên GitHub (vì file `node.exe` 91MB chỉ dành cho Windows offline, đẩy lên sẽ làm nặng repo và gây lỗi). File `.gitignore` đã được cấu hình sẵn để tự loại trừ thư mục này.

### Bước 2: Tạo dịch vụ trên Render.com
1. Đăng ký/Đăng nhập tài khoản miễn phí tại [render.com](https://render.com).
2. Tại trang Dashboard, chọn **New +** → Chọn **Web Service**.
3. Chọn **Build and deploy from a Git repository** → Kết nối với repository GitHub bạn vừa tạo ở Bước 1.
4. Điền các thông số:
   - **Name:** `dam-cuoi-live` (hoặc tên tùy thích)
   - **Region:** `Singapore` (để mạng từ Việt Nam truy cập nhanh nhất)
   - **Branch:** `main` (hoặc `master`)
   - **Root Directory:** Để trống (hoặc gõ `app`)
   - **Runtime:** `Node`
   - **Build Command:** Để trống (hoặc `npm install`)
   - **Start Command:** `node app/server.cjs` (hoặc `npm start`)
   - **Plan:** `Free`
5. Mục **Environment Variables** (Tùy chọn):
   - Thêm biến `NODE_VERSION` với giá trị `22`
6. Bấm nút **Create Web Service**. Đợi 1-2 phút, Render sẽ cấp cho bạn một đường link HTTPS (ví dụ: `https://dam-cuoi-live.onrender.com`).

---

## 🔗 Cách Sử Dụng Sau Khi Đưa Lên Render
- **Bảng điều khiển:** `https://<ten-mien-cua-ban>.onrender.com`
- **Màn hình LIVE (Overlay cho OBS / TikTok Studio):** `https://<ten-mien-cua-ban>.onrender.com/overlay.html`
- **Cấu hình TikFinity Desktop kết nối lên Render:**
  1. Mở phần mềm TikFinity Desktop trên máy tính.
  2. Vào mục **Webhook**.
  3. Bật **Enable Webhook**.
  4. Dán URL: `https://<ten-mien-cua-ban>.onrender.com/api/tikfinity`
  5. Tích chọn **Chat** và **Gift** rồi bấm **Save**.
  > Mọi lượt tặng quà và bình luận từ TikTok Live sẽ tự động được gửi lên máy chủ Render thời gian thực!
