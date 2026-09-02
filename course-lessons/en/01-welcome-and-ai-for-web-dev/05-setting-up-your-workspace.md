# Setting Up Your Workspace

## Building Your AI-Powered Development Environment

A well-configured workspace makes AI tools dramatically more effective. In this lesson, we'll set up VS Code with the right extensions, settings, and workflows for AI-assisted web development.

## Step 1: Install VS Code Extensions

These extensions form the foundation of your AI-powered workspace:

### Essential AI Extensions

**GitHub Copilot** — Your primary AI coding assistant
- Install from the Extensions marketplace
- Sign in with your GitHub account (requires Copilot subscription)
- Enable inline suggestions in settings

**GitHub Copilot Chat** — Conversational AI in your editor
- Works alongside Copilot for longer discussions
- Use `@workspace` to reference your entire project
- Use `/explain`, `/fix`, `/test` commands

**Cursor** (Alternative) — If you prefer Cursor as your editor
- Built-in AI with codebase awareness
- No additional extensions needed

### Web Development Essentials

**ESLint** — Catches code quality issues
```json
// .eslintrc.json
{
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "plugins": ["react", "react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

**Prettier** — Consistent code formatting
```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

**Tailwind CSS IntelliSense** — Autocomplete for Tailwind classes
- Class autocomplete
- Linting for Tailwind classes
- Hover preview of CSS

**TypeScript Hero** — Auto-imports and TypeScript management

## Step 2: Configure VS Code Settings

Open your settings (`Cmd+,` on Mac, `Ctrl+,` on Windows) and configure:

```json
{
  // AI Settings
  "github.copilot.enable": true,
  "github.copilot.inlineSuggest.enable": true,
  "editor.inlineSuggest.enabled": true,
  
  // Editor Settings
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  
  // TypeScript Settings
  "typescript.updateImportsOnFileMove.enabled": "always",
  "typescript.suggest.autoImports": true,
  
  // File Associations
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

## Step 3: Set Up Your Project Structure

A consistent project structure helps AI tools understand your codebase:

```
my-web-project/
├── src/
│   ├── components/        # Reusable UI components
│   │   ├── ui/           # Basic UI elements
│   │   ├── layout/       # Layout components
│   │   └── features/     # Feature-specific components
│   ├── pages/            # Page components
│   ├── hooks/            # Custom React hooks
│   ├── utils/            # Utility functions
│   ├── types/            # TypeScript type definitions
│   ├── services/         # API calls and external services
│   ├── context/          # React context providers
│   └── styles/           # Global styles
├── public/               # Static assets
├── tests/                # Test files
├── docs/                 # Documentation
└── .github/              # GitHub workflows
```

## Step 4: Create AI Context Files

These files help AI tools understand your project better:

### `.github/copilot-instructions.md`
```markdown
# Project Context

## Tech Stack
- Framework: Next.js 14 with App Router
- Language: TypeScript
- Styling: Tailwind CSS
- State: Zustand
- API: tRPC
- Database: PostgreSQL with Prisma

## Conventions
- Use functional components with hooks
- Prefer named exports
- Use TypeScript strict mode
- Follow Airbnb style guide

## File Naming
- Components: PascalCase (UserProfile.tsx)
- Hooks: camelCase with 'use' prefix (useUserData.ts)
- Utils: camelCase (formatDate.ts)
```

### `CONTEXT.md` (for other AI tools)
Similar content formatted for ChatGPT, Claude, or Cursor.

## Step 5: Set Up Git Workflow

Configure Git for AI-assisted development:

```bash
# Useful aliases
git config --global alias.ai-commit '!git add -A && git commit -m "$(git diff --staged --stat)"'

# Pre-commit hooks for code quality
npm install --save-dev husky lint-staged
npx husky install
```

## Step 6: Terminal Integration

Set up your terminal for quick AI access:

```bash
# Install GitHub CLI
brew install gh

# Install AI CLI tools
npm install -g @anthropic-ai/claude-cli
```

## Quick Reference Card

Keep this handy as you start working:

| Action | Shortcut/Command |
|--------|------------------|
| Accept Copilot suggestion | `Tab` |
| Dismiss suggestion | `Esc` |
| Open Copilot Chat | `Cmd+Shift+I` |
| Explain code | Select code, then `/explain` |
| Fix code | Select code, then `/fix` |
| Generate tests | Select code, then `/test` |

## What's Next

Your workspace is ready! In the next lesson, we'll introduce the WebDevHub project — the main project you'll build throughout this course using all the AI techniques you'll learn.
