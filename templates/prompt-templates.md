# Prompt Templates for Web Development

> Copy-paste these prompts into GitHub Copilot Chat, ChatGPT, or Cursor. Replace `[brackets]` with your specifics.

---

## 1. Component Creation

```
Create a React component called [ComponentName] in TypeScript.

Props:
- [prop1]: [type] — [description]
- [prop2]: [type] — [description]

Requirements:
- [Functional requirement]
- Tailwind CSS styling, responsive (mobile-first)
- Loading and error states
- Accessible with ARIA labels
- Co-located test file using React Testing Library

Tech stack: Next.js 14, TypeScript, Tailwind CSS.
```

## 2. API Design

```
Design a REST API endpoint for [resource].

Method: [GET/POST/PUT/DELETE]
Path: /api/[resource]
Auth: [required/public]

Include:
- Zod validation schema
- Prisma database operations
- Proper HTTP status codes (200, 201, 400, 401, 404, 500)
- Pagination for list endpoints
- Error response format: { success: false, error: { code, message } }

Database model:
[paste Prisma schema]
```

## 3. Debugging

```
I'm getting this error in my Next.js app:

Error: [paste error message]

File: [filename]
Line: [line number]

Code context:
[paste relevant code]

What I've tried:
- [attempt 1]
- [attempt 2]

Tech stack: Next.js 14, TypeScript, Prisma, Tailwind.
Please explain the root cause and provide a fix.
```

## 4. Testing

```
Write tests for this component/function:

[paste code]

Include:
- Unit tests for all public methods
- Edge cases (empty input, null, boundary values)
- Error handling scenarios
- React Testing Library for components
- Jest for utility functions
- Mock external dependencies (API calls, database)

Aim for >80% coverage of critical paths.
```

## 5. Styling

```
Create a responsive [component type] using Tailwind CSS.

Design requirements:
- [Layout description]
- Color scheme: [primary color] with [accent color]
- Typography: [font sizes, weights]
- Spacing: [padding/margin preferences]
- Hover/focus states
- Dark mode support
- Mobile: [mobile layout]
- Desktop: [desktop layout]

Reference: [link to design or describe visual]
```

## 6. Deployment

```
Set up deployment for my Next.js app on Vercel.

Current setup:
- Repo: [GitHub repo URL]
- Branch: main
- Environment variables needed: [list vars]

I need:
- vercel.json configuration
- Environment variable setup
- Preview deployments for PRs
- Custom domain setup: [domain.com]
- Build optimization settings
```

## 7. Refactoring

```
Refactor this code to improve [quality goal]:

[paste code]

Goals:
- [ ] Better TypeScript types (remove `any`)
- [ ] Extract reusable logic into custom hooks
- [ ] Improve performance (memoization, lazy loading)
- [ ] Follow project naming conventions
- [ ] Add proper error boundaries
- [ ] Reduce component complexity

Constraints: Don't change the public API/interface.
```

## 8. Documentation

```
Generate documentation for this [API/component/module]:

[paste code]

Include:
- Overview and purpose
- Usage examples with code
- Props/parameters table
- Return values
- Error handling
- Common pitfalls
- Related components/APIs

Format: Markdown, suitable for README or Storybook.
```

---

## Tips for Better Prompts

1. **Be specific** — Include file paths, tech stack, and exact requirements
2. **Provide context** — Paste relevant code, schemas, and error messages
3. **State constraints** — Mention TypeScript strict mode, accessibility, performance
4. **Ask for alternatives** — "Suggest 2 approaches and recommend the best one"
5. **Iterate** — Follow up with "Now add error handling" or "Make it more accessible"
