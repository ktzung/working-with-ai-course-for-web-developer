# What Are Web Development Skills?

## Learning Objectives
- Understand what AI skills are in the context of web development
- Learn how skills differ from prompts and agents
- Identify common web development workflows that become skills

## Skills = Reusable Workflows

In AI-assisted development, a **skill** is a reusable workflow that you can invoke repeatedly to perform a specific task. Think of it as a saved recipe — once you figure out the perfect way to generate a component, scaffold an API, or write tests, you save that workflow as a skill.

**Without skills**: You write the same prompt every time you need a component
**With skills**: You invoke a skill that knows exactly what you need

## Skills vs Prompts vs Agents

| Concept | What It Is | Example |
|---------|-----------|---------|
| **Prompt** | A one-time instruction | "Create a login form with email and password" |
| **Skill** | A reusable workflow with instructions | Component generation skill that always follows your patterns |
| **Agent** | An autonomous assistant that uses skills | Security scanner that runs multiple checks |

**Prompts** are like asking a question. **Skills** are like having a recipe. **Agents** are like hiring a specialist.

## Anatomy of a Skill

A skill typically consists of:

1. **Trigger**: When to use this skill
2. **Instructions**: Step-by-step workflow
3. **Templates**: Code patterns to follow
4. **Constraints**: Rules and limitations
5. **Output format**: What the skill produces

```markdown
# Component Generation Skill

## Trigger
When user asks to create a new React component

## Instructions
1. Ask for component name and props
2. Check existing components for patterns
3. Generate component with TypeScript
4. Include tests and Storybook story
5. Follow project's style guide

## Templates
- Functional component with hooks
- Props interface
- Test file with React Testing Library
- Storybook story

## Constraints
- Use TypeScript
- Follow naming conventions
- Include accessibility attributes
- Maximum 200 lines per component
```

## Common Web Dev Skills

### 1. Component Generation
Generate React/Vue/Angular components following project conventions.

### 2. API Scaffolding
Create Express/FastAPI routes with controllers, models, and validation.

### 3. Test Generation
Write unit tests, integration tests, and E2E tests for existing code.

### 4. Code Refactoring
Improve code quality while maintaining functionality.

### 5. Documentation Generation
Create API docs, README files, and inline comments.

### 6. Database Migration
Generate migration scripts for schema changes.

## Why Skills Matter

1. **Consistency**: Every component follows the same patterns
2. **Speed**: Generate boilerplate in seconds, not minutes
3. **Quality**: Skills encode best practices
4. **Knowledge sharing**: Team skills capture institutional knowledge

## Creating Your First Skill

Here's a simple skill for generating Express.js routes:

```markdown
# Express Route Generator

## Trigger
When user needs a new API endpoint

## Workflow
1. Ask for resource name (e.g., "products")
2. Ask for operations needed (CRUD or subset)
3. Generate route file with:
   - Route definitions
   - Controller imports
   - Middleware (auth, validation)
   - Error handling
4. Generate controller file with:
   - Async handlers
   - Database operations
   - Response formatting
5. Generate validation schema with Zod

## Output
- routes/[resource].js
- controllers/[resource]Controller.js
- validators/[resource]Validator.js
```

## AI Prompt for Skill Creation

```
Create a reusable skill for generating React components that:
1. Follows the project's existing component patterns
2. Includes TypeScript props interface
3. Generates test file with React Testing Library
4. Creates Storybook story
5. Adds proper accessibility attributes
6. Follows naming conventions

The skill should be documented as a markdown file with clear trigger, instructions, and templates.
```

## Practice Exercise

Create three skills for your development workflow:
1. **Component Generator**: Generate React components with tests
2. **API Endpoint Generator**: Create Express routes with controllers
3. **Page Generator**: Create Next.js pages with layouts

Document each skill with trigger, instructions, and templates.

## Key Takeaways

- Skills are reusable workflows that encode development patterns
- They differ from one-time prompts and autonomous agents
- Skills improve consistency, speed, and quality
- Start by identifying repetitive tasks in your workflow
