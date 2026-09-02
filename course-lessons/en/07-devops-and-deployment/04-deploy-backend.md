# Deploy Backend with AI

## Learning Objectives
- Deploy Node.js APIs to Railway and Render
- Configure production databases
- Use AI to set up deployments

## Why Backend Deployment Matters

Your API needs to be reliable, scalable, and secure. Modern platforms handle the infrastructure so you can focus on code.

## Railway (Recommended for Beginners)

Railway makes deployment simple with automatic scaling.

### Deploy with Git

1. Push code to GitHub
2. Go to [railway.app](https://railway.app)
3. Click "New Project"
4. Select "Deploy from GitHub repo"
5. Railway detects Node.js and deploys automatically

### Railway Configuration

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

### Add Database

1. Click "New" > "Database" > "MongoDB"
2. Railway provisions a MongoDB instance
3. Connection string is automatically added as `MONGO_URL` environment variable

### Environment Variables

```bash
# Set via Railway CLI
railway variables set NODE_ENV=production
railway variables set JWT_SECRET=your-secret-key

# Or in Railway Dashboard:
# Variables tab
```

## Render

Render offers free tier and easy scaling.

### Deploy with Git

1. Push code to GitHub
2. Go to [render.com](https://render.com)
3. Click "New" > "Web Service"
4. Connect your repository
5. Configure:
   - Build Command: `npm install`
   - Start Command: `npm start`
6. Click "Create Web Service"

### Render Configuration

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

### Free Tier Limitations

- Services spin down after 15 minutes of inactivity
- First request after spin-down takes ~30 seconds
- 750 hours/month free

## AI Prompt for Backend Deployment

```
Configure deployment for a Node.js API with:
1. Railway or Render setup with automatic deployments
2. MongoDB Atlas database configuration
3. Environment variables for production
4. Health check endpoints
5. Graceful shutdown handling
6. Error logging and monitoring
7. Auto-scaling configuration

Include railway.toml or render.yaml configurations.
```

## MongoDB Atlas (Production Database)

```javascript
// Connect to MongoDB Atlas
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      // Modern Mongoose doesn't need these options
    });
    console.log(`MongoDB Connected: ${conn.connection.host}`);
  } catch (error) {
    console.error(`Error: ${error.message}`);
    process.exit(1);
  }
};
```

## Production Best Practices

```javascript
// app.js - Production configuration
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const compression = require('compression');
const rateLimit = require('express-rate-limit');

const app = express();

// Security headers
app.use(helmet());

// CORS
app.use(cors({
  origin: process.env.CLIENT_URL,
  credentials: true
}));

// Compression
app.use(compression());

// Rate limiting
app.use(rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
}));

// Health check
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date() });
});

// Graceful shutdown
process.on('SIGTERM', () => {
  console.log('SIGTERM received. Shutting down gracefully...');
  server.close(() => {
    mongoose.connection.close(false, () => {
      process.exit(0);
    });
  });
});
```

## Practice Exercise

Deploy your Task Management API:
- Set up Railway or Render account
- Configure MongoDB Atlas database
- Set environment variables
- Deploy and test the API
- Set up monitoring

## Key Takeaways

- Railway and Render simplify backend deployment
- MongoDB Atlas provides managed database hosting
- Environment variables keep secrets secure
- Health checks enable monitoring and auto-restart
