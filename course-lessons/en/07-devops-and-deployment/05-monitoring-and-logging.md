# Monitoring and Logging with AI

## Learning Objectives
- Set up error tracking with Sentry
- Implement structured logging
- Use AI to configure monitoring

## Why Monitoring Matters

You can't fix what you can't see. Monitoring tells you when things go wrong, logging helps you understand why, and analytics show you how users interact with your application.

## Error Tracking with Sentry

Sentry captures errors in real-time with full context:

```javascript
// Install: npm install @sentry/node

const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0, // Capture 100% of transactions in dev
  integrations: [
    Sentry.expressIntegration(),
    Sentry.mongoIntegration()
  ]
});

// Must be before any middleware
app.use(Sentry.expressRequestHandler());
app.use(Sentry.expressErrorHandler());
```

### Capturing Errors

```javascript
// Automatic - captures unhandled errors
app.get('/api/risky', async (req, res) => {
  // This error is automatically captured
  throw new Error('Something went wrong');
});

// Manual - capture specific errors
try {
  await riskyOperation();
} catch (error) {
  Sentry.captureException(error, {
    tags: { section: 'payment' },
    extra: { userId: req.user.id, orderId: req.body.orderId }
  });
  res.status(500).json({ error: 'Payment failed' });
}
```

## Structured Logging with Winston

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

### Using the Logger

```javascript
const logger = require('../utils/logger');

// Log levels
logger.error('Database connection failed', { error: err.message });
logger.warn('High memory usage', { usage: process.memoryUsage() });
logger.info('User registered', { userId: user.id, email: user.email });
logger.debug('Query executed', { query: 'SELECT * FROM users', duration: '50ms' });
```

## Request Logging

```javascript
// middleware/requestLogger.js
const logger = require('../utils/logger');

const requestLogger = (req, res, next) => {
  const start = Date.now();

  res.on('finish', () => {
    const duration = Date.now() - start;
    logger.info('Request completed', {
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

## Health Check Endpoint

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

## AI Prompt for Monitoring

```
Set up monitoring and logging for a Node.js API with:
1. Sentry for error tracking with context
2. Winston for structured logging
3. Request logging middleware
4. Health check endpoint with database status
5. Performance monitoring
6. Alert configuration for critical errors
7. Log rotation and retention

Include examples for each monitoring type.
```

## Analytics with Mixpanel

```javascript
// Track user actions
const Mixpanel = require('mixpanel');
const mixpanel = Mixpanel.init(process.env.MIXPANEL_TOKEN);

// Track events
mixpanel.track('Task Created', {
  distinct_id: user.id,
  task_id: task.id,
  project_id: task.project
});

// Track user properties
mixpanel.people.set(user.id, {
  $email: user.email,
  $name: user.name,
  tasks_created: 42
});
```

## Practice Exercise

Add monitoring to your Task Management API:
- Set up Sentry for error tracking
- Implement Winston logging
- Create health check endpoint
- Add request logging middleware
- Set up alerts for critical errors

## Key Takeaways

- Sentry captures errors with full context for debugging
- Structured logging makes logs searchable and analyzable
- Health checks enable monitoring and auto-restart
- Analytics help understand user behavior
