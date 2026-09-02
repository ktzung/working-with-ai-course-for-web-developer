# Setting Up Your Dev Workspace

## Putting It All Together

You've learned about project context, API specs, coding standards, and task files. Now let's set up a complete development workspace where all these context files work together seamlessly.

## The Complete Workspace Structure

```
webdevhub/
├── .github/
│   ├── copilot-instructions.md    # Context for GitHub Copilot
│   └── workflows/
│       └── ci.yml                 # CI/CD pipeline
├── .vscode/
│   ├── settings.json              # VS Code settings
│   ├── extensions.json            # Recommended extensions
│   └── launch.json                # Debug configuration
├── docs/
│   ├── context/
│   │   ├── project-context.md     # Main project context
│   │   ├── coding-standards.md    # Coding conventions
│   │   └── architecture.md        # Architecture decisions
│   ├── api/
│   │   └── openapi.yaml           # API specification
│   └── current-task.md            # Current task focus
├── src/
│   ├── app/                       # Next.js App Router
│   ├── components/                # React components
│   ├── hooks/                     # Custom hooks
│   ├── services/                  # API services
│   ├── types/                     # TypeScript types
│   └── utils/                     # Utility functions
├── prisma/
│   └── schema.prisma              # Database schema
├── tests/                         # Test files
├── .cursorrules                   # Cursor AI context
├── .eslintrc.json                 # ESLint config
├── .prettierrc                    # Prettier config
├── tailwind.config.ts             # Tailwind config
├── tsconfig.json                  # TypeScript config
└── package.json                   # Dependencies
```

## Setting Up Each Context File

### 1. GitHub Copilot Instructions

Create `.github/copilot-instructions.md`:

```markdown
# WebDevHub - Copilot Instructions

## Project Overview
WebDevHub is a portfolio and project management platform for web developers.

## Tech Stack
- Next.js 14 with App Router
- TypeScript (strict mode)
- Tailwind CSS
- Prisma + PostgreSQL
- NextAuth.js

## Conventions
- Functional components with hooks
- Named exports only
- PascalCase for components, camelCase for functions
- Mobile-first responsive design
- Error boundaries for component errors

## Current Focus
See docs/current-task.md for current sprint goals.
```

### 2. VS Code Settings

Create `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "typescript.updateImportsOnFileMove.enabled": "always",
  "typescript.suggest.autoImports": true,
  "github.copilot.enable": true,
  "github.copilot.inlineSuggest.enable": true,
  "editor.inlineSuggest.enabled": true
}
```

### 3. Recommended Extensions

Create `.vscode/extensions.json`:

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "bradlc.vscode-tailwindcss",
    "prisma.prisma",
    "ms-vscode.vscode-typescript-next",
    "github.copilot",
    "github.copilot-chat"
  ]
}
```

### 4. Cursor Rules

Create `.cursorrules`:

```
You are working on WebDevHub, a portfolio and project management platform.

Tech Stack:
- Next.js 14 with App Router
- TypeScript (strict mode)
- Tailwind CSS
- Prisma + PostgreSQL
- NextAuth.js

Rules:
1. Use functional components with hooks
2. Use named exports (no default exports)
3. Use PascalCase for components, camelCase for functions
4. Use TypeScript strict mode (no any types)
5. Use Tailwind CSS for styling
6. Use Zod for validation
7. Follow mobile-first responsive design
8. Include error handling in all API calls
9. Use Prisma for database queries
10. Follow the patterns in docs/context/coding-standards.md
```

## Git Workflow

### Branch Naming
```
feature/user-authentication
feature/project-kanban
fix/login-redirect
refactor/api-services
```

### Commit Messages
```
feat: add user authentication with NextAuth
fix: resolve login redirect loop
refactor: extract project service layer
docs: update API specification
```

### Pre-commit Hooks

Set up with Husky:

```bash
npm install --save-dev husky lint-staged
npx husky install
```

Add to `package.json`:
```json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
}
```

## Context File Workflow

### Starting a New Task
1. Update `docs/current-task.md` with new task
2. Open relevant files in VS Code
3. Reference context in your first AI prompt

### During Development
1. Keep `current-task.md` updated
2. Add new patterns to `coding-standards.md`
3. Update `project-context.md` with architecture changes

### Completing a Task
1. Mark all requirements as complete in `current-task.md`
2. Document any new patterns or decisions
3. Update API spec if endpoints changed

## Quick Reference

### Context File Locations
| File | Purpose | Location |
|------|---------|----------|
| Project Context | Overall project info | `docs/context/project-context.md` |
| Coding Standards | Code conventions | `docs/context/coding-standards.md` |
| API Spec | API contracts | `docs/api/openapi.yaml` |
| Current Task | Current focus | `docs/current-task.md` |
| Copilot Instructions | Copilot context | `.github/copilot-instructions.md` |
| Cursor Rules | Cursor context | `.cursorrules` |

### When to Update Each File
| File | Update When |
|------|-------------|
| project-context.md | Architecture changes, new tech |
| coding-standards.md | New patterns, convention changes |
| openapi.yaml | API changes |
| current-task.md | Starting new task |
| copilot-instructions.md | Major project changes |

## Practice Exercise

1. Set up the complete workspace structure for WebDevHub
2. Create all context files
3. Make a commit using the Git workflow
4. Start a new task using the context system

## What's Next

Congratulations! You've completed the context engineering section. In Section 4, we'll apply everything you've learned to frontend development — building real components and features with AI assistance.
