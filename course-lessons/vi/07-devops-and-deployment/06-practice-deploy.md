# Thực hành: Triển khai ứng dụng hoàn chỉnh

## Mục tiêu học tập
- Áp dụng tất cả khái niệm triển khai trong dự án thực tế
- Triển khai frontend và backend lên production
- Thiết lập giám sát và ghi log

## Dự án: Triển khai ứng dụng quản lý nhiệm vụ

Triển khai ứng dụng quản lý nhiệm vụ hoàn chỉnh với frontend, backend, cơ sở dữ liệu và giám sát.

## Kiến trúc

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Vercel    │────▶│   Railway   │────▶│   MongoDB   │
│  (Next.js)  │     │  (Express)  │     │   Atlas     │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                    ┌──────┴──────┐
                    │    Redis    │
                    │   (Railway) │
                    └─────────────┘
```

## Bước 1: Chuẩn bị Backend

```bash
# Thêm phụ thuộc production
npm install helmet compression cors express-rate-limit

# Tạo endpoint kiểm tra sức khỏe
# Thêm tắt优雅
# Cấu hình biến môi trường
```

## Bước 2: Triển khai Backend lên Railway

1. Tạo tài khoản Railway
2. Kết nối kho lưu trữ GitHub
3. Thêm dịch vụ MongoDB
4. Thêm dịch vụ Redis
5. Đặt biến môi trường:
   ```
   NODE_ENV=production
   JWT_SECRET=your-secret
   MONGODB_URI=${{MongoDB.MONGO_URL}}
   REDIS_URL=${{Redis.REDIS_URL}}
   ```
6. Triển khai và kiểm thử

## Bước 3: Triển khai Frontend lên Vercel

1. Tạo tài khoản Vercel
2. Nhập kho lưu trữ GitHub
3. Đặt biến môi trường:
   ```
   NEXT_PUBLIC_API_URL=https://your-api.railway.app
   ```
4. Triển khai và kiểm thử

## Bước 4: Thiết lập giám sát

```javascript
// Thêm vào backend
const Sentry = require('@sentry/node');
const logger = require('./utils/logger');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV
});

// Thêm ghi log yêu cầu
app.use(requestLogger);

// Thêm kiểm tra sức khỏe
app.get('/health', healthCheck);
```

## Bước 5: Cấu hình CI/CD

```yaml
# .github/workflows/deploy.yml
name: Triển khai

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test

  deploy-backend:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: railwayapp/deploy@v1
        with:
          service: task-api

  deploy-frontend:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
```

## Bước 6: Tên miền tùy chỉnh

### Backend (Railway)
1. Vào Settings > Domains
2. Thêm tên miền tùy chỉnh: `api.yourdomain.com`
3. Cấu hình bản ghi DNS CNAME

### Frontend (Vercel)
1. Vào Settings > Domains
2. Thêm tên miền tùy chỉnh: `yourdomain.com`
3. Cấu hình bản ghi DNS

## Bước 7: SSL và bảo mật

- SSL tự động trên cả hai nền tảng
- Bật tiêu đề HSTS
- Cấu hình CORS cho miền production
- Thiết lập giới hạn tốc độ

## Sản phẩm bàn giao

1. ✅ Backend triển khai lên Railway
2. ✅ Frontend triển khai lên Vercel
3. ✅ Cơ sở dữ liệu MongoDB Atlas
4. ✅ Bộ đệm Redis
5. ✅ Tên miền tùy chỉnh với SSL
6. ✅ Pipeline CI/CD
7. ✅ Theo dõi lỗi với Sentry
8. ✅ Ghi log có cấu trúc
9. ✅ Kiểm tra sức khỏe

## Danh sách kiểm tra production

- [ ] Biến môi trường đã đặt
- [ ] Sao lưu cơ sở dữ liệu đã cấu hình
- [ ] Theo dõi lỗi đã bật
- [ ] Ghi log đã cấu hình
- [ ] Kiểm tra sức khỏe hoạt động
- [ ] Giới hạn tốc độ đã bật
- [ ] CORS đã cấu hình
- [ ] SSL hoạt động
- [ ] Tên miền tùy chỉnh hoạt động
- [ ] Pipeline CI/CD đã kiểm thử

## Điểm chính

- Nền tảng hiện đại làm cho triển khai dễ tiếp cận
- CI/CD tự động hóa quy trình triển khai
- Giám sát phát hiện sự cố trước người dùng
- Bảo mật phải được cấu hình cho production
