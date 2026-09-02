# Prompts for Debugging

## Debugging with AI

Bugs are inevitable. What matters is how quickly you find and fix them. AI tools are exceptionally good at debugging — if you know how to prompt them effectively.

## The Debugging Prompt Formula

A good debugging prompt includes:

1. **The error message** — Copy the exact error
2. **The code** — Show the relevant code
3. **The context** — What you expected vs. what happened
4. **What you tried** — Previous attempts to fix it

## TypeScript Error Debugging

### Type Mismatch

```
I'm getting this TypeScript error:

Type 'string | undefined' is not assignable to type 'string'.
  Type 'undefined' is not assignable to type 'string'.

In this code:
const user: User = {
  name: data.name,
  email: data.email,
  avatar: data.avatar, // Error here
};

The data comes from an API response where avatar is optional.
How should I handle this properly?
```

### Generic Type Issues

```
TypeScript error in my React component:

No overload matches this call.
  Overload 1 of 2, '(props: ProjectCardProps): ProjectCard', gave the following error.
    Type '{ projects: Project[]; onSelect: (id: string) => void; }' is not assignable to type 'IntrinsicAttributes & ProjectCardProps'.

Here's my component:
[paste component code]

And how I'm using it:
[paste usage code]

I think the issue is with the props type. Can you help?
```

## React Error Debugging

### Hydration Errors

```
I'm getting a hydration mismatch error in Next.js:

Warning: Text content did not match. Server: "Loading..." Client: "5 projects"

This happens in my ProjectCount component:
[paste component code]

The component fetches data client-side, but the server renders different content.
How do I fix this properly in Next.js 14 with App Router?
```

### Infinite Re-renders

```
My React component is stuck in an infinite re-render loop.

Console shows: "Maximum update depth exceeded"

Here's my component:
[paste component code]

The useEffect seems to be triggering constantly. I think it's because:
- I'm using an object as a dependency
- The fetch function creates a new reference each render

What's the proper fix? Should I use useCallback, useMemo, or restructure the effect?
```

### State Update Issues

```
My React state isn't updating correctly.

When I click "Add Task", the task appears briefly then disappears.
I think it's a stale closure issue but I'm not sure.

Here's my code:
[paste component code]

The addTask function is called from an onClick handler.
The tasks state seems to reset after the API call.
```

## CSS/Styling Debugging

### Tailwind Not Working

```
My Tailwind CSS classes aren't being applied.

I have a button with these classes:
className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"

But the button appears unstyled. My tailwind.config.js:
[paste config]

And my CSS file:
[paste CSS]

What could be wrong?
```

### Layout Issues

```
My flexbox layout is breaking on mobile.

I have a sidebar + main content layout:
[paste HTML/JSX]

On desktop it looks correct, but on mobile:
- The sidebar overlaps the content
- The content doesn't scroll properly

I'm using Tailwind CSS. How should I fix the responsive behavior?
```

## API/Network Debugging

### CORS Errors

```
I'm getting a CORS error when calling my API from the frontend:

Access to fetch at 'http://localhost:3001/api/projects' from origin 
'http://localhost:3000' has been blocked by CORS policy.

My API is a Next.js app running on port 3001.
My frontend is a separate React app on port 3000.

How do I configure CORS properly?
```

### Fetch Errors

```
My fetch request is failing with a 400 error, but the API works in Postman.

Here's my fetch code:
[paste code]

The request body in Postman:
[paste body]

The error response:
[paste error]

I think the issue might be with how I'm sending the body. Can you help?
```

## Database Debugging

### Prisma Errors

```
Prisma is throwing this error:

Invalid `prisma.project.findMany()` invocation:
Unknown arg `include` in include.tasks for type Project

My Prisma schema:
[paste schema]

My query:
[paste query]

I thought I set up the relation correctly. What's wrong?
```

### Migration Issues

```
I'm getting this error when running prisma migrate:

Error: P3006
Migration `20240101_add_tasks` failed to apply cleanly to the shadow database.

My migration file:
[paste migration]

I have existing data in the database. How do I handle this safely?
```

## Performance Debugging

### Slow Renders

```
My React component is rendering slowly. The profiler shows it re-renders 
on every keystroke in the search input.

Here's my component structure:
[paste code]

The SearchInput is in a parent component, and the results list is in a child.
I think the parent is re-rendering too often.

How should I optimize this? React.memo, useMemo, or restructuring?
```

### Memory Leaks

```
My app's memory usage keeps growing. After navigating between pages 
for a few minutes, it becomes very slow.

I'm using useEffect for data fetching:
[paste code]

I think I'm not cleaning up properly. What should I add?
```

## Debugging Best Practices

### Provide Enough Context
```
// ❌ Too vague
"Help, my code doesn't work!"

// ✅ Good context
"I'm building a Next.js 14 app with TypeScript. My ProjectCard component 
throws a 'Cannot read property map of undefined' error when the project 
has no tasks. Here's the relevant code..."
```

### Include Error Stack Traces
```
Error: Cannot read properties of undefined (reading 'map')
    at ProjectCard (src/components/ProjectCard.tsx:15:23)
    at renderWithHooks (react-dom.development.js:14985:18)
    at mountIndeterminateComponent (react-dom.development.js:17811:13)
```

### Show What You've Tried
```
I've tried:
1. Adding optional chaining (project.tasks?.map) — still errors
2. Adding a default value (project.tasks || []) — works but feels hacky
3. Adding a loading check — doesn't help, tasks is undefined not loading

What's the proper way to handle this?
```

## Practice Exercise

Debug these WebDevHub issues:

1. **Type Error**: A component expects `Project[]` but receives `Project | undefined`
2. **Re-render Loop**: The Kanban board re-renders when dragging cards
3. **API Error**: The snippet creation endpoint returns 500

Write detailed debugging prompts for each, then fix with your AI tool.

## What's Next

In the next lesson, we'll learn how to prompt AI for styling — creating beautiful, responsive designs with Tailwind CSS and modern CSS techniques.
