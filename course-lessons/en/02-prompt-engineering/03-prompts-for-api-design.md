# Prompts for API Design

## Designing APIs with AI

API design is a critical skill for web developers. Whether you're building REST endpoints, GraphQL schemas, or tRPC routers, AI can help you design clean, consistent, and well-documented APIs.

## REST API Prompts

### Basic CRUD Endpoint

```
Create a REST API for projects using Next.js 14 App Router.

Endpoint: /api/projects

Methods:
- GET /api/projects — List all projects (paginated, 10 per page)
  - Query params: page, limit, search, sortBy, sortOrder
  - Returns: { data: Project[], pagination: { page, limit, total, totalPages } }

- GET /api/projects/[id] — Get single project
  - Returns: Project with related tasks and tech stack
  - 404 if not found

- POST /api/projects — Create project
  - Body: { name: string, description?: string, techStack: string[], repoUrl?: string }
  - Validation with Zod
  - Returns: created Project with 201 status

- PUT /api/projects/[id] — Update project
  - Partial update supported
  - 404 if not found
  - Returns: updated Project

- DELETE /api/projects/[id] — Delete project
  - Soft delete (set deletedAt)
  - 404 if not found
  - Returns: 204 No Content

Include:
- TypeScript types for all request/response
- Zod schemas for validation
- Error handling with consistent error format
- Authentication check (require login)
- Rate limiting
```

### Nested Resource

```
Create REST API for project tasks (nested under projects).

Endpoints:
- GET /api/projects/[projectId]/tasks — List tasks for project
- POST /api/projects/[projectId]/tasks — Create task in project
- GET /api/projects/[projectId]/tasks/[taskId] — Get single task
- PUT /api/projects/[projectId]/tasks/[taskId] — Update task
- DELETE /api/projects/[projectId]/tasks/[taskId] — Delete task

Task model:
- id: string (UUID)
- title: string
- description?: string
- status: 'todo' | 'in_progress' | 'done'
- priority: 'low' | 'medium' | 'high'
- assigneeId?: string
- dueDate?: Date
- createdAt: Date
- updatedAt: Date

Include proper authorization (only project members can manage tasks).
```

## GraphQL Prompts

### Schema Definition

```
Create a GraphQL schema for a project management API.

Types:
- User: id, name, email, avatar, role, projects, createdAt
- Project: id, name, description, techStack, status, owner, members, tasks, createdAt
- Task: id, title, description, status, priority, assignee, project, dueDate

Queries:
- me: User (current user)
- projects(filter: ProjectFilter, pagination: PaginationInput): ProjectConnection
- project(id: ID!): Project
- tasks(projectId: ID!, filter: TaskFilter): [Task]

Mutations:
- createProject(input: CreateProjectInput!): Project
- updateProject(id: ID!, input: UpdateProjectInput!): Project
- deleteProject(id: ID!): Boolean
- createTask(projectId: ID!, input: CreateTaskInput!): Task
- updateTask(id: ID!, input: UpdateTaskInput!): Task

Include:
- Input types for mutations
- Connection type for pagination
- Filter input types
- Custom scalars (DateTime, URL)
- Error handling with union types
```

### Resolver Implementation

```
Implement GraphQL resolvers for the Project type using Apollo Server with TypeScript.

For the Project type:
- owner: Resolve User from ownerId (batch with DataLoader)
- members: Resolve User[] from memberIds (batch with DataLoader)
- tasks: Resolve Task[] with optional status filter
- taskCount: Computed field counting tasks by status

Include:
- DataLoader for N+1 prevention
- Authentication middleware
- Authorization checks (only members can view private projects)
- Error handling with ApolloError
- TypeScript types generated from schema
```

## tRPC Prompts

### Router Definition

```
Create a tRPC router for projects using Next.js 14.

Router: projectRouter

Procedures:
- list: Public, paginated project list with search
  - Input: { page: number, limit: number, search?: string }
  - Returns: { items: Project[], total: number }

- getById: Public, get project by ID
  - Input: { id: string }
  - Returns: Project with tasks and owner

- create: Protected, create new project
  - Input: { name: string, description?: string, techStack: string[] }
  - Returns: Project

- update: Protected, update project (owner only)
  - Input: { id: string, name?: string, description?: string }
  - Returns: Project

- delete: Protected, delete project (owner only)
  - Input: { id: string }
  - Returns: { success: boolean }

Include:
- Zod input validation
- Authentication middleware
- Authorization checks
- Error handling with TRPCError
- Database queries with Prisma
```

## API Documentation Prompts

### OpenAPI Spec

```
Generate an OpenAPI 3.0 specification for the projects API.

Include:
- All endpoints with methods, paths, and descriptions
- Request/response schemas with examples
- Authentication (Bearer token)
- Error responses (400, 401, 403, 404, 500)
- Pagination parameters
- Filtering and sorting options
- Rate limiting headers

Format: YAML
File: docs/api/openapi.yaml
```

### API Documentation

```
Generate API documentation for the projects endpoint.

Include:
- Overview section explaining the API
- Authentication requirements
- Base URL and versioning
- Each endpoint with:
  - Description
  - HTTP method and path
  - Request parameters and body
  - Response format with examples
  - Error codes and messages
  - Rate limiting info
- Code examples in JavaScript/TypeScript
- Common use cases and workflows

Format: Markdown
File: docs/api/projects.md
```

## Best Practices to Include in Prompts

### Error Handling
```
Use consistent error response format:
{
  error: {
    code: "VALIDATION_ERROR",
    message: "Human-readable message",
    details: [{ field: "name", message: "Name is required" }]
  }
}

Map to HTTP status codes:
- 400: Validation errors
- 401: Authentication required
- 403: Insufficient permissions
- 404: Resource not found
- 409: Conflict (duplicate)
- 429: Rate limit exceeded
- 500: Internal server error
```

### Validation
```
Use Zod for all input validation:
- Validate at the API boundary
- Return detailed validation errors
- Sanitize user input
- Type inference from Zod schemas
```

## Practice Exercise

Design APIs for WebDevHub:

1. **User API** — Registration, login, profile management
2. **Snippet API** — CRUD for code snippets with tagging
3. **Portfolio API** — Public portfolio data for developer profiles

Write complete prompts for each, then implement with your AI tool.

## What's Next

In the next lesson, we'll learn how to prompt AI for debugging — fixing errors, resolving type issues, and troubleshooting common web development problems.
