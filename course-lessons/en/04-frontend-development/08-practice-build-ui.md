# Practice: Build a Complete UI Page

## Putting It All Together

Time to apply everything you've learned! In this practice lesson, you'll build a complete dashboard page for WebDevHub using AI assistance.

## The Challenge

Build the **Project Dashboard** page with these features:

1. **Header** — User info, search, notifications
2. **Stats Section** — 4 metric cards
3. **Project Grid** — Filterable project cards
4. **Activity Feed** — Recent activities
5. **Quick Actions** — Common actions panel

## Step 1: Plan the Architecture

Write a prompt to plan the component architecture:

```
I'm building a project dashboard page for WebDevHub.

The page includes:
- Header with user avatar, search bar, and notification bell
- Stats section with 4 cards (total projects, tasks done, collaborators, stars)
- Project grid with filter bar (search, status filter, tech filter)
- Activity feed showing recent actions
- Quick actions panel (new project, new snippet, invite member)

Tech stack: Next.js 14, TypeScript, Tailwind CSS, Zustand

Help me plan:
1. Component hierarchy
2. Data requirements for each component
3. State management approach
4. File structure
```

## Step 2: Build the Stats Section

```
Create a StatsSection component for the dashboard.

It should display 4 stat cards in a responsive grid:
- Mobile: 2x2 grid
- Tablet: 4 columns
- Desktop: 4 columns

Each stat card shows:
- Icon (from Lucide React)
- Label (e.g., "Total Projects")
- Value (number)
- Trend indicator (up/down arrow with percentage)
- Sparkline chart (last 7 days)

Stats data:
- Total Projects: 24 (+12% from last week)
- Tasks Completed: 156 (+8% from last week)
- Collaborators: 8 (+2 new this month)
- Total Stars: 342 (+23% from last week)

Use Tailwind CSS for styling.
Include loading skeleton state.
```

## Step 3: Build the Project Grid

```
Create a ProjectGrid component with filtering.

Features:
- Filter bar with:
  - Search input (debounced)
  - Status filter (All, Active, Archived, Draft)
  - Tech stack filter (multi-select)
  - Sort by (Name, Updated, Stars)
- Project cards in responsive grid (1/2/3 columns)
- Loading skeleton
- Empty state
- "Load More" button

Each ProjectCard shows:
- Thumbnail image
- Project name
- Description (truncated)
- Tech stack badges
- Status badge
- Star count
- Last updated time
- Action menu (Edit, Archive, Delete)

Use Zustand for filter state.
Use React Query for data fetching.
```

## Step 4: Build the Activity Feed

```
Create an ActivityFeed component.

Features:
- List of recent activities
- Each activity shows:
  - User avatar
  - Action description
  - Timestamp (relative: "2 hours ago")
  - Link to related item
- Loading skeleton
- "View All" link at bottom

Activity types:
- project_created: "created project {name}"
- task_completed: "completed task {title} in {project}"
- member_joined: "joined as collaborator"
- star_received: "received a star on {project}"

Use date-fns for relative timestamps.
```

## Step 5: Build the Quick Actions Panel

```
Create a QuickActions component.

Actions:
- New Project (opens create project modal)
- New Snippet (opens snippet editor)
- Invite Member (opens invite dialog)
- View Analytics (navigates to analytics page)

Features:
- Icon buttons with labels
- Hover effects
- Keyboard shortcuts displayed
- Responsive layout (horizontal on desktop, grid on mobile)
```

## Step 6: Assemble the Page

```
Create the DashboardPage component that assembles all sections.

Layout:
- Full width header
- Content area with max-width container
- Stats section at top
- Main content: 2-column layout
  - Left (8/12): Project grid
  - Right (4/12): Activity feed + Quick actions
- On mobile: Single column, stacked

Include:
- Page title and breadcrumb
- Loading states for each section
- Error boundaries
- Responsive layout
```

## Step 7: Add Interactions

```
Add these interactions to the dashboard:

1. Search: Debounced search updates project grid
2. Filters: Filter changes update URL params
3. Project card click: Navigate to project detail
4. Quick actions: Open respective modals/pages
5. Stats cards: Click to view detailed analytics
6. Activity items: Click to navigate to related item
7. Pull to refresh on mobile
```

## Step 8: Polish and Optimize

```
Polish the dashboard:

Performance:
- Lazy load below-fold sections
- Memoize expensive computations
- Optimize re-renders

Accessibility:
- Keyboard navigation
- Screen reader support
- Focus management
- ARIA labels

Visual:
- Smooth animations
- Loading skeletons
- Error states
- Empty states
```

## Evaluation Criteria

Rate your implementation:

| Criteria | Points |
|----------|--------|
| Component architecture | 0-2 |
| Responsive design | 0-2 |
| State management | 0-2 |
| Form handling | 0-2 |
| Accessibility | 0-2 |
| Performance | 0-2 |
| Code quality | 0-2 |
| Visual polish | 0-2 |
| **Total** | **0-16** |

## What You've Learned

By completing this practice, you've demonstrated:

1. **Prompt Engineering** — Writing effective prompts for complex features
2. **Context Engineering** — Using project context for consistent code
3. **Component Architecture** — Designing modular component systems
4. **React Development** — Building components with hooks and state
5. **Styling** — Creating responsive designs with Tailwind CSS
6. **State Management** — Implementing Zustand and React Query
7. **Form Handling** — Building validated forms with React Hook Form

## What's Next

Congratulations on completing Section 4! In the upcoming sections, you'll learn about backend development, API design, database integration, and deployment — all with AI assistance.
