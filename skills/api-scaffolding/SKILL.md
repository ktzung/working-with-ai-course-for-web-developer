# Skill: API Scaffolding

> Scaffold REST and GraphQL API endpoints with proper structure, validation, and error handling.

## Purpose

This skill enables AI tools to generate complete API endpoints including route handlers, validation schemas, database operations, authentication checks, and tests — following Next.js App Router conventions.

## When to Use

- Creating new CRUD endpoints
- Building GraphQL resolvers
- Adding authentication-protected routes
- Scaffolding webhook handlers
- Setting up file upload endpoints

## Instructions

### Step 1: Define the API Contract

Before scaffolding, document:
1. **Resource** — What entity does this API manage?
2. **Methods** — Which HTTP methods (GET, POST, PUT, DELETE)?
3. **Auth** — Public or authenticated?
4. **Validation** — What fields are required/optional?
5. **Relations** — Does it include related data?

### Step 2: Generate Route Handler

```
Create a Next.js API route at app/api/{resource}/route.ts.

Methods: {GET, POST}
Database: Prisma ORM
Model: {Prisma model}

Requirements:
- Zod schema for request body validation
- Authentication via NextAuth session
- Proper HTTP status codes
- Consistent response format: { success, data, error }
- Pagination for GET list (page, limit query params)
- Include related data where appropriate
```

### Step 3: Generate Validation Schema

```
Create Zod schemas for the {resource} API.

Schemas needed:
- CreateSchema: fields required for creation
- UpdateSchema: all fields optional
- QuerySchema: query parameters with defaults

Include:
- String length limits
- Email/URL format validation
- Enum values for status fields
- Custom error messages
```

### Step 4: Generate Tests

```
Write API integration tests for /api/{resource}.

Test cases:
1. GET returns list with pagination
2. POST creates resource with valid data
3. POST returns 400 with invalid data
4. GET by ID returns resource
5. GET by ID returns 404 for nonexistent
6. PUT updates resource
7. DELETE removes resource
8. Unauthenticated requests return 401

Use: Jest + supertest or Next.js testing utilities
```

## Output Structure

```
app/api/{resource}/
├── route.ts              # Main route handler (GET, POST)
├── [id]/
│   └── route.ts          # Dynamic route (GET, PUT, DELETE by ID)
├── schemas.ts            # Zod validation schemas
├── types.ts              # TypeScript types
└── __tests__/
    └── route.test.ts     # Integration tests
```

## Response Format Standard

```typescript
// Success
{ success: true, data: T }
{ success: true, data: T[], pagination: { page, limit, total, totalPages } }

// Error
{ success: false, error: { code: string, message: string } }
```

## Quality Checklist

- [ ] Zod validation on all inputs
- [ ] Authentication check where needed
- [ ] Proper HTTP status codes (200, 201, 400, 401, 404, 500)
- [ ] Database transactions for multi-step operations
- [ ] Error messages are user-friendly
- [ ] No sensitive data in responses (passwords, tokens)
- [ ] Rate limiting on public endpoints
- [ ] Tests cover happy path and error cases

## Example

**Input:** "Create a CRUD API for managing blog posts with title, content, status, and author"

**Output:** Complete route handlers with Zod validation, Prisma queries, auth checks, pagination, and 8 test cases.

## Tools

- **GitHub Copilot Chat** — Best for generating route handlers
- **Cursor** — Best for multi-file API scaffolding
- **ChatGPT** — Best for designing API contracts and schemas
