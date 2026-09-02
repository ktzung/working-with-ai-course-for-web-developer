# API Design with AI

## Learning Objectives
- Design RESTful APIs using AI assistance
- Understand endpoint naming conventions and HTTP methods
- Create middleware patterns for common functionality

## Why API Design Matters

A well-designed API is the backbone of any web application. It defines how your frontend communicates with your backend, how third-party services integrate with your system, and how scalable your application becomes over time.

Think of an API as a restaurant menu — it tells clients exactly what's available, how to order it, and what they'll get back. Bad API design is like a confusing menu: customers get frustrated and leave.

## RESTful API Principles

REST (Representational State Transfer) is the most common API architecture. Here are the core principles:

**Resource-Based URLs**: Every endpoint represents a resource (noun, not verb).

```
✅ Good:  GET /api/users, POST /api/orders
❌ Bad:   GET /api/getUsers, POST /api/createOrder
```

**HTTP Methods Define Actions**:
- `GET` — Retrieve data (read-only)
- `POST` — Create new resource
- `PUT` — Update entire resource
- `PATCH` — Partial update
- `DELETE` — Remove resource

**Status Codes Communicate Results**:
- `200` — Success
- `201` — Created
- `400` — Bad request (client error)
- `401` — Unauthorized
- `404` — Not found
- `500` — Server error

## Using AI to Design Your API

Here's a powerful prompt for designing APIs:

```
Design a RESTful API for an e-commerce platform with:
- User management (register, login, profile)
- Product catalog (CRUD operations)
- Shopping cart (add, remove, checkout)
- Order management (create, track, cancel)

Include:
1. Endpoint URLs with HTTP methods
2. Request/response body examples
3. Authentication requirements per endpoint
4. Common error responses
```

AI will generate a complete API specification that you can refine and implement.

## Express.js API Structure

Here's a typical Express.js API organized with AI assistance:

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

## Middleware Pattern

Middleware functions execute before your route handlers. Common uses:

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

## AI Prompt for Middleware Generation

```
Create Express.js middleware for:
1. Request logging (method, URL, timestamp, response time)
2. Rate limiting (100 requests per 15 minutes per IP)
3. CORS configuration (allow specific origins)
4. Request validation using Joi schemas

Include error handling and make each middleware configurable.
```

## Best Practices

1. **Version your API**: `/api/v1/users` allows future changes without breaking clients
2. **Use plural nouns**: `/users` not `/user` — you're representing a collection
3. **Nest logically**: `/users/:id/orders` shows the relationship
4. **Paginate lists**: Never return unlimited data — use `?page=1&limit=20`
5. **Filter and sort**: `?sort=createdAt&order=desc&status=active`

## Practice Exercise

Design an API for a blog platform with:
- User authentication
- Posts (create, read, update, delete)
- Comments on posts
- Tags and categories
- Search functionality

Use AI to generate the complete endpoint list, then implement the routes in Express.js.

## Key Takeaways

- RESTful APIs use resources, HTTP methods, and status codes
- AI can generate complete API specifications from requirements
- Middleware handles cross-cutting concerns like auth and logging
- Good API design is consistent, predictable, and well-documented
