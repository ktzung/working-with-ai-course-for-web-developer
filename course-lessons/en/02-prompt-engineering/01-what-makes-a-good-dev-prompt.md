# What Makes a Good Developer Prompt

## The Anatomy of an Effective Prompt

Not all prompts are created equal. A vague prompt like "make a button component" will get you generic code. A well-structured prompt will get you exactly what you need. Let's break down the anatomy of a great developer prompt.

## The Four Elements

Every effective developer prompt contains four elements:

### 1. Context
Tell the AI about your project, tech stack, and existing code.

### 2. Task
Clearly describe what you want to build or accomplish.

### 3. Tech Stack
Specify the exact technologies, versions, and patterns you're using.

### 4. Constraints
Define limitations, requirements, and quality standards.

## The Formula

```
[Context] + [Task] + [Tech Stack] + [Constraints] = Great Code
```

## Examples: Bad vs. Good

### Example 1: Creating a Button

**Bad prompt:**
```
Make a button component
```

**Good prompt:**
```
I'm building a Next.js 14 app with TypeScript and Tailwind CSS.

Create a reusable Button component with the following requirements:
- Variants: primary, secondary, outline, ghost
- Sizes: sm, md, lg
- Support for loading state with spinner
- Support for icons (left and right)
- Disabled state
- Use Tailwind CSS for styling
- Export as named export from components/ui/Button.tsx
- Include TypeScript interface for props
```

### Example 2: API Route

**Bad prompt:**
```
Create an API endpoint
```

**Good prompt:**
```
I'm using Next.js 14 App Router with Prisma and PostgreSQL.

Create an API route at /api/projects that:
- GET: Returns paginated list of projects (10 per page) with search by name
- POST: Creates a new project with validation (name required, description optional, techStack as string array)
- Include proper error handling with appropriate HTTP status codes
- Use Zod for request validation
- Include TypeScript types for request/response
- Add rate limiting (100 requests per minute per user)
```

### Example 3: Custom Hook

**Bad prompt:**
```
Make a hook for fetching data
```

**Good prompt:**
```
I'm using React 18 with TypeScript and Zustand for state management.

Create a custom hook called useProjects that:
- Fetches projects from /api/projects
- Supports pagination (page, limit params)
- Includes loading, error, and success states
- Caches results in Zustand store
- Supports refetching and invalidation
- Handles network errors gracefully
- Returns typed data: { projects: Project[], total: number, page: number }
- Include abort controller for cleanup
```

## Prompt Patterns for Web Development

### The Component Pattern
```
Create a [ComponentName] component for [framework] with:
- Props: [list props with types]
- Features: [list features]
- Styling: [approach]
- File: [location]
```

### The API Pattern
```
Create an API route at [path] using [framework]:
- [Method]: [description]
- Validation: [library]
- Error handling: [approach]
- Auth: [requirement]
```

### The Hook Pattern
```
Create a custom hook called [hookName] that:
- [Primary function]
- State management: [approach]
- Error handling: [approach]
- Returns: [type definition]
```

### The Fix Pattern
```
I'm getting this error in [file]:
[paste error]

Here's the relevant code:
[paste code]

Tech stack: [stack]
What I've tried: [attempts]
```

## Advanced Prompt Techniques

### Specify What NOT to Do
```
Create a form component that:
- Uses React Hook Form (NOT Formik)
- Validates with Zod (NOT Yup)
- Uses controlled components (NOT uncontrolled)
- Does NOT use any external UI library
```

### Reference Existing Code
```
Following the same pattern as my existing UserProfile component 
(in src/components/UserProfile.tsx), create a ProjectCard component 
that displays project information with the same styling approach.
```

### Include Edge Cases
```
Create a date formatter that:
- Handles null/undefined dates
- Supports multiple formats (ISO, US, EU)
- Handles timezone conversion
- Returns "Invalid Date" for malformed input
- Works with both Date objects and date strings
```

## Practice Exercise

Take this vague prompt and make it specific:

**Vague**: "Make a search feature"

**Your task**: Rewrite it using the four elements (Context, Task, Tech Stack, Constraints). Consider:
- What framework are you using?
- What should search? (projects, users, snippets?)
- How should results display?
- What about loading states?
- Debouncing? Keyboard shortcuts?

## What's Next

In the next lesson, we'll dive into specific prompt patterns for generating React and Vue components — the building blocks of modern web applications.
