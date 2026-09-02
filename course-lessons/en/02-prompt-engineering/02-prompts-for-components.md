# Prompts for Generating Components

## Component Generation with AI

Components are the building blocks of modern web applications. Whether you use React, Vue, or Angular, AI can generate well-structured components when you provide the right prompts. Let's master component generation.

## React Component Prompts

### Basic Component

```
Create a React component called UserAvatar using TypeScript.

Props:
- src: string (image URL)
- alt: string (alt text)
- size: 'sm' | 'md' | 'lg' (default: 'md')
- showStatus: boolean (default: false)
- status: 'online' | 'offline' | 'away'

Features:
- Circular avatar with border
- Status indicator dot (bottom-right corner)
- Fallback to initials if image fails to load
- Smooth hover scale effect

Styling: Tailwind CSS
File: src/components/ui/UserAvatar.tsx
Export as named export.
```

### Complex Component with State

```
Create a React component called SearchFilter using TypeScript.

This component provides a search bar with filter dropdowns for a project list.

Props:
- onFilterChange: (filters: FilterState) => void
- availableTags: string[]
- availableStatuses: string[]

State (use useReducer):
- searchQuery: string
- selectedTags: string[]
- selectedStatus: string
- sortBy: 'name' | 'date' | 'stars'
- sortOrder: 'asc' | 'desc'

Features:
- Debounced search input (300ms)
- Multi-select tag filter with checkboxes
- Single-select status dropdown
- Sort controls with direction toggle
- Clear all filters button
- Active filter count badge

Styling: Tailwind CSS with dark mode support
File: src/components/filters/SearchFilter.tsx
Include TypeScript types in a separate types file.
```

### Component with API Integration

```
Create a React component called ProjectList using TypeScript.

This component fetches and displays a paginated list of projects.

Props:
- initialPage?: number (default: 1)
- pageSize?: number (default: 12)
- filters?: FilterState

Features:
- Fetch projects from /api/projects with pagination
- Loading skeleton while fetching
- Error state with retry button
- Empty state with illustration
- Infinite scroll or "Load More" button
- Project cards in responsive grid (1 col mobile, 2 tablet, 3 desktop)

Each ProjectCard shows:
- Project thumbnail
- Title and description
- Tech stack badges
- Star count and last updated date
- Link to project detail

Styling: Tailwind CSS
File: src/components/projects/ProjectList.tsx
Use the custom useProjects hook from hooks/useProjects.ts
```

## Vue Component Prompts

### Basic Vue Component

```
Create a Vue 3 component using Composition API and TypeScript.

Component name: TechBadge
File: src/components/ui/TechBadge.vue

Props:
- name: string (technology name)
- icon?: string (icon class or URL)
- color?: string (badge color, default based on tech)
- size: 'sm' | 'md' | 'lg'

Features:
- Rounded pill badge with icon and text
- Color coding by technology (React=blue, Vue=green, etc.)
- Hover effect with slight scale
- Click emits 'select' event with tech name

Styling: Tailwind CSS with scoped styles
Use <script setup> syntax.
```

### Complex Vue Component

```
Create a Vue 3 component using Composition API and TypeScript.

Component name: KanbanBoard
File: src/components/dashboard/KanbanBoard.vue

Props:
- projectId: string

Features:
- Three columns: To Do, In Progress, Done
- Drag and drop between columns using @vueuse/core's useDraggable
- Add new task with inline form
- Task cards with title, assignee avatar, priority badge
- Column task count
- Persist order to API on change

State management: Pinia store
Styling: Tailwind CSS
Use <script setup> and defineProps/defineEmits.
```

## Component Patterns to Request

### Compound Component
```
Create a compound component called Tabs with:
- Tabs.Container (manages state)
- Tabs.List (tab buttons)
- Tabs.Tab (individual tab)
- Tabs.Panels (content area)
- Tabs.Panel (individual panel)

Support keyboard navigation (arrow keys, Home, End).
Use React Context for state sharing between sub-components.
```

### Render Props Component
```
Create a DataFetcher component using render props pattern:
- url: string
- render: (data, loading, error) => ReactNode
- fallback?: ReactNode

Handles loading, error, and success states.
Includes retry logic and caching.
```

### Higher-Order Component
```
Create a withAuth HOC that:
- Wraps any component
- Checks authentication status
- Redirects to /login if not authenticated
- Passes user data as prop
- Preserves component displayName
```

## Prompt Tips for Better Components

### Be Specific About Types
```
// ❌ Vague
Props: user object

// ✅ Specific
Props: user: {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  role: 'admin' | 'member' | 'viewer';
  createdAt: Date;
}
```

### Describe Interactions
```
When the user clicks the "Delete" button:
1. Show a confirmation modal
2. If confirmed, call DELETE /api/projects/:id
3. Show loading spinner on the button
4. On success, remove from list with fade animation
5. On error, show toast notification
```

### Include Accessibility
```
- Use semantic HTML (button, nav, main, etc.)
- Include aria-labels for icon-only buttons
- Support keyboard navigation
- Manage focus for modals
- Include proper heading hierarchy
```

## Practice Exercise

Generate these components for WebDevHub:

1. **ProjectCard** — Displays a project with thumbnail, title, tech stack, and stats
2. **SnippetEditor** — Code editor with syntax highlighting and language selection
3. **StatsWidget** — Dashboard widget showing project statistics with charts

Write detailed prompts for each, then generate the code with your AI tool.

## What's Next

In the next lesson, we'll learn how to prompt AI for API design — creating RESTful endpoints, GraphQL schemas, and API documentation.
