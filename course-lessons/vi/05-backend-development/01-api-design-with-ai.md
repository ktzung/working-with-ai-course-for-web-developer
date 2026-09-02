# Thiết kế API với AI

## Mục tiêu học tập
- Thiết kế RESTful API với sự hỗ trợ của AI
- Hiểu quy tắc đặt tên endpoint và các HTTP method
- Tạo middleware cho các chức năng phổ biến

## Tại sao thiết kế API quan trọng?

API được thiết kế tốt là xương sống của mọi ứng dụng web. Nó định nghĩa cách frontend giao tiếp với backend, cách các dịch vụ bên thứ ba tích hợp với hệ thống, và khả năng mở rộng của ứng dụng theo thời gian.

Hãy tưởng tượng API như thực đơn nhà hàng — nó cho khách hàng biết chính xác những gì có sẵn, cách đặt hàng, và họ sẽ nhận được gì. API thiết kế tệ giống như thực đơn rối rắm: khách hàng sẽ bực mình và bỏ đi.

## Nguyên tắc RESTful API

REST (Representational State Transfer) là kiến trúc API phổ biến nhất. Đây là các nguyên tắc cốt lõi:

**URL dựa trên tài nguyên**: Mỗi endpoint đại diện cho một tài nguyên (danh từ, không phải động từ).

```
✅ Đúng:  GET /api/users, POST /api/orders
❌ Sai:   GET /api/getUsers, POST /api/createOrder
```

**HTTP Method định nghĩa hành động**:
- `GET` — Lấy dữ liệu (chỉ đọc)
- `POST` — Tạo tài nguyên mới
- `PUT` — Cập nhật toàn bộ tài nguyên
- `PATCH` — Cập nhật một phần
- `DELETE` — Xóa tài nguyên

**Mã trạng thái truyền đạt kết quả**:
- `200` — Thành công
- `201` — Đã tạo
- `400` — Yêu cầu sai (lỗi client)
- `401` — Chưa xác thực
- `404` — Không tìm thấy
- `500` — Lỗi server

## Sử dụng AI để thiết kế API

Đây là prompt mạnh mẽ để thiết kế API:

```
Thiết kế RESTful API cho nền tảng thương mại điện tử với:
- Quản lý người dùng (đăng ký, đăng nhập, hồ sơ)
- Danh mục sản phẩm (thao tác CRUD)
- Giỏ hàng (thêm, xóa, thanh toán)
- Quản lý đơn hàng (tạo, theo dõi, hủy)

Bao gồm:
1. URL endpoint với HTTP method
2. Ví dụ request/response body
3. Yêu cầu xác thực cho mỗi endpoint
4. Các phản hồi lỗi phổ biến
```

AI sẽ tạo ra đặc tả API hoàn chỉnh mà bạn có thể tinh chỉnh và triển khai.

## Cấu trúc API với Express.js

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const auth = require('../middleware/auth');

router.post('/register', userController.register);
router.post('/login', userController.login);
router.get('/profile', auth, userController.getProfile);
router.put('/profile', auth, userController.updateProfile);

module.exports = router;
```

## Pattern Middleware

Middleware thực thi trước route handler. Các用途 phổ biến:

```javascript
// middleware/logger.js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next();
};

// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: {
      message: err.message || 'Internal Server Error',
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};
```

## Prompt AI để tạo Middleware

```
Tạo Express.js middleware cho:
1. Ghi log request (method, URL, thời gian, thời gian phản hồi)
2. Giới hạn tốc độ (100 request mỗi 15 phút mỗi IP)
3. Cấu hình CORS (cho phép các origin cụ thể)
4. Xác thực request bằng Joi schema

Bao gồm xử lý lỗi và làm cho mỗi middleware có thể cấu hình.
```

## Thực hành tốt nhất

1. **Version API**: `/api/v1/users` cho phép thay đổi trong tương lai mà không phá vỡ client
2. **Dùng danh từ số nhiều**: `/users` không phải `/user` — bạn đang đại diện cho một tập hợp
3. **Lồng có逻辑**: `/users/:id/orders` thể hiện mối quan hệ
4. **Phân trang danh sách**: Không bao giờ trả về dữ liệu vô hạn — dùng `?page=1&limit=20`
5. **Lọc và sắp xếp**: `?sort=createdAt&order=desc&status=active`

## Bài tập thực hành

Thiết kế API cho nền tảng blog với:
- Xác thực người dùng
- Bài viết (tạo, đọc, cập nhật, xóa)
- Bình luận trên bài viết
- Thẻ và danh mục
- Chức năng tìm kiếm

Sử dụng AI để tạo danh sách endpoint hoàn chỉnh, sau đó triển khai các route trong Express.js.

## Điểm chính

- RESTful API sử dụng tài nguyên, HTTP method và mã trạng thái
- AI có thể tạo đặc tả API hoàn chỉnh từ yêu cầu
- Middleware xử lý các mối quan tâm横切 như xác thực và ghi log
- Thiết kế API tốt phải nhất quán, có thể dự đoán và được tài liệu hóa
