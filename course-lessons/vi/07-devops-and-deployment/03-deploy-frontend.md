# Triển khai Frontend với AI

## Mục tiêu học tập
- Triển khai React/Next.js lên Vercel
- Triển khai trang tĩnh lên Netlify
- Sử dụng AI để cấu hình triển khai

## Tại sao triển khai quan trọng?

Xây dựng ứng dụng chỉ là một nửa trận chiến. Đưa nó lên trực tuyến nơi người dùng có thể truy cập là nửa còn lại. Nền tảng hiện đại làm cho triển khai frontend cực kỳ dễ dàng.

## Vercel (Khuyến nghị cho Next.js)

Vercel được tạo bởi những người tạo ra Next.js. Đây là nền tảng tốt nhất cho ứng dụng Next.js.

### Triển khai với Git

1. Đẩy mã lên GitHub
2. Truy cập [vercel.com](https://vercel.com)
3. Nhấp "New Project"
4. Nhập kho lưu trữ
5. Vercel tự động phát hiện Next.js và cấu hình mọi thứ
6. Nhấp "Deploy"

### Cấu hình Vercel

```json
// vercel.json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "env": {
    "NEXT_PUBLIC_API_URL": "@api-url"
  },
  "rewrites": [
    { "source": "/api/:path*", "destination": "https://api.example.com/:path*" }
  ]
}
```

### Biến môi trường

```bash
# Đặt qua Vercel CLI
vercel env add NEXT_PUBLIC_API_URL

# Hoặc trong Vercel Dashboard:
# Settings > Environment Variables
```

### Tên miền tùy chỉnh

1. Vào Project Settings > Domains
2. Thêm tên miền của bạn
3. Cấu hình bản ghi DNS như显示
4. SSL tự động

## Netlify (Tuyệt vời cho trang tĩnh)

Netlify xuất sắc trong lưu trữ trang tĩnh với trải nghiệm开发 tuyệt vời.

### Triển khai với Git

1. Đẩy mã lên GitHub
2. Truy cập [netlify.com](https://netlify.com)
3. Nhấp "New site from Git"
4. Chọn kho lưu trữ
5. Cấu hình cài đặt build:
   - Build command: `npm run build`
   - Publish directory: `dist` hoặc `build`
6. Nhấp "Deploy site"

### Cấu hình Netlify

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"
```

### Netlify Functions

Hàm serverless cho endpoint API:

```javascript
// netlify/functions/hello.js
exports.handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: 'Xin chào từ Netlify Functions!' })
  };
};
```

## Prompt AI cho triển khai frontend

```
Cấu hình triển khai cho ứng dụng React/Next.js với:
1. Cấu hình Vercel với biến môi trường
2. Thiết lập tên miền tùy chỉnh với SSL
3. Triển khai xem trước cho pull request
4. Tối ưu hóa build và bộ đệm
5. Quy tắc chuyển hướng cho định tuyến SPA
6. Cấu hình cụ thể cho môi trường
7. Thiết lập giám sát hiệu suất

Bao gồm cấu hình vercel.json và netlify.toml.
```

## Tối ưu hóa build

```javascript
// next.config.js
module.exports = {
  // Bật xuất tĩnh khi có thể
  output: 'standalone',

  // Tối ưu hóa hình ảnh
  images: {
    domains: ['cdn.example.com'],
    formats: ['image/avif', 'image/webp']
  },

  // Bật nén
  compress: true,

  // Trình phân tích bundle
  webpack: (config) => {
    if (process.env.ANALYZE === 'true') {
      const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
      config.plugins.push(new BundleAnalyzerPlugin());
    }
    return config;
  }
};
```

## Triển khai xem trước

Mỗi pull request nhận URL duy nhất:

```yaml
# GitHub Actions cho xem trước
name: Triển khai xem trước
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

## Bài tập thực hành

Triển khai frontend quản lý nhiệm vụ:
- Thiết lập tài khoản Vercel hoặc Netlify
- Cấu hình biến môi trường
- Thiết lập tên miền tùy chỉnh
- Bật triển khai xem trước
- Kiểm thử ứng dụng đã triển khai

## Điểm chính

- Vercel lý tưởng cho Next.js; Netlify xuất sắc cho trang tĩnh
- Triển khai dựa trên Git tự động và đáng tin cậy
- Biến môi trường giữ bí mật an toàn
- Triển khai xem trước cho phép kiểm thử trước production
