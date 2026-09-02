# Component Template — [ComponentName]

> Use this template to plan and document React components. Share with AI to generate implementation code.

## Overview

**Component:** `[ComponentName]`
**Path:** `src/components/[ComponentName].tsx`
**Type:** Page / Layout / UI / Form / Widget
**Description:** [What this component does]

## Props Interface

```typescript
interface [ComponentName]Props {
  title: string;
  items: Item[];
  onSelect: (id: string) => void;
  isLoading?: boolean;
  variant?: 'default' | 'compact' | 'expanded';
  className?: string;
}
```

## State

```typescript
// Local state
const [isOpen, setIsOpen] = useState(false);
const [filter, setFilter] = useState('');

// Derived state
const filteredItems = items.filter(item => 
  item.name.toLowerCase().includes(filter.toLowerCase())
);
```

## Component Structure

```
<ComponentName>
├── <Header>
│   ├── <Title />
│   └── <Actions />
├── <Content>
│   ├── <FilterBar />
│   └── <ItemList>
│       └── <ItemCard /> (repeated)
│           ├── <ItemTitle />
│           ├── <ItemMeta />
│           └── <ItemActions />
│       </ItemCard>
│   </ItemList>
</Content>
└── <Footer>
    └── <Pagination />
</Footer>
```

## Styling

```tsx
// Tailwind classes
<div className="rounded-lg border border-gray-200 bg-white shadow-sm">
  <div className="flex items-center justify-between p-4 border-b">
    <h2 className="text-lg font-semibold text-gray-900">{title}</h2>
  </div>
  <div className="p-4">
    {/* Content */}
  </div>
</div>
```

**Responsive breakpoints:**
- Mobile: Single column, stacked layout
- Tablet: Two-column grid
- Desktop: Full layout with sidebar

## Accessibility

- [ ] Semantic HTML elements (`<nav>`, `<main>`, `<article>`)
- [ ] ARIA labels on interactive elements
- [ ] Keyboard navigation (Tab, Enter, Escape)
- [ ] Focus management for modals/dropdowns
- [ ] Color contrast ratio ≥ 4.5:1
- [ ] Screen reader announcements for dynamic content

## AI Prompt for Generation

```
Create a React component called [ComponentName] with TypeScript.

Props: [paste interface]
Requirements:
- [Functional requirement 1]
- [Functional requirement 2]
- Tailwind CSS for styling
- Responsive design (mobile-first)
- Accessible (ARIA labels, keyboard nav)
- Error and loading states

Use Next.js App Router conventions.
```

## Tests

```typescript
describe('[ComponentName]', () => {
  it('renders with required props', () => {});
  it('handles item selection', () => {});
  it('shows loading state', () => {});
  it('displays empty state', () => {});
  it('filters items correctly', () => {});
  it('is accessible', () => {});
});
```

## Dependencies

- `react`, `react-dom`
- [Other components this depends on]
- [Hooks used: `useQuery`, `useForm`, etc.]

## Notes

- [Edge cases to handle]
- [Performance considerations]
- [Future enhancements]
