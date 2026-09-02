# Practice: Deploy a Complete Application

## Learning Objectives
- Apply all deployment concepts in a real project
- Deploy frontend and backend to production
- Set up monitoring and logging

## Project: Deploy Task Management App

Deploy your complete Task Management application with frontend, backend, database, and monitoring.

## Architecture

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

## Step 1: Prepare the Backend

```bash
# Add production dependencies
npm install helmet compression cors express-rate-limit

# Create health check endpoint
# Add graceful shutdown
# Configure environment variables
```

## Step 2: Deploy Backend to Railway

1. Create Railway account
2. Connect GitHub repository
3. Add MongoDB service
4. Add Redis service
5. Set environment variables:
   ```
   NODE_ENV=production
   JWT_SECRET=your-secret
   MONGODB_URI=${{MongoDB.MONGO_URL}}
   REDIS_URL=${{Redis.REDIS_URL}}
   ```
6. Deploy and test

## Step 3: Deploy Frontend to Vercel

1. Create Vercel account
2. Import GitHub repository
3. Set environment variables:
   ```
   NEXT_PUBLIC_API_URL=https://your-api.railway.app
   ```
4. Deploy and test

## Step 4: Set Up Monitoring

```javascript
// Add to backend
const Sentry = require('@sentry/node');
const logger = require('./utils/logger');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV
});

// Add request logging
app.use(requestLogger);

// Add health check
app.get('/health', healthCheck);
```

## Step 5: Configure CI/CD

```yaml
# .github/workflows/deploy.yml
name: Deploy

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

## Step 6: Custom Domain

### Backend (Railway)
1. Go to Settings > Domains
2. Add custom domain: `api.yourdomain.com`
3. Configure DNS CNAME record

### Frontend (Vercel)
1. Go to Settings > Domains
2. Add custom domain: `yourdomain.com`
3. Configure DNS records

## Step 7: SSL and Security

- SSL is automatic on both platforms
- Enable HSTS headers
- Configure CORS for production domain
- Set up rate limiting

## Deliverables

1. ✅ Backend deployed to Railway
2. ✅ Frontend deployed to Vercel
3. ✅ MongoDB Atlas database
4. ✅ Redis caching
5. ✅ Custom domains with SSL
6. ✅ CI/CD pipeline
7. ✅ Error tracking with Sentry
8. ✅ Structured logging
9. ✅ Health checks

## Production Checklist

- [ ] Environment variables set
- [ ] Database backups configured
- [ ] Error tracking enabled
- [ ] Logging configured
- [ ] Health checks working
- [ ] Rate limiting enabled
- [ ] CORS configured
- [ ] SSL active
- [ ] Custom domain working
- [ ] CI/CD pipeline tested

## Key Takeaways

- Modern platforms make deployment accessible
- CI/CD automates the deployment process
- Monitoring catches issues before users do
- Security must be configured for production
