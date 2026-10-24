<div align="center">

  <h1>Christmas Wishes System</h1>
  <h3>Hệ thống Backend API gửi tặng quà Giáng Sinh</h3>

  <p>
    Một giải pháp Backend mạnh mẽ được xây dựng bằng kiến trúc Modular của NestJS. 
    Hệ thống cung cấp RESTful API để quản lý người dùng, xác thực bảo mật và xử lý toàn bộ nghiệp vụ tạo, đóng gói và chia sẻ các hộp quà Giáng Sinh bí mật.
  </p>

  <p>
    <img src="https://img.shields.io/badge/NestJS-v11.2.8-E0234E?logo=nestjs&logoColor=white" alt="NestJS">
    <img src="https://img.shields.io/badge/Node.js-%3E%3D20.0.0-339933?logo=nodedotjs&logoColor=white" alt="Node.js">
    <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white" alt="TypeScript">
    <img src="https://img.shields.io/badge/MongoDB-Atlas_%2F_Mongoose_9.11-47A248?logo=mongodb&logoColor=white" alt="MongoDB">
    <img src="https://img.shields.io/badge/Swagger-OpenAPI_3.0-85EA2D?logo=swagger&logoColor=black" alt="Swagger">
    <img src="https://img.shields.io/badge/Auth-JWT_%26_Passport-000000?logo=jsonwebtokens&logoColor=white" alt="Auth">
    <img src="https://img.shields.io/badge/status-Active_Development-success" alt="Status">
  </p>

</div>

<br />

# BACKEND API SERVICE

Đây là Repository chứa mã nguồn **Backend**, đóng vai trò là lõi xử lý nghiệp vụ, quản lý cơ sở dữ liệu (MongoDB) và cung cấp RESTful API cho hệ thống Frontend Christmas Wishes.

---

## Mục lục

