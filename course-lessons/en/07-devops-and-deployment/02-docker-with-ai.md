# Docker with AI

## Learning Objectives
- Create Dockerfiles for Node.js applications
- Use docker-compose for multi-container setups
- Use AI to generate Docker configurations

## Why Docker Matters

Docker packages your application with all its dependencies into a container. This ensures your app runs the same way everywhere — your laptop, your teammate's laptop, and the production server.

**Without Docker**: "It works on my machine" 🤷
**With Docker**: "It works everywhere" ✅

## Dockerfile Basics

```dockerfile
# Dockerfile for Node.js
FROM node:20-alpine

WORKDIR /app

# Copy package files first (for caching)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Start application
CMD ["node", "src/index.js"]
```

## Multi-Stage Build

Smaller production images:

```dockerfile
# Build stage
FROM node:20-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:20-alpine AS production

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist

EXPOSE 3000
CMD ["node", "dist/index.js"]
```

## Docker Compose

Run multiple services together:

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

## Development with Docker

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

## AI Prompt for Docker

```
Create Docker configuration for a full-stack application with:
1. Multi-stage Dockerfile for Node.js backend
2. Dockerfile for React frontend with Nginx
3. docker-compose.yml with app, database, Redis, and Nginx
4. Development and production configurations
5. Volume mounts for data persistence
6. Health checks for all services
7. Environment variable management

Include .dockerignore and nginx.conf.
```

## Health Checks

```dockerfile
# Health check in Dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

```yaml
# Health check in docker-compose
services:
  app:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

## Docker Commands

```bash
# Build image
docker build -t myapp .

# Run container
docker run -p 3000:3000 myapp

# Docker Compose
docker-compose up -d        # Start all services
docker-compose down          # Stop all services
docker-compose logs -f       # View logs
docker-compose ps            # List running services

# Cleanup
docker system prune -a       # Remove unused resources
```

## Practice Exercise

Dockerize your Task Management application:
- Create Dockerfile for the backend
- Create docker-compose with MongoDB and Redis
- Set up development and production configurations
- Add health checks
- Test the complete setup

## Key Takeaways

- Docker ensures consistent environments across development and production
- Multi-stage builds create smaller production images
- Docker Compose manages multi-service applications
- Health checks monitor container status
