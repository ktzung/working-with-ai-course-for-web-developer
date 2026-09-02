# Project Context — WebDevHub

> Copy this file to your project root and fill in your details. AI tools use this context to generate better, more consistent code.

## Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Frontend | React / Next.js | 14.x |
| Language | TypeScript | 5.x |
| Styling | Tailwind CSS | 3.x |
| State Management | Zustand / React Context | — |
| Backend | Next.js API Routes / Express | — |
| Database | PostgreSQL | 16.x |
| ORM | Prisma | 5.x |
| Auth | NextAuth.js | 5.x |
| Deployment | Vercel | — |
| Testing | Jest + React Testing Library + Playwright | — |

## Architecture

```
┌─────────────┐     ┌──────────────┐     ┌────────────┐
│   Frontend   │────▶│  API Routes  │────▶│  Database   │
│  Next.js SSR │     │  /api/*      │     │  PostgreSQL │
└─────────────┘     └──────────────┘     └────────────┘
       │                    │
       ▼                    ▼
┌─────────────┐     ┌──────────────┐
│  Components  │     │   Auth       │
│  React/TSX   │     │  NextAuth.js │
└─────────────┘     └──────────────┘
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/projects` | List all projects |
| POST | `/api/projects` | Create new project |
| GET | `/api/projects/[id]` | Get project by ID |
| PUT | `/api/projects/[id]` | Update project |
| DELETE | `/api/projects/[id]` | Delete project |
| GET | `/api/tasks` | List tasks (filterable) |
| POST | `/api/tasks` | Create task |
| GET | `/api/users/me` | Current user profile |

## Database Schema (Key Models)

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  projects  Project[]
}

model Project {
  id        String   @id @default(cuid())
  title     String
  userId    String
  user      User     @relation(fields: [userId], references: [id])
  tasks     Task[]
  createdAt DateTime @default(now())
}

model Task {
  id        String   @id @default(cuid())
  title     String
  status    TaskStatus @default(TODO)
  projectId String
  project   Project  @relation(fields: [projectId], references: [id])
}
```

## Coding Standards

- **Components:** Functional components with TypeScript interfaces for props
- **Naming:** PascalCase for components, camelCase for functions/variables
- **Files:** One component per file, co-located tests
- **Imports:** Absolute imports via `@/` alias
- **Styling:** Tailwind utility classes, no inline styles
- **Error Handling:** Try-catch with user-friendly error messages
- **Commits:** Conventional commits (`feat:`, `fix:`, `docs:`)

## Deployment

- **Platform:** Vercel (auto-deploy from `main` branch)
- **Preview:** Every PR gets a preview URL
- **Environment Variables:** Managed via Vercel dashboard
- **Database:** Vercel Postgres or external PostgreSQL
- **Domain:** Custom domain via Vercel DNS

## Environment Variables

```env
DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
NEXTAUTH_URL=http://localhost:3000
GITHUB_ID=...
GITHUB_SECRET=...
```
