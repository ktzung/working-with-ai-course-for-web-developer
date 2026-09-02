# What Is Project Context?

## Why AI Needs Full Project Context

Imagine asking a new developer to join your team and start coding immediately — without showing them the codebase, the architecture, or the coding standards. They'd produce code that doesn't fit. The same happens with AI when it lacks context.

## The Context Problem

AI tools generate code based on patterns they've learned. Without your project's context, they'll:

- Use generic patterns instead of your team's conventions
- Suggest technologies you're not using
- Create code that doesn't integrate with your existing architecture
- Miss important constraints and requirements

## What Is Project Context?

Project context is the information AI needs to generate relevant, consistent code for your specific project. It includes:

### 1. Technical Context
- **Tech stack**: Framework, language, libraries, versions
- **Architecture**: How your app is structured (monolith, microservices, etc.)
- **Database**: Schema, ORM, relationships
- **API design**: REST, GraphQL, tRPC patterns

### 2. Codebase Context
- **File structure**: Where different types of files live
- **Naming conventions**: How files, functions, and variables are named
- **Code patterns**: Common patterns used throughout the codebase
- **Existing components**: What's already built

### 3. Team Context
- **Coding standards**: Linting rules, formatting preferences
- **Review requirements**: What reviewers look for
- **Testing standards**: Coverage expectations, testing patterns
- **Documentation standards**: How code is documented

### 4. Project Context
- **Current goals**: What you're building right now
- **Constraints**: Performance, accessibility, browser support
- **Deadlines**: How much complexity is acceptable
- **User requirements**: Who uses the app and how

## The Impact of Good Context

Without context:
```
Prompt: "Create a user profile component"
Result: Generic component with basic fields
```

With context:
```
Prompt: "Create a user profile component"
Context: [project-context.md shows Next.js 14, TypeScript, Tailwind, 
          existing User type, specific design system, accessibility requirements]
Result: Component that matches your exact architecture, uses your design tokens,
        follows your naming conventions, and integrates with existing code
```

## Context Sources

### Automatic Context
AI tools can automatically gather:
- Open files in your editor
- File structure of your project
- Package.json dependencies
- TypeScript types and interfaces

### Manual Context
You need to provide:
- Architecture decisions
- Coding standards
- Current task description
- Business requirements

### Documentation Context
Written documents that describe:
- Project overview and goals
- Technical architecture
- API specifications
- Design system

## Building Your Context Layer

Think of context as layers:

```
Layer 1: Project Identity
├── What is this project?
├── What tech stack does it use?
└── What are the goals?

Layer 2: Architecture
├── How is the code organized?
├── What patterns are used?
└── How do components interact?

Layer 3: Standards
├── Coding conventions
├── Testing requirements
└── Documentation expectations

Layer 4: Current Task
├── What am I building right now?
├── What are the specific requirements?
└── What constraints apply?
```

## Context for Different AI Tools

### GitHub Copilot
- Reads open files automatically
- Uses `.github/copilot-instructions.md` for project-level context
- Comments in code provide local context

### ChatGPT/Claude
- You provide context in the conversation
- Upload context files at the start of a session
- Reference specific files when asking questions

### Cursor
- Indexes your entire codebase
- Uses `.cursorrules` for project conventions
- Can reference any file in your project

## The Context Engineering Mindset

Context engineering is about being intentional with the information you provide to AI. Instead of hoping AI figures out your project, you:

1. **Document** your project's key decisions
2. **Structure** information so AI can use it
3. **Update** context as your project evolves
4. **Share** context across your team

## What's Next

In the next lesson, we'll create the main context file for your project — `project-context.md` — that serves as the foundation for all AI interactions.
