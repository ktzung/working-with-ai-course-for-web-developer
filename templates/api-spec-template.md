# API Specification — [Endpoint Name]

> Use this template to document API endpoints before implementation. Share with AI to generate boilerplate code.

## Endpoint

| Field | Value |
|-------|-------|
| **Method** | `GET` / `POST` / `PUT` / `DELETE` |
| **Path** | `/api/[resource]` |
| **Auth** | Required / Public |
| **Rate Limit** | 100 req/min |

## Description

[What this endpoint does and when to use it]

## Request

### Headers

```
Content-Type: application/json
Authorization: Bearer <token>
```

### Path Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Resource ID |

### Query Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `page` | number | No | 1 | Page number |
| `limit` | number | No | 20 | Items per page |
| `search` | string | No | — | Search term |

### Request Body

```json
{
  "title": "string (required)",
  "description": "string (optional)",
  "status": "enum: TODO | IN_PROGRESS | DONE"
}
```

## Response

### Success — `200 OK`

```json
{
  "success": true,
  "data": {
    "id": "cuid",
    "title": "My Project",
    "createdAt": "2026-01-15T10:30:00Z"
  }
}
```

### Success (List) — `200 OK`

```json
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

### Error — `400 Bad Request`

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Title is required"
  }
}
```

### Error — `401 Unauthorized`

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or expired token"
  }
}
```

## Validation Rules

- `title`: 1–200 characters, required
- `description`: max 2000 characters, optional
- `status`: must be one of the enum values

## AI Prompt for Implementation

```
Create a Next.js API route at /api/[resource] with:
- Method: [GET/POST/PUT/DELETE]
- Prisma ORM for database operations
- Zod schema validation
- Proper error handling with status codes
- Authentication check via NextAuth session
- Pagination support for list endpoints

Schema: [paste relevant Prisma model]
```

## Testing

```typescript
// Example test
describe('POST /api/projects', () => {
  it('creates a project with valid data', async () => {
    const res = await request(app)
      .post('/api/projects')
      .send({ title: 'Test Project' })
      .set('Authorization', `Bearer ${token}`);
    
    expect(res.status).toBe(201);
    expect(res.body.data.title).toBe('Test Project');
  });
});
```
