# Practice: Build a Complete REST API

## Learning Objectives
- Apply all backend concepts in a real project
- Build a production-ready API from scratch
- Practice AI-assisted development workflow

## Project: Task Management API

Build a complete REST API for a task management application. This project combines everything from Section 5.

## Requirements

**Users**:
- Register with email/password
- Login with JWT
- Profile management (update, avatar upload)
- Role-based access (admin, manager, member)

**Projects**:
- Create, read, update, delete projects
- Add/remove members
- Project settings and status

**Tasks**:
- CRUD operations with rich fields
- Assign to team members
- Priority levels, due dates, status tracking
- Comments and attachments

**Advanced Features**:
- Search and filtering
- Pagination
- File uploads
- Email notifications

## Step 1: Project Setup

```bash
mkdir task-api && cd task-api
npm init -y
npm install express mongoose dotenv bcryptjs jsonwebtoken
npm install multer cloudinary zod cors helmet morgan
npm install -D nodemon jest supertest
```

## Step 2: Use AI to Generate the Foundation

```
Create a complete Express.js project structure for a task management API with:
1. MVC architecture (models, controllers, routes, middleware)
2. MongoDB connection with Mongoose
3. Environment configuration with dotenv
4. Error handling middleware
5. Authentication middleware with JWT
6. Input validation with Zod
7. CORS and security headers

Generate all boilerplate files including package.json scripts.
```

## Step 3: Implement Models

```javascript
// models/Task.js
const taskSchema = new mongoose.Schema({
  title: { type: String, required: true, trim: true },
  description: { type: String },
  status: {
    type: String,
    enum: ['TODO', 'IN_PROGRESS', 'REVIEW', 'DONE'],
    default: 'TODO'
  },
  priority: {
    type: String,
    enum: ['LOW', 'MEDIUM', 'HIGH', 'URGENT'],
    default: 'MEDIUM'
  },
  dueDate: { type: Date },
  project: { type: mongoose.Schema.Types.ObjectId, ref: 'Project', required: true },
  assignee: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  creator: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  tags: [String],
  attachments: [{
    url: String,
    name: String,
    size: Number,
    uploadedAt: { type: Date, default: Date.now }
  }],
  comments: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    content: { type: String, required: true },
    createdAt: { type: Date, default: Date.now }
  }]
}, { timestamps: true });
```

## Step 4: Build Controllers

Use AI to generate each controller:

```
Create a task controller with Express.js and Mongoose that handles:
1. createTask - validate input, check project membership, create task
2. getTasks - filter by project, status, assignee; sort and paginate
3. getTask - find by ID with populated references
4. updateTask - partial updates with validation
5. deleteTask - soft delete with permission check
6. addComment - add comment to task
7. updateStatus - change task status with validation

Include proper error handling and authorization checks.
```

## Step 5: Add File Uploads

```javascript
// routes/tasks.js
const upload = require('../middleware/upload');

router.post('/:id/attachments',
  auth,
  upload.array('files', 5),
  taskController.addAttachments
);

router.delete('/:id/attachments/:attachmentId',
  auth,
  taskController.removeAttachment
);
```

## Step 6: Testing

```javascript
// tests/tasks.test.js
const request = require('supertest');
const app = require('../app');

describe('Tasks API', () => {
  let token, projectId;

  beforeAll(async () => {
    // Setup test user and project
    const res = await request(app)
      .post('/api/auth/register')
      .send({ email: 'test@test.com', password: 'password123', name: 'Test' });
    token = res.body.data.token;
  });

  test('POST /api/tasks - create task', async () => {
    const res = await request(app)
      .post('/api/tasks')
      .set('Authorization', `Bearer ${token}`)
      .send({
        title: 'Test Task',
        project: projectId,
        priority: 'HIGH'
      });

    expect(res.status).toBe(201);
    expect(res.body.data.title).toBe('Test Task');
  });
});
```

## Step 7: API Documentation

Use AI to generate Swagger/OpenAPI docs:

```
Generate OpenAPI 3.0 documentation for this task management API including:
- All endpoints with request/response schemas
- Authentication requirements
- Error response schemas
- Example requests and responses
```

## Deliverables

By the end of this practice, you should have:
1. ✅ Complete Express.js API with MVC structure
2. ✅ MongoDB models with relationships
3. ✅ JWT authentication with role-based access
4. ✅ CRUD operations for all resources
5. ✅ File upload with cloud storage
6. ✅ Input validation and error handling
7. ✅ API documentation
8. ✅ Basic test suite

## Key Takeaways

- Building a complete API requires combining many concepts
- AI accelerates development by generating boilerplate code
- Testing ensures your API works correctly
- Documentation makes your API usable by others
