# Giám sát và ghi log với AI

## Mục tiêu học tập
- Thiết lập theo dõi lỗi với Sentry
- Triển khai ghi log có cấu trúc
- Sử dụng AI để cấu hình giám sát

## Tại sao giám sát quan trọng?

Bạn không thể sửa những gì bạn không thấy. Giám sát cho bạn biết khi mọi thứ xảy ra sự cố, ghi log giúp bạn hiểu为什么, và分析显示 cách người dùng tương tác với ứng dụng.

## Theo dõi lỗi với Sentry

Sentry捕捉 lỗi theo thời gian thực với đầy đủ ngữ cảnh:

```javascript
// Cài đặt: npm install @sentry/node

const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0, // Ch捕捉 100% giao dịch trong dev
  integrations: [
    Sentry.expressIntegration(),
    Sentry.mongoIntegration()
  ]
});

// Phải đặt trước bất kỳ middleware nào
app.use(Sentry.expressRequestHandler());
app.use(Sentry.expressErrorHandler());
```

### Ch捕捉 lỗi

```javascript
// Tự động -捕捉 lỗi không được xử lý
app.get('/api/risky', async (req, res) => {
  // Lỗi này được捕捉 tự động
  throw new Error('Đã xảy ra lỗi');
});

// Thủ công -捕捉 lỗi cụ thể
try {
  await riskyOperation();
} catch (error) {
  Sentry.captureException(error, {
    tags: { section: 'payment' },
    extra: { userId: req.user.id, orderId: req.body.orderId }
  });
  res.status(500).json({ error: 'Thanh toán thất bại' });
}
```

## Ghi log có cấu trúc với Winston

```javascript
// utils/logger.js
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: { service: 'task-api' },
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

module.exports = logger;
```

### Sử dụng Logger

```javascript
const logger = require('../utils/logger');

// Mức log
logger.error('Kết nối cơ sở dữ liệu thất bại', { error: err.message });
logger.warn('Sử dụng bộ nhớ cao', { usage: process.memoryUsage() });
logger.info('Người dùng đã đăng ký', { userId: user.id, email: user.email });
logger.debug('Truy vấn đã thực thi', { query: 'SELECT * FROM users', duration: '50ms' });
```

## Ghi log yêu cầu

```javascript
// middleware/requestLogger.js
const logger = require('../utils/logger');

const requestLogger = (req, res, next) => {
  const start = Date.now();

  res.on('finish', () => {
    const duration = Date.now() - start;
    logger.info('Yêu cầu đã hoàn thành', {
      method: req.method,
      url: req.url,
      status: res.statusCode,
      duration: `${duration}ms`,
      userAgent: req.get('User-Agent'),
      ip: req.ip
    });
  });

  next();
};
```

## Endpoint kiểm tra sức khỏe

```javascript
app.get('/health', async (req, res) => {
  const health = {
    status: 'ok',
    timestamp: new Date(),
    uptime: process.uptime(),
    memory: process.memoryUsage(),
    database: 'unknown'
  };

  try {
    await mongoose.connection.db.admin().ping();
    health.database = 'connected';
  } catch (error) {
    health.database = 'disconnected';
    health.status = 'degraded';
  }

  const statusCode = health.status === 'ok' ? 200 : 503;
  res.status(statusCode).json(health);
});
```

## Prompt AI cho giám sát

```
Thiết lập giám sát và ghi log cho API Node.js với:
1. Sentry cho theo dõi lỗi với ngữ cảnh
2. Winston cho ghi log có cấu trúc
3. Middleware ghi log yêu cầu
4. Endpoint kiểm tra sức khỏe với trạng thái cơ sở dữ liệu
5. Giám sát hiệu suất
6. Cấu hình cảnh báo cho lỗi nghiêm trọng
7. Xoay vòng log và保留

Bao gồm ví dụ cho mỗi loại giám sát.
```

## Phân tích với Mixpanel

```javascript
// Theo dõi hành động người dùng
const Mixpanel = require('mixpanel');
const mixpanel = Mixpanel.init(process.env.MIXPANEL_TOKEN);

// Theo dõi sự kiện
mixpanel.track('Nhiệm vụ đã tạo', {
  distinct_id: user.id,
  task_id: task.id,
  project_id: task.project
});

// Theo dõi thuộc tính người dùng
mixpanel.people.set(user.id, {
  $email: user.email,
  $name: user.name,
  tasks_created: 42
});
```

## Bài tập thực hành

Thêm giám sát vào API quản lý nhiệm vụ:
- Thiết lập Sentry cho theo dõi lỗi
- Triển khai ghi log Winston
- Tạo endpoint kiểm tra sức khỏe
- Thêm middleware ghi log yêu cầu
- Thiết lập cảnh báo cho lỗi nghiêm trọng

## Điểm chính

- Sentry捕捉 lỗi với đầy đủ ngữ cảnh để gỡ lỗi
- Ghi log có cấu trúc làm cho log có thể tìm kiếm và phân tích
- Kiểm tra sức khỏe cho phép giám sát và tự động khởi động lại
- Phân tích giúp hiểu hành vi người dùng
