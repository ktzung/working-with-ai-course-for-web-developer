# Skill: Component Generation

> Generate production-ready React/Vue components from natural language descriptions.

## Purpose

This skill enables AI tools to generate complete, well-structured UI components that follow project conventions, include TypeScript types, proper styling, accessibility features, and co-located tests.

## When to Use

- Building new UI features
- Creating reusable component libraries
- Prototyping page layouts
- Generating form components with validation

## Instructions

### Step 1: Gather Requirements

Before generating a component, clarify:
1. **Component name** — PascalCase, descriptive
2. **Props** — What data does it receive? What callbacks does it expose?
3. **Visual design** — Layout, colors, responsive behavior
4. **Interactivity** — Click handlers, form inputs, animations
5. **States** — Loading, error, empty, success

### Step 2: Generate Component

Use this prompt structure:

```
Create a React component called {name} with TypeScript.

Props interface:
{props}

Requirements:
- {functional requirements}
- Tailwind CSS for styling
- Responsive: mobile-first approach
- Accessible: semantic HTML, ARIA labels, keyboard navigation
- Error and loading states included

File path: src/components/{name}.tsx
```

### Step 3: Generate Tests

```
Write tests for the {name} component using React Testing Library and Jest.

Test cases:
1. Renders with required props
2. Handles user interactions correctly
3. Shows loading state when isLoading=true
4. Displays error state with message
5. Renders empty state appropriately
6. Meets accessibility standards

File path: src/components/__tests__/{name}.test.tsx
```

### Step 4: Generate Storybook Story (Optional)

```
Create a Storybook story for {name} component.

Include stories for:
- Default state
- Loading state
- Error state
- Empty state
- All variant props
- Responsive preview
```

## Output Structure

Each generated component should include:

```
src/components/{Name}/
├── {Name}.tsx          # Component implementation
├── {Name}.types.ts     # TypeScript interfaces
├── {Name}.test.tsx     # Unit tests
├── {Name}.stories.tsx  # Storybook stories (optional)
└── index.ts            # Barrel export
```

## Quality Checklist

- [ ] TypeScript strict mode compatible
- [ ] No `any` types
- [ ] Props have JSDoc comments
- [ ] Tailwind classes follow project design tokens
- [ ] Responsive at all breakpoints
- [ ] Keyboard navigable
- [ ] Screen reader friendly
- [ ] Tests cover critical paths
- [ ] Component is memoized if expensive to render

## Example

**Input:** "Create a user card component that shows avatar, name, email, and a follow button"

**Output:** A complete `UserCard` component with props interface, Tailwind styling, hover states, follow button with loading state, and 6 test cases.

## Tools

- **GitHub Copilot Chat** — Best for inline component generation
- **Cursor** — Best for multi-file component scaffolding
- **ChatGPT** — Best for architecture decisions and complex logic
