# Caching Strategies with AI

## Learning Objectives
- Implement caching at multiple levels
- Use Redis for server-side caching
- Configure CDN and browser caching

## Why Caching Matters

Caching stores frequently accessed data closer to where it's needed. It reduces database load, decreases response times, and improves user experience.

**Without caching**: Every request hits the database
**With caching**: Repeated requests served from memory (milliseconds vs. hundreds of milliseconds)

## Caching Levels

1. **Browser cache**: CSS, JS, images stored on user's device
2. **CDN cache**: Static assets served from edge servers worldwide
3. **Application cache**: Frequently queried data stored in Redis
4. **Database cache**: Query results cached by the database engine

## Redis Caching

Redis is an in-memory data store, perfect for caching:

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

## Cache Middleware

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

    // Override res.json to cache the response
    const originalJson = res.json.bind(res);
    res.json = async (data) => {
      await cache.set(key, data, ttl);
      originalJson(data);
    };

    next();
  } catch (error) {
    next(); // Continue without cache on error
  }
};

// Usage
router.get('/products', cacheMiddleware(600), productController.getProducts);
```

## Cache Invalidation

The hardest problem in caching — keeping cache fresh:

```javascript
// Invalidate cache when data changes
exports.createProduct = async (req, res) => {
  const product = await Product.create(req.body);

  // Clear related caches
  await cache.clearPattern('cache:/api/products*');
  await cache.del('cache:product-count');

  res.status(201).json({ success: true, data: product });
};

// Cache-aside pattern
async function getProductsWithCache() {
  // Try cache first
  let products = await cache.get('products:all');

  if (!products) {
    // Cache miss — fetch from database
    products = await Product.find().lean();
    // Store in cache for 10 minutes
    await cache.set('products:all', products, 600);
  }

  return products;
}
```

## CDN Configuration

```javascript
// Express static files with cache headers
app.use('/static', express.static('public', {
  maxAge: '1y', // Cache for 1 year
  immutable: true
}));

// API responses with cache headers
app.get('/api/products', (req, res) => {
  res.set({
    'Cache-Control': 'public, max-age=300', // 5 minutes
    'ETag': generateETag(products)
  });
  res.json(products);
});
```

## Browser Caching Headers

```javascript
// Different cache strategies per content type
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

## AI Prompt for Caching

```
Implement a caching strategy for a Node.js API with:
1. Redis cache with configurable TTL per endpoint
2. Cache middleware for GET requests
3. Cache invalidation on POST/PUT/DELETE
4. Cache-aside pattern for database queries
5. CDN configuration for static assets
6. Browser caching headers for different content types
7. Cache warming for frequently accessed data

Include monitoring for cache hit rates.
```

## React Query Caching

```javascript
// Client-side caching with React Query
const { data: products } = useQuery({
  queryKey: ['products'],
  queryFn: fetchProducts,
  staleTime: 5 * 60 * 1000, // Consider fresh for 5 minutes
  cacheTime: 30 * 60 * 1000, // Keep in cache for 30 minutes
  refetchOnWindowFocus: true
});
```

## Practice Exercise

Add caching to your Task Management API:
- Redis cache for task lists and project details
- Cache invalidation when tasks are created/updated/deleted
- CDN configuration for uploaded files
- Browser caching for static assets
- Monitor cache hit rates

## Key Takeaways

- Caching dramatically improves performance and reduces database load
- Redis is the standard for server-side application caching
- Cache invalidation is the hardest part — plan it carefully
- Multiple caching layers work together for optimal performance
