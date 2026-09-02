# Building React Components with AI

## From Prompt to Production Component

Let's build real React components for WebDevHub using AI. We'll go from simple UI elements to complex interactive features.

## Simple UI Components

### Button Component

```
Create a Button component for WebDevHub using React 18, TypeScript, and Tailwind CSS.

Variants:
- primary: Blue background, white text
- secondary: Gray background, dark text
- outline: Transparent with border
- ghost: Transparent, no border
- danger: Red background, white text

Sizes: sm, md, lg

States:
- Default
- Hover (slight darken)
- Active (pressed effect)
- Disabled (reduced opacity, no pointer)
- Loading (spinner replaces icon/text)

Props:
- variant: ButtonVariant
- size: ButtonSize
- isLoading?: boolean
- isDisabled?: boolean
- leftIcon?: ReactNode
- rightIcon?: ReactNode
- children: ReactNode
- onClick?: () => void
- type?: 'button' | 'submit' | 'reset'
- className?: string

Use forwardRef for ref forwarding.
Include proper aria attributes.
Export as named export from components/ui/Button.tsx.
```

### Card Component

```
Create a Card component using React 18, TypeScript, and Tailwind CSS.

Sub-components:
- Card (wrapper)
- Card.Header (top section with title and actions)
- Card.Body (main content)
- Card.Footer (bottom section with actions)

Features:
- Hover effect (slight shadow increase)
- Click handler (optional)
- Padding variants (sm, md, lg)
- Border options (none, light, full)
- Dark mode support

Props for Card:
- padding?: 'sm' | 'md' | 'lg'
- border?: 'none' | 'light' | 'full'
- isClickable?: boolean
- onClick?: () => void
- className?: string
- children: ReactNode

Use compound component pattern with dot notation.
Export from components/ui/Card.tsx.
```

## Complex Interactive Components

### Search Input with Autocomplete

```
Create a SearchInput component with autocomplete suggestions.

Props:
- placeholder?: string
- onSearch: (query: string) => void
- onSelect: (item: SearchResult) => void
- suggestions: SearchResult[]
- isLoading?: boolean
- debounceMs?: number (default: 300)

Features:
- Debounced input (configurable delay)
- Dropdown suggestions list
- Keyboard navigation (up/down arrows, enter to select, escape to close)
- Loading spinner during search
- Clear button
- Recent searches (stored in localStorage)
- Highlight matching text in suggestions

SearchResult type:
interface SearchResult {
  id: string;
  title: string;
  description?: string;
  icon?: ReactNode;
}

Accessibility:
- aria-expanded for dropdown
- aria-activedescendant for keyboard navigation
- role="listbox" for suggestions
- role="option" for each suggestion

Styling: Tailwind CSS with dark mode
File: components/ui/SearchInput.tsx
```

### Modal with Form

```
Create a Modal component that can contain forms.

Props:
- isOpen: boolean
- onClose: () => void
- title: string
- size?: 'sm' | 'md' | 'lg' | 'xl'
- children: ReactNode
- footer?: ReactNode

Features:
- Backdrop click to close
- Escape key to close
- Focus trap (tab cycles within modal)
- Body scroll lock when open
- Animated entrance/exit (fade + scale)
- Close button in header
- Responsive (full screen on mobile)

Form integration:
- Auto-focus first input when opened
- Prevent close on backdrop click when form is dirty
- Show confirmation dialog if form has unsaved changes

Accessibility:
- role="dialog"
- aria-modal="true"
- aria-labelledby for title
- Return focus to trigger element on close

Use React Portal for rendering.
File: components/ui/Modal.tsx
```

## Data Display Components

### Data Table

```
Create a DataTable component for displaying project data.

Props:
- data: T[]
- columns: Column<T>[]
- isLoading?: boolean
- onRowClick?: (row: T) => void
- sortable?: boolean
- selectable?: boolean
- onSelectionChange?: (selected: T[]) => void

Column type:
interface Column<T> {
  key: keyof T;
  header: string;
  render?: (value: T[keyof T], row: T) => ReactNode;
  sortable?: boolean;
  width?: string;
}

Features:
- Sortable columns (click header to sort)
- Row selection (checkboxes)
- Loading skeleton
- Empty state
- Responsive (horizontal scroll on mobile)
- Sticky header

Styling: Tailwind CSS
File: components/ui/DataTable.tsx
```

## Prompting Tips for React Components

### Specify the Component API
```
// ❌ Vague
Make a dropdown component

// ✅ Specific
Create a Dropdown component with:
- trigger: ReactNode (the button that opens dropdown)
- items: Array<{ label: string, onClick: () => void, icon?: ReactNode }>
- placement: 'bottom-start' | 'bottom-end' | 'top-start' | 'top-end'
- closeOnSelect: boolean (default: true)
```

### Include TypeScript Types
```
Include these TypeScript types:

interface ProjectCardProps {
  project: Project;
  variant: 'default' | 'compact' | 'featured';
  onSelect?: (id: string) => void;
  onEdit?: (id: string) => void;
  onDelete?: (id: string) => void;
}

interface Project {
  id: string;
  name: string;
  description: string;
  techStack: string[];
  status: 'active' | 'archived' | 'draft';
  stars: number;
  updatedAt: Date;
}
```

### Describe Interactions
```
When the user clicks the star button:
1. Optimistically update the star count (+1)
2. Call POST /api/projects/:id/star
3. If the API fails, revert the count and show error toast
4. If the user clicks again, unstar (POST /api/projects/:id/unstar)
```

## Practice Exercise

Build these components for WebDevHub:

1. **ProjectCard** — Card with image, title, tech stack badges, star count
2. **FilterBar** — Search + filters with active filter pills
3. **ActivityFeed** — List of recent activities with timestamps

Write detailed prompts, generate the code, then review and refine.

## What's Next

In the next lesson, we'll build Vue components with AI — showing how the same principles apply across frameworks.
