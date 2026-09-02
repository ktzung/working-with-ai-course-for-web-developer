# Phase 3: Building the Backend

## The Engine Behind TaskFlow

The frontend is the face of your application, but the backend is the brain. In this phase, you'll build the API, database, authentication system, and real-time features that power TaskFlow.

## Setting Up the Express Server

### Project Structure

"Help me organize the Express backend for TaskFlow with a clean folder structure: routes, controllers, middleware, services, and models."

AI will create:
- Route definitions for each resource
- Controller functions with business logic
- Middleware for auth, validation, and error handling
- Service layer for database operations
- Model definitions with Prisma

### Error Handling

"Set up centralized error handling for the Express API. Include custom error classes, async error catching, and consistent error responses."

AI will implement:
- Custom AppError class with status codes
- Async wrapper to catch promise rejections
- Global error handler middleware
- Consistent error response format

## Database Setup with Prisma

### Schema Implementation

"Help me implement the Prisma schema for TaskFlow based on our design. Include all models, relations, and indexes."

AI will create:
- User model with auth fields
- Workspace and membership models
- Board, column, and task models
- Comment and attachment models
- Proper relations and cascading deletes

### Database Operations

"Write Prisma service functions for TaskFlow: create workspace, add member, create board, move task, and add comment."

AI will generate type-safe database operations with:
- Transaction support for multi-step operations
- Proper error handling
- Pagination for list queries
- Filtering and sorting

## Authentication System

### JWT Implementation

"Implement JWT authentication for TaskFlow. Include registration, login, token refresh, and password reset."

AI will build:
- Registration with password hashing (bcrypt)
- Login with JWT token generation
- Token refresh mechanism
- Password reset with email verification
- Middleware to protect routes

### Role-Based Access Control

"Implement role-based access control for TaskFlow. Users can be workspace admins, members, or viewers with different permissions."

AI will create:
- Role definitions and permission matrices
- Middleware to check permissions
- Route protection based on roles
- Invitation system with role assignment

## Real-Time Features with Socket.io

### WebSocket Setup

"Set up Socket.io for real-time updates in TaskFlow. Users should see live changes when teammates update boards."

AI will implement:
- Socket.io server integration with Express
- Room-based messaging (one room per workspace)
- Event handlers for task updates, moves, and comments
- Authentication for WebSocket connections

### Real-Time Events

"Implement real-time events for TaskFlow: task created, task moved, task updated, comment added, and user joined."

AI will create:
- Event emitters for each action
- Broadcast to appropriate rooms
- Optimistic update support for frontend
- Reconnection handling

## File Upload System

"Implement file upload for TaskFlow. Users should be able to attach files to tasks with preview support."

AI will build:
- Multer middleware for file handling
- File type validation and size limits
- Local storage or S3 integration
- Thumbnail generation for images
- File metadata storage in database

## API Validation

"Add request validation to all TaskFlow API endpoints using Zod."

AI will implement:
- Zod schemas for each endpoint
- Validation middleware
- Detailed validation error messages
- Type inference for TypeScript

## Deliverables for Phase 3

- [ ] Express server with clean architecture
- [ ] Prisma schema and database operations
- [ ] JWT authentication system
- [ ] Role-based access control
- [ ] Socket.io real-time features
- [ ] File upload system
- [ ] Request validation with Zod
- [ ] API documentation

## Key Takeaway

AI handles the repetitive parts of backend development — CRUD operations, authentication boilerplate, validation schemas — while you focus on business logic and architecture decisions. The result is a robust, well-structured backend ready for production.
