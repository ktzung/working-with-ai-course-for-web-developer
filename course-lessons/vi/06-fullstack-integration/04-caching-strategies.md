# Chiến lược bộ đệm với AI

## Mục tiêu học tập
- Triển khai bộ đệm ở nhiều cấp độ
- Sử dụng Redis cho bộ đệm phía server
- Cấu hình CDN và bộ đệm trình duyệt

## Tại sao bộ đệm quan trọng?

Bộ đệm lưu trữ dữ liệu thường xuyên truy cập gần nơi cần thiết. Nó giảm tải cơ sở dữ liệu, giảm thời gian phản hồi và cải thiện trải nghiệm người dùng.

**Không có bộ đệm**: Mọi yêu cầu đều chạm cơ sở dữ liệu
**Có bộ đệm**: Yêu cầu lặp lại được phục vụ từ bộ nhớ (mili giây so với hàng trăm mili giây)

## Các cấp độ bộ đệm

1. **Bộ đệm trình duyệt**: CSS, JS, hình ảnh lưu trên thiết bị người dùng
2. **Bộ đệm CDN**: Tài nguyên tĩnh được phục vụ từ server biên toàn cầu
3. **Bộ đệm ứng dụng**: Dữ liệu truy vấn thường xuyên lưu trong Redis
4. **Bộ đệm cơ sở dữ liệu**: Kết quả truy vấn được bộ đệm bởi công cụ cơ sở dữ liệu

## Bộ đệm Redis

Redis là kho dữ liệu trong bộ nhớ, hoàn hảo cho bộ đệm:

```javascript
// services/cacheService.js
const Redis = require('ioredis');
const redis = new Redis(process.env.REDIS_URL);

const cache = {
  async get(key) {
    const data = await redis.get(key);
    return data ? JSON.parse(data) : null;
  },

  async set(key, value, ttl = 3600) {
    await redis.set(key, JSON.stringify(value), 'EX', ttl);
  },

  async del(key) {
    await redis.del(key);
  },

  async clearPattern(pattern) {
    const keys = await redis.keys(pattern);
    if (keys.length) {
      await redis.del(...keys);
    }
  }
};

module.exports = cache;
```

## Middleware bộ đệm

```javascript
// middleware/cache.js
const cache = require('../services/cacheService');

const cacheMiddleware = (ttl = 3600) => async (req, res, next) => {
  const key = `cache:${req.originalUrl}`;

  try {
    const cached = await cache.get(key);
    if (cached) {
      return res.json(cached);
    }

    // Ghi đè res.json để lưu bộ đệm phản hồi
    const originalJson = res.json.bind(res);
    res.json = async (data) => {
      await cache.set(key, data, ttl);
      originalJson(data);
    };

    next();
  } catch (error) {
    next(); // Tiếp tục không có bộ đệm khi lỗi
  }
};

// Sử dụng
router.get('/products', cacheMiddleware(600), productController.getProducts);
```

## Vô hiệu hóa bộ đệm

Vấn đề khó nhất trong bộ đệm — giữ bộ đệm tươi mới:

```javascript
// Vô hiệu hóa bộ đệm khi dữ liệu thay đổi
exports.createProduct = async (req, res) => {
  const product = await Product.create(req.body);

  // Xóa bộ đệm liên quan
  await cache.clearPattern('cache:/api/products*');
  await cache.del('cache:product-count');

  res.status(201).json({ success: true, data: product });
};

// Mẫu cache-aside
async function getProductsWithCache() {
  // Thử bộ đệm trước
  let products = await cache.get('products:all');

  if (!products) {
    // Lỡ bộ đệm — lấy từ cơ sở dữ liệu
    products = await Product.find().lean();
    // Lưu trong bộ đệm 10 phút
    await cache.set('products:all', products, 600);
  }

  return products;
}
```

## Cấu hình CDN

```javascript
// Tệp tĩnh Express với tiêu đề bộ đệm
app.use('/static', express.static('public', {
  maxAge: '1y', // Đệm 1 năm
  immutable: true
}));

// Phản hồi API với tiêu đề bộ đệm
app.get('/api/products', (req, res) => {
  res.set({
    'Cache-Control': 'public, max-age=300', // 5 phút
    'ETag': generateETag(products)
  });
  res.json(products);
});
```

## Tiêu đề bộ đệm trình duyệt

```javascript
// Chiến lược bộ đệm khác nhau cho mỗi loại nội dung
const cacheHeaders = {
  immutable: { 'Cache-Control': 'public, max-age=31536000, immutable' },
  shortTerm: { 'Cache-Control': 'public, max-age=300' },
  noCache: { 'Cache-Control': 'no-cache, must-revalidate' },
  private: { 'Cache-Control': 'private, max-age=0' }
};

app.use('/assets', (req, res, next) => {
  res.set(cacheHeaders.immutable);
  next();
});

app.use('/api/user', (req, res, next) => {
  res.set(cacheHeaders.private);
  next();
});
```

## Prompt AI cho bộ đệm

```
Triển khai chiến lược bộ đệm cho Node.js API với:
1. Bộ đệm Redis với TTL có thể cấu hình cho mỗi endpoint
2. Middleware bộ đệm cho yêu cầu GET
3. Vô hiệu hóa bộ đệm trên POST/PUT/DELETE
4. Mẫu cache-aside cho truy vấn cơ sở dữ liệu
5. Cấu hình CDN cho tài nguyên tĩnh
6. Tiêu đề bộ đệm trình duyệt cho các loại nội dung khác nhau
7. Làm ấm bộ đệm cho dữ liệu thường xuyên truy cập

Bao gồm giám sát tỷ lệ命中 bộ đệm.
```

## Bộ đệm React Query

```javascript
// Bộ đệm phía client với React Query
const { data: products } = useQuery({
  queryKey: ['products'],
  queryFn: fetchProducts,
  staleTime: 5 * 60 * 1000, // Coi là tươi trong 5 phút
  cacheTime: 30 * 60 * 1000, // Giữ trong bộ đệm 30 phút
  refetchOnWindowFocus: true
});
```

## Bài tập thực hành

Thêm bộ đệm vào API quản lý nhiệm vụ:
- Bộ đệm Redis cho danh sách nhiệm vụ và chi tiết dự án
- Vô hiệu hóa bộ đệm khi nhiệm vụ được tạo/cập nhật/xóa
- Cấu hình CDN cho tệp đã tải lên
- Bộ đệm trình duyệt cho tài nguyên tĩnh
- Giám sát tỷ lệ命中 bộ đệm

## Điểm chính

- Bộ đệm cải thiện hiệu suất đáng kể và giảm tải cơ sở dữ liệu
- Redis là tiêu chuẩn cho bộ đệm ứng dụng phía server
- Vô hiệu hóa bộ đệm là phần khó nhất — lên kế hoạch cẩn thận
- Nhiều lớp bộ đệm hoạt động cùng nhau cho hiệu suất tối ưu
