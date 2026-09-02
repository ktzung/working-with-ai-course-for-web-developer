# The current-task.md File

## Focused Context for What You're Building Now

While `project-context.md` describes your entire project, `current-task.md` focuses AI on what you're building right now. This prevents AI from suggesting changes to unrelated parts of your codebase.

## Why current-task.md Matters

Without task focus, AI might:
- Suggest refactoring code you're not working on
- Generate features that conflict with your current work
- Miss the specific requirements of your task
- Create code that doesn't integrate with your current changes

## The Structure

```markdown
# Current Task: User Authentication

## Objective
Implement user authentication with NextAuth.js, including login, 
registration, and protected routes.

## Requirements
- [ ] Login page with email/password
- [ ] Registration page with form validation
- [ ] GitHub OAuth provider
- [ ] Google OAuth provider
- [ ] Protected dashboard routes
- [ ] User profile in header
- [ ] Logout functionality

## Technical Details

### Files to Modify
- `src/app/(auth)/login/page.tsx` — Login page
- `src/app/(auth)/register/page.tsx` — Registration page
- `src/app/(auth)/layout.tsx` — Auth layout
- `src/components/auth/LoginForm.tsx` — Login form component
- `src/components/auth/RegisterForm.tsx` — Registration form component
- `src/lib/auth.ts` — NextAuth configuration
- `src/middleware.ts` — Route protection

### Files to Create
- `src/app/api/auth/[...nextauth]/route.ts` — NextAuth API route
- `src/components/auth/ProtectedRoute.tsx` — Route guard component
- `src/hooks/useAuth.ts` — Auth hook

### Dependencies
- next-auth@4.x
- @next-auth/prisma-adapter
- bcrypt (for password hashing)

### Database Changes
- Add Account model (for OAuth)
- Add Session model
- Add VerificationToken model
- Update User model with password field

## Current Progress
- [x] Installed next-auth
- [x] Created Prisma schema
- [ ] NextAuth configuration
- [ ] Login page
- [ ] Registration page
- [ ] Protected routes

## Known Issues
- Need to handle CSRF tokens properly
- Password hashing should use bcrypt with 12 rounds
- Must validate email format before saving

## Context for AI
When generating code for this task:
- Use NextAuth.js v4 with App Router
- Follow the existing project patterns in project-context.md
- Use Prisma adapter for database sessions
- Implement proper error handling
- Add loading states for auth operations
```

## How to Use current-task.md

### At the Start of a Session
1. Open `current-task.md` in your editor
2. Reference it in your first prompt:
   ```
   I'm working on user authentication. Here's my current task:
   [paste current-task.md]
   
   Help me implement the NextAuth configuration.
   ```

### During Development
Update the file as you work:
- Check off completed requirements
- Add new issues you discover
- Update technical details

### When Switching Tasks
Create a new `current-task.md` or update the existing one with your new focus.

## Task Templates

### Feature Implementation
```markdown
# Current Task: [Feature Name]

## Objective
[One sentence description of what you're building]

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Technical Details
### Files to Modify
- file1.tsx — [what changes]
- file2.ts — [what changes]

### Files to Create
- newfile.tsx — [purpose]

### Dependencies
- package@version — [why]

## Current Progress
- [x] Completed step
- [ ] Remaining step

## Context for AI
[Specific instructions for AI]
```

### Bug Fix
```markdown
# Current Task: Fix [Bug Description]

## Problem
[What's happening vs. what should happen]

## Steps to Reproduce
1. Go to [page]
2. Click [button]
3. See [error]

## Expected Behavior
[What should happen]

## Current Behavior
[What actually happens]

## Error Details
```
[Paste error message/stack trace]
```

## Investigation
- [ ] Check [area 1]
- [ ] Check [area 2]
- [ ] Check [area 3]

## Context for AI
[What you need help with]
```

### Refactoring
```markdown
# Current Task: Refactor [Area]

## Current State
[Description of current implementation]

## Problems
- Problem 1
- Problem 2
- Problem 3

## Target State
[Description of desired implementation]

## Files Affected
- file1.tsx — [changes needed]
- file2.ts — [changes needed]

## Constraints
- Must maintain backward compatibility
- Cannot change public API
- Must pass existing tests

## Context for AI
[What you need help with]
```

## Combining with project-context.md

Use both files together:

```
Project context: [paste relevant sections from project-context.md]
Current task: [paste current-task.md]

Generate the NextAuth configuration that fits my project's patterns 
and implements the requirements in my current task.
```

## Practice Exercise

1. Create a `current-task.md` for a feature you're working on
2. Use it in a conversation with AI
3. Update it as you make progress
4. Notice how it keeps AI focused on your actual task

## What's Next

In the next lesson, we'll set up the complete development workspace — folder structure, Git workflow, and all the context files working together.
