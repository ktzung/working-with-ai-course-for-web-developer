# Docker với AI

## Mục tiêu học tập
- Tạo Dockerfile cho ứng dụng Node.js
- Sử dụng docker-compose cho thiết lập nhiều容器
- Sử dụng AI để tạo cấu hình Docker

## Tại sao Docker quan trọng?

Docker đóng gói ứng dụng với tất cả phụ thuộc vào một container. Điều này đảm bảo ứng dụng chạy giống nhau ở khắp nơi — máy tính xách tay của bạn, máy tính xách tay của đồng đội, và server production.

**Không có Docker**: "Nó hoạt động trên máy tôi" 🤷
**Có Docker**: "Nó hoạt động khắp nơi" ✅

## Dockerfile cơ bản

```dockerfile
# Dockerfile cho Node.js
FROM node:20-alpine

WORKDIR /app

# Sao chép tệp package trước (cho bộ đệm)
COPY package*.json ./

# Cài đặt phụ thuộc
RUN npm ci --only=production

# Sao chép mã ứng dụng
COPY . .

# Expose cổng
EXPOSE 3000

# Khởi động ứng dụng
CMD ["node", "src/index.js"]
```

## Build nhiều giai đoạn

Hình ảnh production nhỏ hơn:

```dockerfile
# Giai đoạn build
FROM node:20-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Giai đoạn production
FROM node:20-alpine AS production

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist

EXPOSE 3000
CMD ["node", "dist/index.js"]
```

## Docker Compose

Chạy nhiều dịch vụ cùng nhau:

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=mongodb://mongo:27017/myapp
      - REDIS_URL=redis://redis:6379
    depends_on:
      - mongo
      - redis
    restart: unless-stopped

  mongo:
    image: mongo:7
    volumes:
      - mongo-data:/data/db
    ports:
      - "27017:27017"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - app

volumes:
  mongo-data:
```

## .dockerignore

```
node_modules
npm-debug.log
.env
.git
.gitignore
README.md
coverage
.nyc_output
```

## Phát triển với Docker

```yaml
# docker-compose.dev.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
    command: npm run dev

  mongo:
    image: mongo:7
    ports:
      - "27017:27017"
```

```dockerfile
# Dockerfile.dev
FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

EXPOSE 3000
CMD ["npm", "run", "dev"]
```

## Prompt AI cho Docker

```
Tạo cấu hình Docker cho ứng dụng full-stack với:
1. Dockerfile nhiều giai đoạn cho backend Node.js
2. Dockerfile cho frontend React với Nginx
3. docker-compose.yml với app, cơ sở dữ liệu, Redis và Nginx
4. Cấu hình phát triển và production
5. Mount volume cho持久 hóa dữ liệu
6. Kiểm tra sức khỏe cho tất cả dịch vụ
7. Quản lý biến môi trường

Bao gồm .dockerignore và nginx.conf.
```

## Kiểm tra sức khỏe

```dockerfile
# Kiểm tra sức khỏe trong Dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

```yaml
# Kiểm tra sức khỏe trong docker-compose
services:
  app:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

## Lệnh Docker

```bash
# Build hình ảnh
docker build -t myapp .

# Chạy container
docker run -p 3000:3000 myapp

# Docker Compose
docker-compose up -d        # Khởi động tất cả dịch vụ
docker-compose down          # Dừng tất cả dịch vụ
docker-compose logs -f       # Xem log
docker-compose ps            # Liệt kê dịch vụ đang chạy

# Dọn dẹp
docker system prune -a       # Xóa tài nguyên không sử dụng
```

## Bài tập thực hành

Docker hóa ứng dụng quản lý nhiệm vụ:
- Tạo Dockerfile cho backend
- Tạo docker-compose với MongoDB và Redis
- Thiết lập cấu hình phát triển và production
- Thêm kiểm tra sức khỏe
- Kiểm thử thiết lập hoàn chỉnh

## Điểm chính

- Docker đảm bảo môi trường nhất quán giữa phát triển và production
- Build nhiều giai đoạn tạo hình ảnh production nhỏ hơn
- Docker Compose quản lý ứng dụng nhiều dịch vụ
- Kiểm tra sức khỏe giám sát trạng thái container
