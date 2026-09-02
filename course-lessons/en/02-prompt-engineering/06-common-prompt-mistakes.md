# Common Prompt Mistakes

## Avoiding Pitfalls in AI-Assisted Development

Even experienced developers make prompt mistakes. Recognizing these patterns will help you get better results from AI tools and avoid frustrating back-and-forth conversations.

## Mistake 1: Being Too Vague

**Bad**: "Make a login page"

**Why it fails**: AI doesn't know your framework, styling approach, auth method, or design preferences.

**Better**: "Create a login page for Next.js 14 with TypeScript. Use React Hook Form with Zod validation. Include email and password fields, a 'Remember me' checkbox, social login buttons (Google, GitHub), and a link to registration. Style with Tailwind CSS, centered card layout, max-width 400px."

## Mistake 2: Not Specifying the Tech Stack

**Bad**: "Create a function to fetch users"

**Why it fails**: Should it use fetch, axios, or a library? What about error handling? TypeScript types?

**Better**: "Create a function using TypeScript that fetches users from /api/users using the native fetch API. Include pagination params (page, limit), loading state, error handling with try/catch, and return type { users: User[], total: number }."

## Mistake 3: Asking for Too Much at Once

**Bad**: "Build me a complete e-commerce platform with user auth, product catalog, shopping cart, payment processing, order management, and admin dashboard"

**Why it fails**: This is too broad. AI will generate something generic that doesn't match your needs.

**Better**: Break it down:
1. "Create the product data model with Prisma"
2. "Build a ProductCard component"
3. "Create the product listing API endpoint"
4. "Build the shopping cart with Zustand"

## Mistake 4: Not Providing Context

**Bad**: "Fix this error: Cannot read property 'map' of undefined"

**Why it fails**: AI doesn't know where the error occurs or what data structure you're working with.

**Better**: "I'm getting 'Cannot read property map of undefined' in my ProjectList component when the API returns an empty response. Here's my component code: [paste]. The data comes from useProjects hook: [paste]. How should I handle the case when projects is undefined?"

## Mistake 5: Ignoring Existing Code Patterns

**Bad**: "Create a new API endpoint for tasks"

**Why it fails**: AI might generate code that doesn't match your existing patterns.

**Better**: "Following the same pattern as my existing /api/projects endpoint (paste the code), create a similar endpoint for tasks. Use the same error handling, validation approach, and response format."

## Mistake 6: Not Specifying Edge Cases

**Bad**: "Create a date formatter"

**Why it fails**: What about null dates? Invalid dates? Timezones? Different formats?

**Better**: "Create a date formatter that handles: null/undefined input (returns 'N/A'), invalid dates (returns 'Invalid Date'), timezone conversion to UTC, and supports formats: 'relative' (2 hours ago), 'short' (Jan 1, 2024), 'long' (January 1, 2024 at 3:00 PM)."

## Mistake 7: Accepting First Result Without Review

**The problem**: AI generates code that looks right but has subtle issues.

**Solution**: Always review AI-generated code for:
- Security vulnerabilities (XSS, injection)
- Performance issues (unnecessary re-renders, missing memoization)
- Edge cases (null checks, error handling)
- Code style consistency
- TypeScript type safety

## Mistake 8: Not Iterating

**Bad**: Getting mediocre results and giving up.

**Better**: Iterate! If the first result isn't perfect:
- "Make it more accessible"
- "Add dark mode support"
- "Optimize for mobile"
- "Add error boundaries"
- "Include unit tests"

## Mistake 9: Copy-Pasting Errors Without Context

**Bad**: Pasting a 50-line stack trace with no explanation.

**Better**: "I'm getting this error after adding the search feature to my project list. The error occurs when I type in the search input. Here's the relevant code and error: [paste]. I think it's related to the debounce function."

## Mistake 10: Not Using AI for Learning

**The problem**: Only using AI to generate code, never to understand it.

**Better**: Ask follow-up questions:
- "Explain why you used useCallback here"
- "What are the trade-offs of this approach?"
- "How would this scale with 10,000 items?"
- "What are alternative implementations?"

## Quick Reference: Prompt Checklist

Before sending a prompt, check:

- [ ] Specific tech stack mentioned?
- [ ] Clear task description?
- [ ] Relevant code/context included?
- [ ] Edge cases specified?
- [ ] Constraints defined?
- [ ] Expected output format clear?

## Practice Exercise

Fix these bad prompts:

1. **Bad**: "Make a search feature"
2. **Bad**: "My code doesn't work"
3. **Bad**: "Add styling to this component"
4. **Bad**: "Create a database schema"

Rewrite each using the patterns from this lesson.

## What's Next

In the next lesson, we'll put everything together in a prompt workshop where you'll practice with 8 real-world web development scenarios.
