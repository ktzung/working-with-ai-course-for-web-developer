# Xử lý lỗi với AI

## Mục tiêu học tập
- Triển khai mẫu xử lý lỗi toàn cục
- Tạo middleware xác thực
- Thiết kế phản hồi lỗi nhất quán

## Tại sao xử lý lỗi quan trọng?

Người dùng sẽ phá vỡ ứng dụng của bạn. Họ sẽ gửi dữ liệu không hợp lệ, truy cập liên kết hết hạn, và nhấp nút quá nhanh. Xử lý lỗi tốt biến sự cố thành thông báo hữu ích và giữ ứng dụng hoạt động.

## Định dạng phản hồi lỗi

Nhất quán là chìa khóa. Mọi lỗi nên theo cùng một cấu trúc:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Dữ liệu đầu vào không hợp lệ",
    "details": [
      { "field": "email", "message": "Phải là địa chỉ email hợp lệ" },
      { "field": "password", "message": "Phải có ít nhất 8 ký tự" }
    ]
  }
}
```

## Lớp lỗi tùy chỉnh

```javascript
// utils/AppError.js
class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

// Sử dụng
class NotFoundError extends AppError {
  constructor(resource) {
    super(`Không tìm thấy ${resource}`, 404, 'NOT_FOUND');
  }
}

class ValidationError extends AppError {
  constructor(details) {
    super('Xác thực thất bại', 400, 'VALIDATION_ERROR');
    this.details = details;
  }
}

class UnauthorizedError extends AppError {
  constructor(message = 'Yêu cầu xác thực') {
    super(message, 401, 'UNAUTHORIZED');
  }
}
```

## Trình xử lý lỗi toàn cục

```javascript
// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  let error = { ...err };
  error.message = err.message;

  // Ghi log lỗi để gỡ lỗi
  console.error(`[${new Date().toISOString()}] ${err.method} ${err.url}:`, err);

  // Mongoose ObjectId sai
  if (err.name === 'CastError') {
    error = new AppError('Không tìm thấy tài nguyên', 404, 'NOT_FOUND');
  }

  // Mongoose khóa trùng lặp
  if (err.code === 11000) {
    const field = Object.keys(err.keyValue)[0];
    error = new AppError(
      `Giá trị trùng lặp cho ${field}`,
      400,
      'DUPLICATE_VALUE'
    );
  }

  // Lỗi xác thực Mongoose
  if (err.name === 'ValidationError') {
    const details = Object.values(err.errors).map(e => ({
      field: e.path,
      message: e.message
    }));
    error = new ValidationError(details);
  }

  // Lỗi JWT
  if (err.name === 'JsonWebTokenError') {
    error = new AppError('Token không hợp lệ', 401, 'INVALID_TOKEN');
  }
  if (err.name === 'TokenExpiredError') {
    error = new AppError('Token đã hết hạn', 401, 'TOKEN_EXPIRED');
  }

  res.status(error.statusCode || 500).json({
    success: false,
    error: {
      code: error.code || 'INTERNAL_ERROR',
      message: error.isOperational ? error.message : 'Đã xảy ra lỗi',
      ...(error.details && { details: error.details }),
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};

module.exports = errorHandler;
```

## Wrapper lỗi bất đồng bộ

Tránh try-catch trong mỗi controller:

```javascript
// middleware/asyncHandler.js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Sử dụng
router.get('/products', asyncHandler(async (req, res) => {
  const products = await Product.find();
  res.json({ success: true, data: products });
}));
```

## Middleware xác thực với Zod

```javascript
// middleware/validate.js
const { z } = require('zod');

const validate = (schema) => (req, res, next) => {
  try {
    req.body = schema.parse(req.body);
    next();
  } catch (error) {
    const details = error.errors.map(e => ({
      field: e.path.join('.'),
      message: e.message
    }));
    next(new ValidationError(details));
  }
};

// Định nghĩa schema
const registerSchema = z.object({
  email: z.string().email('Định dạng email không hợp lệ'),
  password: z.string().min(8, 'Mật khẩu phải có ít nhất 8 ký tự')
    .regex(/[A-Z]/, 'Phải chứa chữ hoa')
    .regex(/[0-9]/, 'Phải chứa số'),
  name: z.string().min(2).max(50)
});

// Sử dụng route
router.post('/register', validate(registerSchema), authController.register);
```

## Prompt AI cho xử lý lỗi

```
Tạo hệ thống xử lý lỗi toàn diện cho Express.js với:
1. Lớp lỗi tùy chỉnh (AppError, NotFound, Validation, Auth, Forbidden)
2. Middleware xử lý lỗi toàn cục
3. Wrapper lỗi bất đồng bộ
4. Xác thực đầu vào với schema Zod
5. Ghi log lỗi với Winston
6. Phản hồi khác nhau cho môi trường phát triển và production
7. Trình xử lý từ chối không xử lý và ngoại lệ không bắt

Bao gồm ví dụ sử dụng mỗi loại lỗi trong controller.
```

## Trình xử lý 404

```javascript
// Bắt các route không xác định
app.use('*', (req, res) => {
  res.status(404).json({
    success: false,
    error: {
      code: 'NOT_FOUND',
      message: `Không tìm thấy route ${req.method} ${req.originalUrl}`
    }
  });
});
```

## Bài tập thực hành

Thêm xử lý lỗi toàn diện vào API CRUD của bạn:
- Lớp lỗi tùy chỉnh cho mỗi loại lỗi
- Trình xử lý lỗi toàn cục với ghi log
- Xác thực đầu vào với Zod cho tất cả endpoint
- Phản hồi lỗi đúng cách cho lỗi cơ sở dữ liệu
- Chi tiết lỗi cho môi trường phát triển và production

## Điểm chính

- Định dạng lỗi nhất quán giúp frontend xử lý lỗi dễ dàng
- Lớp lỗi tùy chỉnh làm cho lỗi mô tả và có thể hành động
- Trình xử lý lỗi toàn cục bắt mọi thứ ở một nơi
- Xác thực ngăn dữ liệu xấu trước khi进入 cơ sở dữ liệu
