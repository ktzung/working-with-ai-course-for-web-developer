# Error Handling with AI

## Learning Objectives
- Implement global error handling patterns
- Create validation middleware
- Design consistent error responses

## Why Error Handling Matters

Users will break your app. They'll submit invalid data, access expired links, and click buttons too fast. Good error handling turns crashes into helpful messages and keeps your application running.

## Error Response Format

Consistency is key. Every error should follow the same structure:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      { "field": "email", "message": "Must be a valid email address" },
      { "field": "password", "message": "Must be at least 8 characters" }
    ]
  }
}
```

## Custom Error Classes

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

// Usage
class NotFoundError extends AppError {
  constructor(resource) {
    super(`${resource} not found`, 404, 'NOT_FOUND');
  }
}

class ValidationError extends AppError {
  constructor(details) {
    super('Validation failed', 400, 'VALIDATION_ERROR');
    this.details = details;
  }
}

class UnauthorizedError extends AppError {
  constructor(message = 'Authentication required') {
    super(message, 401, 'UNAUTHORIZED');
  }
}
```

## Global Error Handler

```javascript
// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  let error = { ...err };
  error.message = err.message;

  // Log error for debugging
  console.error(`[${new Date().toISOString()}] ${err.method} ${err.url}:`, err);

  // Mongoose bad ObjectId
  if (err.name === 'CastError') {
    error = new AppError('Resource not found', 404, 'NOT_FOUND');
  }

  // Mongoose duplicate key
  if (err.code === 11000) {
    const field = Object.keys(err.keyValue)[0];
    error = new AppError(
      `Duplicate value for ${field}`,
      400,
      'DUPLICATE_VALUE'
    );
  }

  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const details = Object.values(err.errors).map(e => ({
      field: e.path,
      message: e.message
    }));
    error = new ValidationError(details);
  }

  // JWT errors
  if (err.name === 'JsonWebTokenError') {
    error = new AppError('Invalid token', 401, 'INVALID_TOKEN');
  }
  if (err.name === 'TokenExpiredError') {
    error = new AppError('Token expired', 401, 'TOKEN_EXPIRED');
  }

  res.status(error.statusCode || 500).json({
    success: false,
    error: {
      code: error.code || 'INTERNAL_ERROR',
      message: error.isOperational ? error.message : 'Something went wrong',
      ...(error.details && { details: error.details }),
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};

module.exports = errorHandler;
```

## Async Error Wrapper

Avoid try-catch in every controller:

```javascript
// middleware/asyncHandler.js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
router.get('/products', asyncHandler(async (req, res) => {
  const products = await Product.find();
  res.json({ success: true, data: products });
}));
```

## Validation Middleware with Zod

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

// Schema definition
const registerSchema = z.object({
  email: z.string().email('Invalid email format'),
  password: z.string().min(8, 'Password must be at least 8 characters')
    .regex(/[A-Z]/, 'Must contain uppercase letter')
    .regex(/[0-9]/, 'Must contain a number'),
  name: z.string().min(2).max(50)
});

// Route usage
router.post('/register', validate(registerSchema), authController.register);
```

## AI Prompt for Error Handling

```
Create a comprehensive error handling system for Express.js with:
1. Custom error classes (AppError, NotFound, Validation, Auth, Forbidden)
2. Global error handler middleware
3. Async error wrapper
4. Request validation with Zod schemas
5. Error logging with Winston
6. Different responses for development vs production
7. Unhandled rejection and uncaught exception handlers

Include examples of using each error type in controllers.
```

## 404 Handler

```javascript
// Catch undefined routes
app.use('*', (req, res) => {
  res.status(404).json({
    success: false,
    error: {
      code: 'NOT_FOUND',
      message: `Route ${req.method} ${req.originalUrl} not found`
    }
  });
});
```

## Practice Exercise

Add comprehensive error handling to your CRUD API:
- Custom error classes for each error type
- Global error handler with logging
- Input validation with Zod for all endpoints
- Proper error responses for database errors
- Development vs production error details

## Key Takeaways

- Consistent error format helps frontend handle errors gracefully
- Custom error classes make errors descriptive and actionable
- Global error handler catches everything in one place
- Validation prevents bad data before it reaches your database
