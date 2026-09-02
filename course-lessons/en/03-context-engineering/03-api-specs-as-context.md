# API Specs as Context

## Using API Specifications to Guide AI

API specifications are one of the most powerful forms of context for AI. When you share your OpenAPI/Swagger spec, GraphQL schema, or tRPC router definition, AI can generate code that perfectly matches your API contracts.

## Why API Specs Matter

Without API specs, AI might:
- Guess field names (is it `userName` or `name` or `username`?)
- Miss required fields
- Use wrong data types
- Forget about pagination
- Ignore error responses

With API specs, AI will:
- Use exact field names and types
- Handle all response formats
- Include proper error handling
- Follow your pagination pattern

## OpenAPI/Swagger Specs

### Creating an OpenAPI Spec

```yaml
# openapi.yaml
openapi: 3.0.0
info:
  title: WebDevHub API
  version: 1.0.0
  description: API for WebDevHub project management platform

servers:
  - url: http://localhost:3000/api
    description: Local development

paths:
  /projects:
    get:
      summary: List projects
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
        - name: search
          in: query
          schema:
            type: string
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Project'
                  pagination:
                    $ref: '#/components/schemas/Pagination'
        '401':
          $ref: '#/components/responses/Unauthorized'

    post:
      summary: Create project
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateProjectInput'
      responses:
        '201':
          description: Created
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Project'
        '400':
          $ref: '#/components/responses/ValidationError'

components:
  schemas:
    Project:
      type: object
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
        description:
          type: string
        techStack:
          type: array
          items:
            type: string
        status:
          type: string
          enum: [active, archived, draft]
        ownerId:
          type: string
          format: uuid
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time

    CreateProjectInput:
      type: object
      required:
        - name
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 100
        description:
          type: string
          maxLength: 500
        techStack:
          type: array
          items:
            type: string
          maxItems: 20

    Pagination:
      type: object
      properties:
        page:
          type: integer
        limit:
          type: integer
        total:
          type: integer
        totalPages:
          type: integer

  responses:
    Unauthorized:
      description: Authentication required
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    ValidationError:
      description: Validation failed
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'

    Error:
      type: object
      properties:
        error:
          type: object
          properties:
            code:
              type: string
            message:
              type: string
            details:
              type: array
              items:
                type: object
                properties:
                  field:
                    type: string
                  message:
                    type: string
```

### Using OpenAPI with AI

When prompting AI, reference the spec:

```
I have an OpenAPI spec at docs/api/openapi.yaml.

Generate a TypeScript API client for the projects endpoints that:
- Uses the native fetch API
- Includes all types from the spec
- Handles pagination
- Handles errors with the standard error format
- Includes request/response interceptors
```

## GraphQL Schemas

### Schema as Context

```graphql
# schema.graphql
type User {
  id: ID!
  name: String!
  email: String!
  avatar: String
  role: Role!
  projects: [Project!]!
  createdAt: DateTime!
}

type Project {
  id: ID!
  name: String!
  description: String
  techStack: [String!]!
  status: ProjectStatus!
  owner: User!
  members: [User!]!
  tasks: [Task!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Task {
  id: ID!
  title: String!
  description: String
  status: TaskStatus!
  priority: Priority!
  assignee: User
  project: Project!
  dueDate: DateTime
}

enum Role {
  ADMIN
  MEMBER
  VIEWER
}

enum ProjectStatus {
  ACTIVE
  ARCHIVED
  DRAFT
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  DONE
}

enum Priority {
  LOW
  MEDIUM
  HIGH
}

type Query {
  me: User!
  projects(filter: ProjectFilter, pagination: PaginationInput): ProjectConnection!
  project(id: ID!): Project
  tasks(projectId: ID!, filter: TaskFilter): [Task!]!
}

type Mutation {
  createProject(input: CreateProjectInput!): Project!
  updateProject(id: ID!, input: UpdateProjectInput!): Project!
  deleteProject(id: ID!): Boolean!
  createTask(projectId: ID!, input: CreateTaskInput!): Task!
  updateTask(id: ID!, input: UpdateTaskInput!): Task!
}
```

### Using GraphQL Schema with AI

```
Here's my GraphQL schema: [paste schema]

Generate React hooks for:
- useProjects (list with filter and pagination)
- useProject (single project by ID)
- useCreateProject (mutation)
- useUpdateProject (mutation)

Use Apollo Client with TypeScript.
Include proper loading, error, and success states.
```

## tRPC Router Definitions

### Router as Context

```typescript
// server/routers/project.ts
import { z } from 'zod';
import { router, protectedProcedure } from '../trpc';

export const projectRouter = router({
  list: protectedProcedure
    .input(z.object({
      page: z.number().min(1).default(1),
      limit: z.number().min(1).max(100).default(10),
      search: z.string().optional(),
    }))
    .query(async ({ input, ctx }) => {
      // Implementation
    }),

  getById: protectedProcedure
    .input(z.object({ id: z.string().uuid() }))
    .query(async ({ input, ctx }) => {
      // Implementation
    }),

  create: protectedProcedure
    .input(z.object({
      name: z.string().min(1).max(100),
      description: z.string().max(500).optional(),
      techStack: z.array(z.string()).max(20),
    }))
    .mutation(async ({ input, ctx }) => {
      // Implementation
    }),
});
```

### Using tRPC Definitions with AI

```
Here's my tRPC project router: [paste router]

Generate a React component that:
- Lists projects with search and pagination
- Uses the trpc.project.list.useQuery hook
- Shows loading skeleton while fetching
- Handles errors gracefully
- Includes a search input with debounce
```

## Combining Multiple Specs

For complex projects, combine specs:

```
Context for this task:

1. OpenAPI spec: [paste relevant endpoints]
2. Prisma schema: [paste relevant models]
3. TypeScript types: [paste relevant types]

Generate a service layer that:
- Implements the API endpoints
- Uses Prisma for database queries
- Returns typed responses matching the OpenAPI spec
```

## Practice Exercise

1. Create an OpenAPI spec for WebDevHub's snippet API
2. Generate a TypeScript client using the spec
3. Create React hooks that use the client

## What's Next

In the next lesson, we'll set up coding standards as context — ensuring AI-generated code follows your team's conventions.