- [1. Công nghệ & Thư viện Cốt lõi](#1-công-nghệ--thư-viện-cốt-lõi)
- [2. Tính năng Nghiệp vụ](#2-tính-năng-nghiệp-vụ)
  - [2.1. Xác thực & Quản lý Người dùng (Auth & Users)](#21-xác-thực--quản-lý-người-dùng-auth--users)
  - [2.2. Quản lý Quà Tặng (Gifts)](#22-quản-lý-quà-tặng-gifts)
  - [2.3. Hệ thống Email Thông báo & OTP (Brevo)](#23-hệ-thống-email-thông-báo--otp-brevo)
- [3. Hướng dẫn Cài đặt & Khởi chạy](#3-hướng-dẫn-cài-đặt--khởi-chạy)
  - [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
  - [Quy trình cài đặt](#quy-trình-cài-đặt)
  - [Cấu hình biến môi trường (.env)](#cấu-hình-biến-môi-trường-env)
  - [Các lệnh thực thi (Scripts)](#các-lệnh-thực-thi-scripts)
  - [Kiểm thử (Testing)](#kiểm-thử-testing)
- [4. Tài liệu API (Swagger)](#4-tài-liệu-api-swagger)
- [5. Cấu trúc Module & Thư mục](#5-cấu-trúc-module--thư-mục)
- [6. Tác giả](#6-tác-giả)

---

## 1. Công nghệ & Thư viện Cốt lõi

Hệ thống được phát triển dựa trên cấu hình `package.json` với các phiên bản bảo mật và ổn định mới nhất:

| Công nghệ / Thư viện | Phiên bản | Vai trò Kỹ thuật |
| :--- | :--- | :--- |
| **[NestJS](https://nestjs.com/)** | `^11.2.8` | Khung ứng dụng backend Node.js kiến trúc Modular enterprise |
| **[@nestjs/mongoose](https://docs.nestjs.com/techniques/mongodb)** | `^11.0.4` | Tích hợp MongoDB và Mongoose vào hệ sinh thái NestJS |
| **[Mongoose](https://mongoosejs.com/)** | `^9.11.4` | ODM MongoDB, quản lý Schema, Data Models và Validation dữ liệu |
| **[google-auth-library](https://github.com/googleapis/google-auth-library-nodejs)** | `^10.9.1` | Xử lý xác thực Token từ Google OAuth 2.0 (Google Login) |
| **[@getbrevo/brevo](https://www.brevo.com/)** | `^3.0.4` | Dịch vụ gửi Transactional Email (Xác thực, OTP khôi phục mật khẩu) |
| **[bcrypt](https://www.npmjs.com/package/bcrypt)** | `^6.0.0` | Mã hóa và bảo mật mật khẩu người dùng theo chuẩn băm một chiều |
| **[passport-jwt](https://www.passportjs.org/)** | `^4.0.1` | Chiến lược xác thực người dùng dựa trên cơ chế JWT Access Token |
| **[@nestjs/swagger](https://docs.nestjs.com/openapi/introduction)** | `^11.4.7` | Tự động tạo tài liệu API tương tác chuẩn OpenAPI |

---

## 2. Tính năng Nghiệp vụ

### 2.1. Xác thực & Quản lý Người dùng (Auth & Users)
* **Xác thực đa phương thức:** Hỗ trợ JWT truyền thống và Google OAuth 2.0 (tự động liên kết hoặc khởi tạo tài khoản theo địa chỉ email).
* **Mã hóa an toàn:** Toàn bộ mật khẩu được băm bảo mật theo chuẩn bcrypt.
* **Hồ sơ cá nhân:** Cung cấp tính năng cập nhật thông tin hồ sơ và thay đổi mật khẩu an toàn với xác thực mật khẩu hiện tại.
* **Hệ thống Single Sign-On (SSO):** Người dùng có thể sử dụng cùng một tài khoản liên thông cho cả sự kiện Tết và Giáng sinh trong hệ sinh thái Festive Ecosystem.

### 2.2. Quản lý Quà Tặng (Gifts)
* **Đóng gói hộp quà:** Cho phép đóng gói hộp quà kèm nội dung lời chúc, tùy chỉnh màu sắc chủ đề (đỏ, xanh, vàng) và đính kèm đường dẫn âm nhạc lễ hội.
* **Quản lý danh sách quà:** Lấy danh sách các món quà đã tạo để theo dõi, quản lý và lấy đường dẫn chia sẻ độc quyền.
* **Mở quà tương tác:** Cung cấp dữ liệu chi tiết của hộp quà theo mã định danh (ID) để kết xuất hiệu ứng 3D và phát bản nhạc chỉ định phía Frontend.

### 2.3. Hệ thống Email Thông báo & OTP (Brevo)
* **Tích hợp Brevo API:** Điều phối các mẫu email giao dịch tự động.
* **Cơ chế khôi phục mật khẩu:** Cung cấp quy trình xác thực mã OTP gửi về email với thời hạn sử dụng nghiêm ngặt để khôi phục tài khoản an toàn.

---

## 3. Hướng dẫn Cài đặt & Khởi chạy

### Yêu cầu hệ thống
* **Node.js:** Phiên bản `>= 20.0.0` (Khuyến nghị Node.js 20.x hoặc 22.x LTS).
* **Cơ sở dữ liệu:** MongoDB Server cục bộ hoặc MongoDB Atlas cluster.
* **Trình quản lý gói:** `npm` (>= 10.0.0), `yarn`, `pnpm`.

### Quy trình cài đặt

```bash
# 1. Tải mã nguồn Backend về máy cục bộ
git clone https://github.com/hvt299/Christmas-Wishes-Backend.git
cd Christmas-Wishes-Backend

# 2. Cài đặt các gói phụ thuộc
npm install
```

### Cấu hình biến môi trường (.env)

Tạo tệp `.env` tại thư mục gốc của dự án:

```env
PORT=3001
FRONTEND_URL=http://localhost:3000
MONGO_URI=mongodb://localhost:27017/festive_events

GEMINI_API_KEY=YourSecretKeyHere

JWT_SECRET=YourSecretKeyHere
JWT_EXPIRATION=1d

SENDER_EMAIL=YourEmailHere
BREVO_API_KEY=YourSecretKeyHere
GOOGLE_CLIENT_ID=YourSecretKeyHere
```

### Các lệnh thực thi (Scripts)

```bash
# Khởi chạy môi trường phát triển (Watch mode)
npm run start:dev

# Biên dịch bản phát hành sản phẩm (Production Build)
npm run build

# Khởi chạy bản production đã biên dịch
npm run start:prod
```

### Kiểm thử (Testing)

```bash
# Chạy Unit tests
npm run test

# Chạy E2E tests
npm run test:e2e

# Xuất báo cáo độ phủ mã nguồn (Coverage report)
npm run test:cov
```

---

## 4. Tài liệu API (Swagger)

Sau khi khởi chạy máy chủ backend thành công, truy cập đường dẫn sau trên trình duyệt để tương tác với Swagger UI:

```text
http://localhost:3001/api
```

Giao diện Swagger hiển thị chi tiết danh sách Routes, Schemas, Request Body và Response DTO.

---

## 5. Cấu trúc Module & Thư mục

```text
Christmas-Wishes-Backend/
├── src/
│   ├── app.module.ts          # Root module điều phối các phân hệ
│   ├── main.ts                # Entry point, cấu hình CORS, Global Validation & Swagger
│   ├── auth/                  # Logic Đăng nhập, Đăng ký, JWT Strategy, Google OAuth2
│   ├── users/                 # Module quản lý tài khoản User & thông tin cá nhân
│   ├── gifts/                 # Module xử lý đóng gói và truy xuất hộp quà Giáng Sinh
│   ├── wishes/                # Module quản lý danh mục lời chúc
│   ├── songs/                 # Module danh mục âm nhạc lễ hội
│   └── email/                 # Service gửi email thông báo và mã OTP qua Brevo API
├── test/                      # Kịch bản kiểm thử E2E
├── package.json               # Cấu hình phụ thuộc và scripts
└── tsconfig.json              # Cấu hình trình biên dịch TypeScript
```

---

## 6. Tác giả

Được phát triển bởi **Mr.T (hvt299)**  
GitHub: [https://github.com/hvt299](https://github.com/hvt299)
