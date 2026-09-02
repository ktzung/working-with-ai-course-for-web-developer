# Ethics and Code Quality with AI

## The Responsibility Question

When AI writes your code, who's responsible for it? The answer is simple: you are. Every line of AI-generated code that makes it into production is your responsibility. This lesson covers the ethical and practical considerations of working with AI-generated code.

## Code Review Is Non-Negotiable

AI-generated code needs the same scrutiny as code written by a junior developer — maybe more. Here's why:

**AI doesn't understand your business logic.** It might generate syntactically perfect code that completely misses your requirements. A function that sorts users by "popularity" might use the wrong metric if you don't specify what popularity means.

**AI can introduce subtle bugs.** Race conditions, off-by-one errors, incorrect null checks — these are common in AI-generated code because the AI is pattern-matching, not reasoning about your specific data flow.

**AI may use outdated patterns.** Training data has a cutoff date. Your AI might suggest deprecated APIs, old React patterns (like class components when you want hooks), or security practices that are no longer recommended.

### The Review Checklist

For every piece of AI-generated code, check:

1. **Does it do what I asked?** — Read the code, don't just run it
2. **Are there security issues?** — SQL injection, XSS, hardcoded secrets
3. **Does it handle edge cases?** — Empty arrays, null values, network errors
4. **Is it consistent with my codebase?** — Naming conventions, file structure, patterns
5. **Are there performance concerns?** — Unnecessary re-renders, N+1 queries, memory leaks

## Security Considerations

AI tools are trained on public code — including code with security vulnerabilities. Common issues:

### Hardcoded Credentials
AI might generate code with placeholder API keys or database strings. Always replace these with environment variables.

```typescript
// ❌ AI might generate this
const apiKey = "sk-1234567890abcdef";

// ✅ Always use environment variables
const apiKey = process.env.API_KEY;
```

### SQL Injection
AI-generated database queries might not use parameterized queries:

```typescript
// ❌ Vulnerable to injection
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE id = $1';
const result = await db.query(query, [userId]);
```

### XSS Vulnerabilities
When generating HTML or React components, AI might not properly sanitize user input:

```typescript
// ❌ Dangerous
<div dangerouslySetInnerHTML={{ __html: userContent }} />

// ✅ Sanitized
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userContent) }} />
```

## Code Ownership and Licensing

AI tools are trained on open-source code. This raises questions:

- **Can you use AI-generated code in commercial projects?** Generally yes, but check your AI tool's terms of service.
- **Does AI-generated code have copyright?** This is legally unsettled. Most experts agree that purely AI-generated code isn't copyrightable, but code you significantly modify may be.
- **Should you attribute AI assistance?** This is a team decision. Some teams document AI usage in commit messages or code comments.

## Maintaining Code Quality

### Establish Team Standards

If your team uses AI tools, agree on:
- **Review requirements** for AI-generated code
- **Testing standards** (AI code needs tests too)
- **Documentation expectations** (explain what the code does, not just that AI wrote it)
- **Security scanning** as part of CI/CD

### Don't Blindly Accept

The biggest risk isn't that AI writes bad code — it's that developers accept it without understanding it. If you can't explain what the code does, don't merge it.

### Keep Learning

AI is a tool, not a replacement for understanding. Use AI to write code faster, but make sure you understand:
- Why the code works
- What patterns it uses
- How it could fail
- How to improve it

## The Human Element

AI can write code, but it can't:
- Understand your users' needs
- Make product decisions
- Navigate team dynamics
- Take responsibility for bugs in production

Your value as a developer isn't in typing code — it's in solving problems, making decisions, and building things that matter. AI just helps you do it faster.

## What's Next

In the next lesson, we'll set up your development workspace with the right AI tools and extensions to maximize your productivity.
