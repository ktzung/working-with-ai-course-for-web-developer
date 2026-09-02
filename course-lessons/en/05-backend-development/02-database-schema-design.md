# Database Schema Design with AI

## Learning Objectives
- Design database schemas using AI assistance
- Understand SQL vs NoSQL trade-offs
- Create relationships, indexes, and migrations

## Choosing Your Database

The first decision: SQL or NoSQL? AI can help you decide based on your data patterns.

**SQL (PostgreSQL, MySQL)** — Best for:
- Structured data with clear relationships
- Transactions that must be ACID-compliant
- Complex queries with joins
- Examples: E-commerce, banking, CRM systems

**NoSQL (MongoDB, Firebase)** — Best for:
- Flexible, evolving schemas
- Document-oriented data
- Rapid prototyping
- Examples: Content management, real-time apps, IoT

## AI Prompt for Schema Design

```
Design a database schema for a project management tool with:
- Users (name, email, role, avatar)
- Projects (name, description, owner, members, status)
- Tasks (title, description, assignee, priority, due date, status)
- Comments (author, content, attachments)
- Time entries (user, task, hours, date)

For each entity, specify:
1. Fields with data types
2. Relationships (one-to-many, many-to-many)
3. Indexes for performance
4. Validation rules

Use PostgreSQL with Prisma ORM syntax.
```

## SQL Schema with Prisma

Here's what AI generates for a project management schema:

```prisma
// schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  avatar    String?
  role      Role     @default(MEMBER)
  projects  ProjectMember[]
  tasks     Task[]    @relation("AssignedTasks")
  comments  Comment[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
}

enum Role {
  ADMIN
  MANAGER
  MEMBER
}

model Project {
  id          String   @id @default(cuid())
  name        String
  description String?
  status      ProjectStatus @default(ACTIVE)
  ownerId     String
  owner       User     @relation(fields: [ownerId], references: [id])
  members     ProjectMember[]
  tasks       Task[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([ownerId])
  @@index([status])
}

model Task {
  id          String   @id @default(cuid())
  title       String
  description String?
  priority    Priority @default(MEDIUM)
  status      TaskStatus @default(TODO)
  dueDate     DateTime?
  projectId   String
  project     Project  @relation(fields: [projectId], references: [id])
  assigneeId  String?
  assignee    User?    @relation("AssignedTasks", fields: [assigneeId], references: [id])
  comments    Comment[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([projectId])
  @@index([assigneeId])
  @@index([status])
}
```

## MongoDB Schema Alternative

For NoSQL with Mongoose:

```javascript
// models/Project.js
const projectSchema = new mongoose.Schema({
  name: { type: String, required: true, trim: true },
  description: String,
  status: {
    type: String,
    enum: ['ACTIVE', 'ARCHIVED', 'COMPLETED'],
    default: 'ACTIVE'
  },
  owner: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  members: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    role: { type: String, enum: ['ADMIN', 'MEMBER', 'VIEWER'] },
    joinedAt: { type: Date, default: Date.now }
  }],
  tags: [String]
}, { timestamps: true });

projectSchema.index({ owner: 1, status: 1 });
projectSchema.index({ 'members.user': 1 });
```

## Relationships Pattern

**One-to-Many**: A user has many tasks
```prisma
model User {
  tasks Task[]
}
model Task {
  userId String
  user   User   @relation(fields: [userId], references: [id])
}
```

**Many-to-Many**: Users belong to many projects
```prisma
model ProjectMember {
  userId    String
  projectId String
  role      Role   @default(MEMBER)
  user      User    @relation(fields: [userId], references: [id])
  project   Project @relation(fields: [projectId], references: [id])
  @@id([userId, projectId])
}
```

## Indexing Strategy

AI can suggest indexes based on your query patterns:

```
Given these common queries:
1. Find tasks by project and status
2. Find tasks assigned to a user
3. Search projects by name
4. Get recent comments for a task

Suggest indexes for optimal performance.
```

AI will recommend composite indexes like:
```prisma
@@index([projectId, status])  // Query 1
@@index([assigneeId, status]) // Query 2
```

## Migrations

When your schema evolves, migrations track changes:

```bash
# Generate migration
npx prisma migrate dev --name add-task-priority

# Apply migrations
npx prisma migrate deploy
```

## Practice Exercise

Design a database schema for a social media platform:
- Users with profiles and follower relationships
- Posts with images, likes, and comments
- Direct messages between users
- Notifications system

Use AI to generate both Prisma and Mongoose versions, then compare the approaches.

## Key Takeaways

- SQL suits structured relational data; NoSQL suits flexible documents
- AI generates complete schemas from natural language requirements
- Indexes are critical for query performance — design them around your access patterns
- Migrations let you evolve your schema safely over time
