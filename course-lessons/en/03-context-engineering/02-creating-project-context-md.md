# Creating project-context.md

## Your Project's AI Bible

The `project-context.md` file is the single most important document for AI-assisted development. It tells AI everything it needs to know about your project. Let's create one for WebDevHub.

## The Structure

A good `project-context.md` has these sections:

```markdown
# Project Context: WebDevHub

## Overview
WebDevHub is a portfolio and project management platform for web developers.
It allows developers to showcase projects, manage tasks, and share code snippets.

## Tech Stack

### Frontend
- **Framework**: Next.js 14.2.x with App Router
- **Language**: TypeScript 5.x (strict mode)
- **Styling**: Tailwind CSS 3.4.x
- **State Management**: Zustand 4.x
- **Forms**: React Hook Form 7.x + Zod 3.x
- **Icons**: Lucide React
- **Charts**: Recharts

### Backend
- **API**: Next.js API Routes (App Router)
- **Database**: PostgreSQL 16
- **ORM**: Prisma 5.x
- **Auth**: NextAuth.js 4.x (GitHub, Google providers)
- **Validation**: Zod 3.x

### DevOps
- **Hosting**: Vercel
- **CI/CD**: GitHub Actions
- **Containerization**: Docker (local dev)

## Architecture

### App Router Structure
```
src/app/
├── (auth)/          # Auth pages (login, register)
├── (dashboard)/     # Protected dashboard pages
├── api/             # API routes
├── layout.tsx       # Root layout
└── page.tsx         # Landing page
```

### Component Organization
```
src/components/
├── ui/              # Base UI components (Button, Card, Input)
├── layout/          # Layout components (Header, Sidebar, Footer)
├── features/        # Feature-specific components
└── shared/          # Shared utility components
```

### Data Flow
1. Components call hooks (useProjects, useTasks)
2. Hooks call service functions (projectService.getProjects)
3. Services make API requests to /api/*
4. API routes validate with Zod, query with Prisma
5. Responses follow standard format: { data, error, pagination }

## Coding Conventions

### File Naming
- Components: PascalCase (`ProjectCard.tsx`)
- Hooks: camelCase with `use` prefix (`useProjects.ts`)
- Utils: camelCase (`formatDate.ts`)
- Types: PascalCase (`Project.ts`)
- API routes: kebab-case (`route.ts`)

### Component Patterns
- Functional components only (no class components)
- Named exports (no default exports)
- Props interface named `{ComponentName}Props`
- Hooks at top of component
- JSX return at bottom

### TypeScript
- Strict mode enabled
- No `any` types (use `unknown` if needed)
- Interfaces for object shapes, types for unions
- Explicit return types for functions

### Styling
- Tailwind CSS utility classes
- Mobile-first responsive design
- Dark mode support via `dark:` prefix
- Custom colors defined in tailwind.config.ts

## Database Schema

### Core Models
- **User**: id, name, email, avatar, role, createdAt
- **Project**: id, name, description, techStack[], status, ownerId
- **Task**: id, title, description, status, priority, projectId, assigneeId
- **Snippet**: id, title, code, language, tags[], userId

### Relationships
- User has many Projects (1:N)
- Project has many Tasks (1:N)
- User has many Snippets (1:N)
- Project has many Members (M:N via ProjectMember)

## API Conventions

### Response Format
```typescript
// Success
{ data: T }
{ data: T[], pagination: { page, limit, total, totalPages } }

// Error
{ error: { code: string, message: string, details?: any } }
```

### HTTP Methods
- GET: Read (list or single)
- POST: Create
- PUT: Full update
- PATCH: Partial update
- DELETE: Delete (soft delete preferred)

### Status Codes
- 200: Success
- 201: Created
- 204: Deleted
- 400: Validation error
- 401: Unauthorized
- 403: Forbidden
- 404: Not found
- 500: Server error

## Testing Standards

- Unit tests for utility functions
- Component tests with React Testing Library
- API tests with supertest
- Minimum 80% code coverage
- Test file naming: `{name}.test.ts` or `{name}.spec.ts`

## Current Sprint Goals

- [ ] Complete user authentication flow
- [ ] Build project CRUD operations
- [ ] Implement Kanban board
- [ ] Add code snippet manager

## Known Issues

- Dark mode toggle needs persistence
- Mobile sidebar animation is janky
- Search is case-sensitive (should be insensitive)
```

## How to Use This File

### With GitHub Copilot
Save as `.github/copilot-instructions.md` in your project root.

### With ChatGPT/Claude
Upload at the start of each conversation or paste relevant sections.

### With Cursor
Save as `.cursorrules` or reference in your prompts.

## Keeping It Updated

Update `project-context.md` when:
- Adding new technologies
- Changing architecture decisions
- Updating coding conventions
- Completing sprint goals
- Discovering new issues

## Practice Exercise

Create a `project-context.md` for your own project. Include:
1. Tech stack overview
2. Architecture description
3. Coding conventions
4. Current goals

## What's Next

In the next lesson, we'll learn how to use API specifications as context — making AI-generated code match your exact API contracts.
