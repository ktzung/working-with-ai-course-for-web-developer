# Component Architecture with AI

## Designing Component Hierarchies

Good component architecture is the foundation of maintainable frontend code. AI can help you design component hierarchies that are modular, reusable, and easy to understand.

## The Component Thinking Process

Before generating code, think about:

1. **What is the UI composed of?** — Identify visual elements
2. **What data does each element need?** — Define props
3. **How do elements interact?** — Map relationships
4. **What can be reused?** — Extract shared components

## AI-Assisted Architecture Design

### Prompt for Component Analysis

```
I'm building a project dashboard for WebDevHub. Here's the UI description:

The dashboard has:
- A header with user info and notifications
- A sidebar with navigation
- A main content area with:
  - Stats cards (total projects, tasks completed, active collaborators)
  - A project list with filters
  - A recent activity feed
  - A quick actions panel

Help me design the component hierarchy. For each component, specify:
- Component name
- Props it needs
- Whether it's a container or presentational component
- Where it should live in the file structure
```

### AI Response Example

```
Component Hierarchy:

src/components/
├── layout/
│   ├── DashboardLayout.tsx        # Container: manages layout state
│   ├── Header.tsx                 # Presentational: displays user info
│   ├── Sidebar.tsx                # Presentational: navigation links
│   └── MainContent.tsx            # Presentational: content wrapper
├── dashboard/
│   ├── StatsSection.tsx           # Container: fetches and manages stats
│   ├── StatsCard.tsx              # Presentational: single stat display
│   ├── ProjectListSection.tsx     # Container: manages project list
│   ├── ProjectCard.tsx            # Presentational: single project
│   ├── ActivityFeed.tsx           # Container: fetches activities
│   ├── ActivityItem.tsx           # Presentational: single activity
│   └── QuickActions.tsx           # Presentational: action buttons
└── shared/
    ├── FilterBar.tsx              # Reusable: filter controls
    └── EmptyState.tsx             # Reusable: empty state display
```

## Component Design Patterns

### Container/Presentational Split

```
Design a component system using the Container/Presentational pattern:

Container components:
- Handle data fetching
- Manage state
- Handle business logic
- Pass data down as props

Presentational components:
- Receive data via props
- Handle display logic only
- Emit events via callbacks
- No direct API calls

For the project list feature, create:
1. ProjectListContainer (fetches, filters, paginates)
2. ProjectList (renders the grid)
3. ProjectCard (displays single project)
4. ProjectCardSkeleton (loading state)
```

### Compound Components

```
Design a compound component for a data table:

DataTable
├── DataTable.Header
│   └── DataTable.ColumnHeader (sortable)
├── DataTable.Body
│   └── DataTable.Row
│       └── DataTable.Cell
├── DataTable.Footer
│   └── DataTable.Pagination
└── DataTable.Empty

Features:
- Sortable columns
- Selectable rows
- Pagination
- Loading skeleton
- Empty state

Use React Context to share state between sub-components.
```

## Prompting for Architecture

### The Analysis Prompt

```
Analyze this UI and suggest a component architecture:

[paste screenshot or describe UI]

Requirements:
- Maximum 3 levels of nesting
- Each component should have a single responsibility
- Identify reusable components
- Suggest state management approach
- Consider loading and error states
```

### The Refactoring Prompt

```
I have a large component (500+ lines) that handles the entire project page.

Here's the code: [paste code]

Help me break it down into smaller, focused components:
1. Identify logical boundaries
2. Extract reusable pieces
3. Determine what state belongs where
4. Maintain the same functionality
```

## Architecture Decision Records

Document your decisions:

```markdown
# ADR: Component Architecture

## Context
We need a consistent approach to component organization.

## Decision
- Use Container/Presentational pattern
- Max 3 levels of component nesting
- One component per file
- Co-locate related files (component + test + styles)

## Consequences
- Easier to test presentational components
- Clear separation of concerns
- May need more files for simple features
```

## Practice Exercise

Design the component architecture for WebDevHub's snippet manager feature:

Requirements:
- List of code snippets with search
- Snippet cards with preview
- Snippet editor with syntax highlighting
- Language filter
- Tag management

Write a prompt that generates the complete architecture, then implement the components.

## What's Next

In the next lesson, we'll use AI to build React components — from simple UI elements to complex interactive features.
