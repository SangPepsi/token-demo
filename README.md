# VNPAY Token API - Demo

Demo tích hợp VNPAY Token API - thanh toán token hóa. Cho phép lưu thông tin thẻ một lần và thanh toán nhanh cho các giao dịch sau.

## Cấu trúc dự án

```
token-demo/
├── token-api/          # Backend - Spring Boot
├── token-ui/           # Frontend - React
└── README.md
```

## Công nghệ

| Thành phần | Công nghệ |
|------------|-----------|
| Backend | Spring Boot 3.1, Java 17, JPA |
| Frontend | React 18, React Router, Axios |
| Database | H2 (dev) / PostgreSQL (Docker) |

## Chức năng

- **Tạo Token** - Đăng ký thông tin thẻ để tạo token
- **Thanh toán & Tạo Token** - Thanh toán một lần và lưu token
- **Thanh toán bằng Token** - Sử dụng token đã lưu
- **Xóa Token** - Hủy token đã đăng ký
- **Quản lý giao dịch** - Xem lịch sử giao dịch

## Yêu cầu

- Java 17+
- Node.js 18+
- npm hoặc yarn

## Chạy nhanh (Local)

### 1. Backend

```bash
cd token-api
./gradlew bootRun
```

API chạy tại: http://localhost:8085

### 2. Frontend

```bash
cd token-ui
npm install
npm start
```

Ứng dụng mở tại: http://localhost:3000/token-ui

### 3. Cấu hình API cho Frontend

Mặc định frontend gọi API tại `http://localhost:8085`. Nếu API chạy port khác, tạo file `token-ui/.env`:

```
REACT_APP_API_BASE_URL=http://localhost:8085
```

## Chạy bằng Docker

```bash
cd token-api
docker-compose up --build
```

- API: http://localhost:8085
- PostgreSQL: localhost:5432

**Lưu ý:** Frontend cần chạy riêng (`npm start` trong token-ui).

## API Endpoints

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/create-token` | Tạo URL tạo token |
| GET | `/pay-and-create-token` | Tạo URL thanh toán & tạo token |
| GET | `/payment-token` | Tạo URL thanh toán bằng token |
| GET | `/remove-token` | Tạo URL xóa token |
| GET | `/transactions` | Danh sách giao dịch (phân trang) |
| POST | `/transactions/report` | Báo cáo giao dịch từ redirect VNPAY |
| POST | `/vnpay-ipn` | IPN callback từ VNPAY |

## Cấu hình VNPAY

Chỉnh trong `token-api/src/main/resources/application.properties`:

| Thuộc tính | Mô tả |
|------------|-------|
| `vnp.tmn.code` | Mã website (TMN Code) |
| `vnp.hash.secret` | Chuỗi bí mật |
| `vnp.return.url` | URL nhận kết quả sau thanh toán |

**Sandbox:** Dự án dùng môi trường sandbox VNPAY (`sandbox.vnpayment.vn`).

## Build Production

### Backend

```bash
cd token-api
./gradlew build
# JAR: build/libs/token-api-0.0.1-SNAPSHOT.jar
```

### Frontend

```bash
cd token-ui
npm run build
# Output: build/
```

Deploy thư mục `build/` lên web server. Cấu hình `homepage` trong `package.json` nếu deploy vào subpath.

## Xử lý lỗi thường gặp

| Lỗi | Giải pháp |
|-----|-----------|
| Không kết nối được API | Kiểm tra backend đã chạy trên port 8085 |
| CORS | Backend đã cấu hình cho localhost:* |
| Mã giao dịch trùng | Bấm "Dùng dữ liệu mẫu" để tạo mã mới |

## Tài liệu

- [VNPAY Token API](https://sandbox.vnpayment.vn/apis/files/VNPAY%20Payment%20Token_Techspec%202.1.0-VN.pdf)
