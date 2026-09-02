# The Main Project: WebDevHub

## Your Portfolio-Ready Project

Throughout this course, you'll build **WebDevHub** — a portfolio and project management platform designed specifically for web developers. This isn't a toy project; it's a real application with production-grade features that you can customize and deploy.

## What Is WebDevHub?

WebDevHub is a full-stack web application that lets developers:

- **Showcase their work** — A beautiful portfolio page with project cards, tech stack badges, and live demos
- **Track projects** — A Kanban-style project board for managing tasks and deadlines
- **Manage snippets** — A code snippet library with syntax highlighting and tagging
- **Collaborate** — Share projects with teammates and get feedback
- **Deploy easily** — Built-in deployment configuration for Vercel, Netlify, or custom servers

## The Tech Stack

We've chosen a modern, production-ready stack:

### Frontend
- **Next.js 14** — React framework with App Router, Server Components, and API routes
- **TypeScript** — Type safety across the entire application
- **Tailwind CSS** — Utility-first styling with custom design tokens
- **Zustand** — Lightweight state management
- **React Hook Form** — Form handling with validation

### Backend
- **Next.js API Routes** — Serverless API endpoints
- **Prisma** — Type-safe database ORM
- **PostgreSQL** — Relational database
- **NextAuth.js** — Authentication with multiple providers

### DevOps
- **GitHub Actions** — CI/CD pipeline
- **Vercel** — Deployment platform
- **Docker** — Containerization for local development

## Project Structure

```
webdevhub/
├── src/
│   ├── app/                    # Next.js App Router pages
│   │   ├── (auth)/            # Auth group (login, register)
│   │   ├── (dashboard)/       # Dashboard group
│   │   ├── api/               # API routes
│   │   ├── layout.tsx         # Root layout
│   │   └── page.tsx           # Home page
│   ├── components/
│   │   ├── ui/                # Base UI components
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Input.tsx
│   │   │   └── Modal.tsx
│   │   ├── layout/            # Layout components
│   │   │   ├── Header.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── Footer.tsx
│   │   ├── portfolio/         # Portfolio features
│   │   │   ├── ProjectCard.tsx
│   │   │   ├── TechBadge.tsx
│   │   │   └── ProjectGrid.tsx
│   │   ├── dashboard/         # Dashboard features
│   │   │   ├── KanbanBoard.tsx
│   │   │   ├── TaskCard.tsx
│   │   │   └── StatsWidget.tsx
│   │   └── snippets/          # Code snippets
│   │       ├── SnippetCard.tsx
│   │       ├── CodeBlock.tsx
│   │   └── SnippetEditor.tsx
│   ├── hooks/                 # Custom hooks
│   ├── lib/                   # Utility libraries
│   ├── services/              # API service functions
│   ├── types/                 # TypeScript types
│   └── styles/                # Global styles
├── prisma/
│   └── schema.prisma          # Database schema
├── public/                    # Static assets
├── tests/                     # Test files
├── .github/                   # GitHub workflows
├── docker-compose.yml         # Docker configuration
└── package.json
```

## Features You'll Build

### Phase 1: Foundation (Sections 1-4)
- Project setup and configuration
- Component library with AI assistance
- Responsive layouts
- State management patterns

### Phase 2: Core Features (Sections 5-8)
- Portfolio showcase with project cards
- Kanban project board
- Code snippet manager
- User authentication

### Phase 3: Advanced Features (Sections 9-12)
- API design and implementation
- Database integration
- Real-time updates
- Deployment and optimization

## How AI Helps at Each Stage

| Feature | AI Assistance |
|---------|---------------|
| Component Library | Generate base components, suggest patterns |
| Portfolio Page | Create responsive layouts, optimize images |
| Kanban Board | Implement drag-and-drop, state management |
| Snippet Manager | Syntax highlighting, search functionality |
| Authentication | Set up NextAuth, protected routes |
| API Routes | Design endpoints, validation, error handling |
| Database | Prisma schema, migrations, queries |
| Deployment | Docker config, CI/CD pipeline |

## Your First Task

Before the next lesson, set up your development environment:

1. **Create a new Next.js project**:
   ```bash
   npx create-next-app@latest webdevhub --typescript --tailwind --eslint --app --src-dir
   ```

2. **Install core dependencies**:
   ```bash
   cd webdevhub
   npm install zustand react-hook-form @hookform/resolvers zod
   npm install prisma @prisma/client next-auth
   ```

3. **Initialize Prisma**:
   ```bash
   npx prisma init
   ```

4. **Open in VS Code**:
   ```bash
   code .
   ```

## What's Next

Starting with Section 2, we'll dive into prompt engineering — learning how to communicate with AI tools to generate exactly the code you need. You'll practice with real WebDevHub components and features.

The project is your playground. Every technique you learn will be applied directly to building WebDevHub. By the end of the course, you'll have a complete, deployable application and the skills to build many more.
