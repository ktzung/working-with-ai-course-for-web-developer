# AI-Assisted Web Development — Best Practices

> Proven patterns for getting the most out of AI tools in web development projects.

## 1. Provide Rich Context

AI tools generate better code when they understand your project.

**Do this:**
```
I'm building a Next.js 14 app with TypeScript, Tailwind CSS, and Prisma.
The User model has: id, email, name, avatar, projects (relation).
I need a dashboard component that shows the user's projects in a grid.
```

**Not this:**
```
Make a dashboard component.
```

**Tips:**
- Keep `PROJECT_CONTEXT.md` updated and reference it in prompts
- Paste relevant schema, types, and existing code
- Mention your coding standards and conventions
- Specify the file path where code should be created

## 2. Iterate, Don't Generate Everything at Once

Break complex features into small, verifiable steps.

**Workflow:**
1. Generate the data model / schema first
2. Build the API endpoint
3. Create the UI component
4. Add tests
5. Refine and optimize

Each step is a focused prompt with clear inputs and outputs.

## 3. Use AI for Boilerplate, Think for Architecture

| Let AI Handle | You Should Decide |
|--------------|-------------------|
| CRUD boilerplate | Data model design |
| Test scaffolding | Test strategy |
| Component structure | State management approach |
| CSS layouts | Design system choices |
| API route handlers | API architecture |
| Documentation | Feature requirements |

## 4. Validate AI Output

Always review AI-generated code before committing.

**Checklist:**
- [ ] Does it compile without errors?
- [ ] Are types correct (no `any`)?
- [ ] Does it handle edge cases (null, empty, error)?
- [ ] Is it secure (no XSS, proper auth)?
- [ ] Does it follow your project conventions?
- [ ] Are there unnecessary dependencies?

## 5. Use AI for Code Review

After writing code, ask AI to review it.

```
Review this code for:
1. TypeScript type safety
2. React best practices
3. Potential bugs
4. Performance issues
5. Security concerns

[paste code]
```

## 6. Leverage AI for Testing

AI excels at generating comprehensive test suites.

```
Write tests for this component covering:
- Happy path rendering
- User interactions
- Loading and error states
- Edge cases (empty data, long text)
- Accessibility (keyboard nav, screen reader)

[paste component code]
```

## 7. Debug Systematically with AI

When encountering errors, provide full context:

```
Error: [exact error message]
File: [filename and line]
Code: [relevant code snippet]
What I tried: [previous attempts]
Expected: [what should happen]
Actual: [what actually happens]
```

## 8. Document as You Go

Use AI to generate documentation alongside code:

```
Generate a README section for this API endpoint:
- Purpose and use case
- Request/response examples
- Error codes
- Rate limits

[paste endpoint code]
```

## 9. Refactor with AI Assistance

Periodically ask AI to review and improve existing code:

```
Refactor this component to:
- Extract reusable logic into a custom hook
- Improve TypeScript types
- Add proper error boundaries
- Optimize re-renders

[paste component code]
```

## 10. Build a Prompt Library

Save prompts that work well for your project:

| Task | Prompt Template |
|------|----------------|
| New component | [template from prompt-templates.md] |
| API endpoint | [template from prompt-templates.md] |
| Bug fix | [template from prompt-templates.md] |
| Code review | [template from agents/code-reviewer.md] |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Accepting AI code without review | Always read and understand generated code |
| Vague prompts | Include tech stack, file paths, and examples |
| Generating too much at once | Break into small, testable pieces |
| Not providing existing code | Paste related code for context |
| Ignoring TypeScript errors | Fix type errors immediately, don't suppress |
| Copy-pasting without adapting | Adjust to your project's patterns |

## Tool-Specific Tips

### GitHub Copilot
- Write descriptive comments before functions for better suggestions
- Use `@workspace` to reference your entire project
- Accept suggestions with Tab, reject with Escape

### Cursor
- Use Cmd+K for inline edits
- Use Cmd+L for chat-based generation
- Reference files with @filename for context

### ChatGPT
- Use system prompts for consistent behavior
- Provide code in markdown blocks for better parsing
- Ask for alternatives: "Suggest 2 approaches"
