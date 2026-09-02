# Prompt Workshop

## Practice with Real Scenarios

Time to put your prompt engineering skills to the test! In this workshop, you'll work through 8 real-world web development scenarios. For each one, you'll write a prompt, generate code with AI, and refine the results.

## Scenario 1: User Registration Form

**Requirement**: Build a registration form for WebDevHub with email, password, name, and role selection.

**Your task**: Write a prompt that generates:
- React Hook Form with Zod validation
- Password strength indicator
- Email availability check (debounced)
- Terms of service acceptance
- Loading state on submit
- Error handling with user-friendly messages

**Prompt template**:
```
Create a registration form component for Next.js 14 with TypeScript.

[Add your specific requirements here]
```

**Evaluation criteria**:
- Does the form validate correctly?
- Are error messages helpful?
- Is the password indicator accurate?
- Does it handle network errors?

## Scenario 2: Project Dashboard Widget

**Requirement**: Create a statistics widget showing project metrics.

**Your task**: Write a prompt for a component that displays:
- Total projects count
- Tasks completed this week
- Active collaborators
- Trend indicators (up/down arrows with percentages)
- Mini sparkline chart
- Responsive layout

**Challenge**: Make it work with mock data first, then connect to real API.

## Scenario 3: Drag-and-Drop Kanban Board

**Requirement**: Build a Kanban board for task management.

**Your task**: Write a prompt for:
- Three columns (To Do, In Progress, Done)
- Drag and drop between columns
- Add new task inline
- Task cards with priority badges
- Column task counts
- Persist order to API

**Hint**: Specify the drag-and-drop library (dnd-kit, react-beautiful-dnd, or native HTML5 DnD).

## Scenario 4: Code Snippet Manager

**Requirement**: Create a code snippet component with syntax highlighting.

**Your task**: Write a prompt for:
- Code editor with syntax highlighting (Prism.js or highlight.js)
- Language selector dropdown
- Copy to clipboard button
- Line numbers
- Dark/light theme toggle
- Save and share functionality

**Challenge**: Support 10+ programming languages.

## Scenario 5: Search with Filters

**Requirement**: Build a search feature with multiple filter options.

**Your task**: Write a prompt for:
- Debounced search input
- Filter by technology stack (multi-select)
- Filter by status (single select)
- Sort options (name, date, stars)
- Active filter pills with remove buttons
- URL state synchronization (shareable search URLs)

**Evaluation**: Does search feel instant? Are filters intuitive?

## Scenario 6: Responsive Navigation

**Requirement**: Create a navigation component that works on all devices.

**Your task**: Write a prompt for:
- Desktop: Horizontal nav with dropdowns
- Tablet: Collapsed nav with hamburger menu
- Mobile: Bottom tab bar
- Active state indicators
- User avatar with dropdown menu
- Notification bell with count badge
- Smooth transitions between states

## Scenario 7: API Integration Layer

**Requirement**: Create a reusable API client for WebDevHub.

**Your task**: Write a prompt for:
- Type-safe API client using TypeScript
- Automatic token refresh
- Request/response interceptors
- Error handling with custom error types
- Loading state management
- Request cancellation
- Retry logic for failed requests

**Hint**: Consider using axios or a custom fetch wrapper.

## Scenario 8: Real-Time Notifications

**Requirement**: Add a notification system to WebDevHub.

**Your task**: Write a prompt for:
- Toast notifications (success, error, warning, info)
- Notification center dropdown
- Real-time updates via WebSocket or SSE
- Mark as read/unread
- Notification preferences
- Sound and desktop notification support

## Workshop Process

For each scenario:

1. **Write your prompt** (5 minutes)
   - Use the four elements: Context, Task, Tech Stack, Constraints
   - Be specific about types, edge cases, and interactions

2. **Generate code** (5 minutes)
   - Use your preferred AI tool
   - Generate the initial implementation

3. **Review and refine** (10 minutes)
   - Check for bugs and edge cases
   - Verify TypeScript types
   - Test responsiveness
   - Check accessibility

4. **Iterate** (5 minutes)
   - Ask AI to improve specific aspects
   - Add missing features
   - Optimize performance

## Scoring Rubric

Rate your prompts on:

| Criteria | Points |
|----------|--------|
| Specific tech stack | 0-2 |
| Clear task description | 0-2 |
| Edge cases covered | 0-2 |
| Type safety | 0-2 |
| Accessibility | 0-2 |
| **Total** | **0-10** |

## After the Workshop

Reflect on:
- Which scenarios were hardest to prompt for?
- What patterns did you discover?
- How did your prompts improve from scenario 1 to 8?
- What will you do differently next time?

## What's Next

In Section 3, we'll dive into context engineering — learning how to set up your project so AI tools understand your codebase and generate more relevant code.
