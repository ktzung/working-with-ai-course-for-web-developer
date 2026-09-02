# State Management with AI

## Choosing the Right State Solution

State management is crucial for complex web applications. AI can help you implement the right solution for each type of state in your application.

## Types of State

### Local State
- Form inputs
- Toggle states
- UI states (modals, dropdowns)
- Component-specific data

### Server State
- API data
- Cached responses
- Optimistic updates
- Real-time data

### Global State
- User authentication
- Theme preferences
- Shopping cart
- Notifications

## React Context

### Theme Context

```
Create a ThemeContext for WebDevHub using React Context:

Features:
- Light/dark mode toggle
- System preference detection
- Persist preference in localStorage
- Smooth transition between themes

Implementation:
- ThemeProvider component
- useTheme hook
- ThemeToggle component

Types:
type Theme = 'light' | 'dark' | 'system';

interface ThemeContextType {
  theme: Theme;
  resolvedTheme: 'light' | 'dark';
  setTheme: (theme: Theme) => void;
}

Use Tailwind CSS dark mode with class strategy.
```

### Auth Context

```
Create an AuthContext for user authentication:

Features:
- Login/logout functions
- User data storage
- Loading state
- Protected route support

Implementation:
- AuthProvider component
- useAuth hook
- ProtectedRoute component

Types:
interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  role: 'admin' | 'member' | 'viewer';
}

interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => Promise<void>;
  register: (data: RegisterInput) => Promise<void>;
}
```

## Zustand

### Project Store

```
Create a Zustand store for project management:

State:
- projects: Project[]
- currentProject: Project | null
- filters: FilterState
- isLoading: boolean
- error: string | null

Actions:
- fetchProjects(): Promise<void>
- fetchProject(id: string): Promise<void>
- createProject(data: CreateProjectInput): Promise<Project>
- updateProject(id: string, data: UpdateProjectInput): Promise<Project>
- deleteProject(id: string): Promise<void>
- setFilters(filters: Partial<FilterState>): void
- clearFilters(): void

Features:
- TypeScript types for all state and actions
- Devtools integration
- Persist filters in localStorage
- Optimistic updates for better UX

Use Zustand with TypeScript.
```

### UI Store

```
Create a Zustand store for UI state:

State:
- sidebarCollapsed: boolean
- activeModal: string | null
- notifications: Notification[]
- theme: 'light' | 'dark'

Actions:
- toggleSidebar(): void
- openModal(id: string): void
- closeModal(): void
- addNotification(notification: Notification): void
- removeNotification(id: string): void
- setTheme(theme: 'light' | 'dark'): void

Features:
- Persist sidebar state
- Auto-dismiss notifications after timeout
- Notification queue management
```

## Server State with React Query

```
Set up React Query for server state management:

Configuration:
- Stale time: 5 minutes
- Cache time: 10 minutes
- Retry: 3 times
- Refetch on window focus

Hooks to create:
- useProjects: Fetch paginated projects
- useProject: Fetch single project
- useCreateProject: Mutation to create project
- useUpdateProject: Mutation to update project
- useDeleteProject: Mutation to delete project

Features:
- Automatic background refetching
- Optimistic updates
- Cache invalidation
- Loading and error states
- Infinite scroll support
```

## State Management Patterns

### Derived State

```
Create derived state patterns:

1. Filtered projects list (derived from projects + filters)
2. Project statistics (derived from projects list)
3. User permissions (derived from user role)
4. Unread notification count (derived from notifications)

Use useMemo for expensive computations.
Keep derived state separate from source state.
```

### State Machines

```
Create a state machine for project status:

States:
- draft → active (on publish)
- active → archived (on archive)
- archived → active (on restore)
- any → deleted (on delete)

Implementation:
- useProjectStateMachine hook
- Valid transitions map
- Action handlers for each transition
- Guard functions for permissions
```

## Prompting for State Management

### Choosing the Right Solution

```
I need state management for WebDevHub. Help me decide:

State types:
1. User authentication (login, logout, user data)
2. Theme preference (light/dark)
3. Project list (fetched from API)
4. Form state (multi-step form)
5. UI state (sidebar, modals, notifications)

For each type, recommend:
- Best solution (Context, Zustand, React Query, local state)
- Why it's the best choice
- Implementation approach
```

### Implementation Prompt

```
Implement state management for the project list feature:

Requirements:
- Fetch projects from /api/projects
- Support pagination, search, and filters
- Cache results for 5 minutes
- Optimistic updates for create/delete
- Loading and error states
- Infinite scroll support

Use Zustand for client state and React Query for server state.
Include TypeScript types for all state and actions.
```

## Practice Exercise

Implement state management for WebDevHub:

1. **Theme Context** — Light/dark mode with persistence
2. **Auth Context** — Login/logout with protected routes
3. **Project Store** — Zustand store for project CRUD

Write prompts for each, implement, then test the state flow.

## What's Next

In the next lesson, we'll build forms with AI — handling validation, submission, and error states.
