# WebDevHub — Project Specification

> The capstone project for the Working With AI Course for Web Developers.

## Overview

**WebDevHub** is a full-stack web application that serves as a personal developer dashboard for managing web development projects, tracking tasks, and collaborating with team members. Built entirely with AI-assisted development.

## Features

### Core Features

1. **Project Management**
   - Create, edit, and delete projects
   - Track project status (Planning, Active, On Hold, Completed)
   - Assign tech stack tags to projects
   - Project timeline with milestones

2. **Task Board**
   - Kanban-style task board (To Do, In Progress, Review, Done)
   - Drag-and-drop task management
   - Task priorities (Low, Medium, High, Critical)
   - Due dates and reminders
   - Task assignment to team members

3. **User Dashboard**
   - Overview of all projects and tasks
   - Activity feed showing recent changes
   - Quick actions (create project, add task)
   - Statistics and charts (tasks completed, project progress)

4. **Authentication**
   - Email/password login
   - GitHub OAuth integration
   - Protected routes and API endpoints
   - User profile management

5. **Real-time Updates**
   - Live task status changes
   - Notifications for assignments and mentions
   - Collaborative editing indicators

### Nice-to-Have Features

- Dark mode toggle
- Keyboard shortcuts
- Export projects to JSON/CSV
- API key generation for external integrations
- Markdown support in task descriptions

## Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Framework | Next.js 14 (App Router) | SSR, API routes, file-based routing |
| Language | TypeScript | Type safety, better AI suggestions |
| Styling | Tailwind CSS | Rapid UI development, consistent design |
| Database | PostgreSQL | Relational data, strong consistency |
| ORM | Prisma | Type-safe queries, migrations |
| Auth | NextAuth.js v5 | Multi-provider auth, session management |
| State | Zustand | Lightweight, TypeScript-friendly |
| Testing | Jest + RTL + Playwright | Unit, integration, E2E coverage |
| Deployment | Vercel | Zero-config, preview deploys |
| Monitoring | Vercel Analytics | Core Web Vitals tracking |

## Database Schema

```prisma
model User {
  id        String    @id @default(cuid())
  email     String    @unique
  name      String?
  avatar    String?
  projects  Project[]
  tasks     Task[]    @relation("AssignedTasks")
  createdAt DateTime  @default(now())
}

model Project {
  id          String      @id @default(cuid())
  title       String
  description String?
  status      ProjectStatus @default(PLANNING)
  tags        String[]
  userId      String
  user        User        @relation(fields: [userId], references: [id])
  tasks       Task[]
  createdAt   DateTime    @default(now())
  updatedAt   DateTime    @updatedAt
}

model Task {
  id          String     @id @default(cuid())
  title       String
  description String?
  status      TaskStatus @default(TODO)
  priority    Priority   @default(MEDIUM)
  dueDate     DateTime?
  projectId   String
  project     Project    @relation(fields: [projectId], references: [id])
  assigneeId  String?
  assignee    User?      @relation("AssignedTasks", fields: [assigneeId], references: [id])
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
}

enum ProjectStatus { PLANNING ACTIVE ON_HOLD COMPLETED }
enum TaskStatus { TODO IN_PROGRESS REVIEW DONE }
enum Priority { LOW MEDIUM HIGH CRITICAL }
```

## Milestones

| Phase | Duration | Deliverables |
|-------|----------|-------------|
| **Phase 1: Setup** | Week 1 | Project scaffolding, DB setup, auth |
| **Phase 2: Core** | Week 2–3 | Project CRUD, task board, dashboard |
| **Phase 3: Polish** | Week 4 | Styling, responsive design, dark mode |
| **Phase 4: Testing** | Week 5 | Unit tests, E2E tests, security audit |
| **Phase 5: Deploy** | Week 6 | Vercel deployment, monitoring, docs |

## Success Criteria

- [ ] All core features implemented and working
- [ ] Lighthouse score > 90 on all categories
- [ ] Test coverage > 80%
- [ ] Zero critical security vulnerabilities
- [ ] Responsive on mobile, tablet, and desktop
- [ ] Accessible (WCAG AA compliance)
- [ ] Deployed and accessible via public URL
