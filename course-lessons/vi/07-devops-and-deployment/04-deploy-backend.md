# Triển khai Backend với AI

## Mục tiêu học tập
- Triển khai Node.js API lên Railway và Render
- Cấu hình cơ sở dữ liệu production
- Sử dụng AI để thiết lập triển khai

## Tại sao triển khai backend quan trọng?

API của bạn cần đáng tin cậy, có thể mở rộng và an toàn. Nền tảng hiện đại xử lý hạ tầng để bạn có thể tập trung vào mã.

## Railway (Khuyến nghị cho người mới bắt đầu)

Railway làm cho triển khai đơn giản với tự động mở rộng.

### Triển khai với Git

1. Đẩy mã lên GitHub
2. Truy cập [railway.app](https://railway.app)
3. Nhấp "New Project"
4. Chọn "Deploy from GitHub repo"
5. Railway phát hiện Node.js và triển khai tự động

### Cấu hình Railway

```toml
# railway.toml
[build]
builder = "nixpacks"

[deploy]
startCommand = "npm start"
healthcheckPath = "/health"
healthcheckTimeout = 300
restartPolicyType = "on_failure"
restartPolicyMaxRetries = 3
```

### Thêm cơ sở dữ liệu

1. Nhấp "New" > "Database" > "MongoDB"
2. Railway cung cấp phiên bản MongoDB
3. Chuỗi kết nối tự động được thêm dưới dạng biến môi trường `MONGO_URL`

### Biến môi trường

```bash
# Đặt qua Railway CLI
railway variables set NODE_ENV=production
railway variables set JWT_SECRET=your-secret-key

# Hoặc trong Railway Dashboard:
# Tab Variables
```

## Render

Render cung cấp gói miễn phí và dễ dàng mở rộng.

### Triển khai với Git

1. Đẩy mã lên GitHub
2. Truy cập [render.com](https://render.com)
3. Nhấp "New" > "Web Service"
4. Kết nối kho lưu trữ của bạn
5. Cấu hình:
   - Build Command: `npm install`
   - Start Command: `npm start`
6. Nhấp "Create Web Service"

### Cấu hình Render

```yaml
# render.yaml
services:
  - type: web
    name: task-api
    env: node
    buildCommand: npm install
    startCommand: npm start
    envVars:
      - key: NODE_ENV
        value: production
      - key: DATABASE_URL
        fromDatabase:
          name: task-db
          property: connectionString

databases:
  - name: task-db
    plan: free
    databaseName: task_management
```

### Giới hạn gói miễn phí

- Dịch vụ tắt sau 15 phút không hoạt động
- Yêu cầu đầu tiên sau khi tắt mất ~30 giây
- 750 giờ/tháng miễn phí

## Prompt AI cho triển khai backend

```
Cấu hình triển khai cho API Node.js với:
1. Thiết lập Railway hoặc Render với triển khai tự động
2. Cấu hình cơ sở dữ liệu MongoDB Atlas
3. Biến môi trường cho production
4. Endpoint kiểm tra sức khỏe
5. Xử lý tắt优雅
6. Ghi log lỗi và giám sát
7. Cấu hình tự động mở rộng

Bao gồm cấu hình railway.toml hoặc render.yaml.
```

## MongoDB Atlas (Cơ sở dữ liệu production)

```javascript
// Kết nối MongoDB Atlas
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      // Mongoose hiện đại không cần các tùy chọn này
    });
    console.log(`MongoDB đã kết nối: ${conn.connection.host}`);
  } catch (error) {
    console.error(`Lỗi: ${error.message}`);
    process.exit(1);
  }
};
```

## Thực hành tốt nhất cho production

```javascript
// app.js - Cấu hình production
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const compression = require('compression');
const rateLimit = require('express-rate-limit');

const app = express();

// Tiêu đề bảo mật
app.use(helmet());

// CORS
app.use(cors({
  origin: process.env.CLIENT_URL,
  credentials: true
}));

// Nén
app.use(compression());

// Giới hạn tốc độ
app.use(rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
}));

// Kiểm tra sức khỏe
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date() });
});

// Tắt优雅
process.on('SIGTERM', () => {
  console.log('SIGTERM received. Shutting down gracefully...');
  server.close(() => {
    mongoose.connection.close(false, () => {
      process.exit(0);
    });
  });
});
```

## Bài tập thực hành

Triển khai API quản lý nhiệm vụ:
- Thiết lập tài khoản Railway hoặc Render
- Cấu hình cơ sở dữ liệu MongoDB Atlas
- Đặt biến môi trường
- Triển khai và kiểm thử API
- Thiết lập giám sát

## Điểm chính

- Railway và Render đơn giản hóa triển khai backend
- MongoDB Atlas cung cấp lưu trữ cơ sở dữ liệu được quản lý
- Biến môi trường giữ bí mật an toàn
- Kiểm tra sức khỏe cho phép giám sát và tự động khởi động lại
