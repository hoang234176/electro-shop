# 🛒 Electro Shop - Hệ Thống Cửa Hàng Điện Tử

Electro Shop là một nền tảng thương mại điện tử mua bán đồ điện tử (điện thoại, laptop, phụ kiện,...) được xây dựng với kiến trúc Full-stack. Dự án tích hợp các công nghệ hiện đại như thanh toán trực tuyến qua VNPay, lưu trữ ảnh trên Cloudinary và đặc biệt sử dụng AI Gemini để gợi ý sản phẩm thông minh.

## ✨ Tính năng nổi bật

- **Khám phá và mua sắm:** Xem chi tiết sản phẩm, lọc theo danh mục, quản lý giỏ hàng.
- **Gợi ý sản phẩm bằng AI (Gemini):** Tự động đề xuất các phụ kiện liên quan thông minh. Ví dụ: Khi xem điện thoại iPhone, AI sẽ tự động phân tích và đề xuất sạc, ốp lưng, tai nghe hoặc kính cường lực tương thích.
- **Thanh toán trực tuyến (VNPay):** Hỗ trợ thanh toán an toàn qua cổng VNPay (sử dụng Ngrok để forward IPN Webhook khi chạy local).
- **Quản lý Media lưu trữ đám mây:** Tự động upload và tối ưu hóa hình ảnh sản phẩm thông qua Cloudinary.
- **Đánh giá & Nhận xét:** Người dùng có thể để lại sao đánh giá và bình luận về sản phẩm.
- **Quản trị viên (Admin Dashboard):** Quản lý người dùng, sản phẩm (thêm, sửa, nhập hàng, quản lý biến thể màu sắc), và duyệt/hủy đơn hàng.

## 🚀 Công nghệ sử dụng

**Frontend:**
- Ngôn ngữ: **TypeScript**
- Thư viện/Framework: React.js
- Định tuyến: React Router DOM

**Backend:**
- Ngôn ngữ: **JavaScript**
- Môi trường: Node.js & Express.js
- Cơ sở dữ liệu: **MongoDB** (sử dụng Mongoose)

**Dịch vụ bên thứ 3 (Third-party Services):**
- **Cloudinary**: Lưu trữ và quản lý hình ảnh.
- **Google Gemini AI**: Phân tích dữ liệu và đề xuất sản phẩm liên quan.
- **VNPay**: Cổng thanh toán nội địa.
- **Ngrok**: Public localhost để nhận webhook (IPN) từ VNPay.

## 📂 Cấu trúc thư mục chính

```text
electro-shop/
├── backend/               # Chứa mã nguồn Backend (Node.js/Express)
│   ├── src/
│   │   ├── configs/       # Cấu hình Database, Cloudinary
│   │   ├── controllers/   # Xử lý logic API (Auth, Product, Order, AI,...)
│   │   ├── models/        # Định nghĩa Schema MongoDB
│   │   ├── routes/        # Khai báo các endpoint API
│   │   └── utils/         # Các hàm tiện ích dùng chung
│   └── package.json
└── web/                   # Chứa mã nguồn Frontend (React/TypeScript)
    ├── src/
    │   ├── component/     # Các UI component tái sử dụng
    │   ├── pages/         # Các trang giao diện (Home, ProductDetail, Cart, Admin,...)
    │   ├── services/      # Các hàm gọi API giao tiếp với Backend
    │   └── utils/         # Các hàm tiện ích Frontend
    └── package.json
```

## 🛠️ Hướng dẫn cài đặt và khởi chạy

### 1. Yêu cầu hệ thống
- Node.js (v16 trở lên)
- MongoDB (Local hoặc MongoDB Atlas)
- Tài khoản Cloudinary, VNPay Sandbox, và Google AI Studio (để lấy Gemini API Key)

### 2. Thiết lập Biến môi trường (.env)

Hệ thống sử dụng file `.env` riêng biệt để bảo mật các key quan trọng. Bạn cần tạo file `.env` ở thư mục `backend/` theo định dạng sau:

```env
# Server & Database
PORT=5000
MONGODB_URI=mongodb://localhost:27017/electro-shop  # Hoặc link MongoDB Atlas
JWT_SECRET=chuoi_ki_tu_bao_mat_cua_ban

# Cloudinary
CLOUDINARY_CLOUD_NAME=ten_cloud_cua_ban
CLOUDINARY_API_KEY=api_key_cua_ban
CLOUDINARY_API_SECRET=api_secret_cua_ban

# VNPay
VNP_TMN_CODE=ma_website_vnpay_sandbox
VNP_HASH_SECRET=chuoi_bi_mat_vnpay
VNP_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
VNP_API_URL=https://sandbox.vnpayment.vn/merchant_webapi/api/transaction
VNP_RETURN_URL=http://localhost:3000/payment-result

# Gemini AI
GEMINI_API_KEY=google_gemini_api_key_cua_ban
```

### 3. Cài đặt và chạy Backend

```bash
cd backend
npm install
npm start
# Backend sẽ chạy tại http://localhost:5000
```

### 4. Cài đặt và chạy Frontend

```bash
cd web
npm install
npm start 
# Hoặc npm run dev (tùy vào cấu hình Vite/CRA của bạn)
# Frontend sẽ chạy tại http://localhost:3000
```

## 🌐 Hướng dẫn cấu hình Ngrok cho thanh toán VNPay

VNPay yêu cầu một URL Public để gửi thông báo thay đổi trạng thái giao dịch (IPN Webhook) về Backend của bạn. Khi chạy ở môi trường Local, bạn cần sử dụng Ngrok.

1. Tải và cài đặt Ngrok.
2. Mở terminal mới và chạy lệnh expose port của Backend (ví dụ Backend chạy ở port 5000):
   ```bash
   ngrok http 5000
   ```
3. Ngrok sẽ tạo ra một URL public dạng: `https://<chuoi-ngau-nhien>.ngrok-free.app`.
4. Đăng nhập vào trang quản trị của VNPay Sandbox. Ở phần cấu hình URL nhận IPN, hãy cập nhật thành:
   ```text
   https://<chuoi-ngau-nhien>.ngrok-free.app/api/orders/vnpay_ipn
   ```
*(Lưu ý: URL IPN trên backend thực tế cần khớp với Route IPN bạn đã định nghĩa)*

## 🤝 Đóng góp

Mọi đóng góp giúp dự án hoàn thiện hơn đều được hoan nghênh. Xin vui lòng:
1. Fork dự án.
2. Tạo branch mới (`git checkout -b feature/AmazingFeature`).
3. Commit thay đổi (`git commit -m 'Add some AmazingFeature'`).
4. Push lên branch (`git push origin feature/AmazingFeature`).
5. Mở một Pull Request.

## 📝 Giấy phép

Phân phối theo giấy phép MIT.