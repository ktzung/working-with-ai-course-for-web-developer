# Prompts for Styling

## Styling with AI

Beautiful, responsive design is essential for modern web applications. AI can generate stunning styles when you describe what you want clearly. Let's master styling prompts for Tailwind CSS and modern CSS.

## Tailwind CSS Prompts

### Component Styling

```
Style this React card component using Tailwind CSS:

Requirements:
- Clean, modern card design with subtle shadow
- Rounded corners (lg)
- Hover effect: slight lift with increased shadow
- Responsive padding: smaller on mobile, larger on desktop
- Dark mode support
- Image section at top with aspect-ratio 16:9
- Content area with title, description, and metadata
- Footer with action buttons

Color scheme: Blue primary (#3B82F6), neutral grays
Typography: Clean sans-serif, good hierarchy
```

### Dashboard Layout

```
Create a responsive dashboard layout using Tailwind CSS:

Structure:
- Fixed sidebar (280px on desktop, hidden on mobile with hamburger toggle)
- Top header with search bar and user menu
- Main content area with scroll
- Right panel (optional, hidden on tablet/mobile)

Sidebar:
- Dark background (slate-900)
- Logo at top
- Navigation items with icons
- Active state with highlight
- Collapse to icons only on smaller screens

Header:
- White background with bottom border
- Search input (centered, max-width 600px)
- User avatar and dropdown (right)
- Notification bell with badge

Responsive breakpoints:
- Mobile: < 768px (sidebar hidden, full-width content)
- Tablet: 768px - 1024px (sidebar collapsed, content with right panel hidden)
- Desktop: > 1024px (full layout)
```

### Form Styling

```
Style a registration form using Tailwind CSS:

Fields:
- Full name (text input)
- Email (email input)
- Password (with show/hide toggle)
- Confirm password
- Role selector (dropdown)
- Terms checkbox
- Submit button

Design requirements:
- Centered card layout with max-width 480px
- Clean input styling with focus ring
- Error states with red border and message
- Success states with green checkmark
- Loading state on submit button
- Social login buttons (Google, GitHub)
- Link to login page

Accessibility:
- Proper label associations
- Focus visible states
- Error messages linked to inputs
```

## Animation Prompts

### Micro-interactions

```
Add these micro-interactions using Tailwind CSS and Framer Motion:

1. Button hover: Scale 1.02, shadow increase, 200ms transition
2. Card appear: Fade in + slide up, staggered 100ms between cards
3. Modal open: Backdrop fade + content scale from 0.95
4. Toast notification: Slide in from right, auto-dismiss after 3s
5. Loading skeleton: Pulse animation
6. Tab switch: Underline slide animation
7. Dropdown: Fade + slide down, 150ms

Use Tailwind's transition utilities where possible.
Use Framer Motion for complex animations.
Keep animations subtle and performant.
```

### Page Transitions

```
Create page transitions for a Next.js app:

- Page enter: Fade in + slight upward movement (300ms)
- Page exit: Fade out + slight downward movement (200ms)
- Loading state: Skeleton screens matching page layout
- Route change: Progress bar at top of page

Use Framer Motion's AnimatePresence.
Handle both initial load and navigation transitions.
Ensure transitions don't block interaction.
```

## Responsive Design Prompts

### Mobile-First Approach

```
Convert this desktop design to mobile-first using Tailwind CSS:

Desktop layout:
- 3-column grid for project cards
- Sidebar navigation
- Horizontal header with all controls

Mobile requirements:
- Single column stack
- Bottom navigation bar
- Collapsed header with hamburger menu
- Touch-friendly tap targets (min 44px)
- Swipe gestures for card actions
- Pull-to-refresh on list views

Tablet (768px+):
- 2-column grid
- Collapsible sidebar
- Full header

Use Tailwind responsive prefixes (sm:, md:, lg:, xl:).
```

### Typography Scale

```
Create a responsive typography system using Tailwind CSS:

Headings:
- h1: 2.5rem mobile, 3.5rem desktop
- h2: 2rem mobile, 2.75rem desktop
- h3: 1.5rem mobile, 2rem desktop
- h4: 1.25rem mobile, 1.5rem desktop

Body:
- Large: 1.125rem
- Base: 1rem
- Small: 0.875rem
- Caption: 0.75rem

Line heights:
- Headings: 1.2
- Body: 1.6
- Tight: 1.3

Font weights:
- Headings: 700
- Body: 400
- Emphasis: 600

Use Tailwind's text- utilities with responsive variants.
```

## Theme and Design System Prompts

### Color Palette

```
Generate a color palette for a developer portfolio site:

Primary: Blue (for CTAs, links)
Secondary: Purple (for accents, badges)
Success: Green (for status, confirmations)
Warning: Amber (for alerts, pending states)
Error: Red (for errors, destructive actions)
Neutral: Slate (for text, backgrounds, borders)

For each color, provide:
- 50 through 950 shades
- Recommended usage for each shade
- Dark mode variants
- Accessible contrast ratios

Format as Tailwind CSS config extension.
```

### Design Tokens

```
Create a design token system for WebDevHub:

Colors:
- Brand colors with shades
- Semantic colors (success, warning, error, info)
- Neutral palette for text and backgrounds

Spacing:
- Consistent spacing scale (4px base)
- Component-specific spacing

Typography:
- Font families (heading, body, mono)
- Font sizes with line heights
- Font weights

Shadows:
- sm, md, lg, xl variants
- Colored shadows for emphasis

Border radius:
- sm, md, lg, full variants

Format as Tailwind CSS theme extension.
```

## CSS-Specific Prompts

### Custom CSS Properties

```
Create a CSS custom properties system for theming:

:root {
  /* Generate these variables */
  --color-primary-50 through --color-primary-900
  --color-secondary-50 through --color-secondary-900
  --color-neutral-50 through --color-neutral-900
  --spacing-1 through --spacing-16
  --font-size-xs through --font-size-4xl
  --shadow-sm through --shadow-2xl
  --radius-sm through --radius-full
}

Include dark mode overrides using prefers-color-scheme.
```

### Grid Layouts

```
Create CSS Grid layouts for:

1. Dashboard grid: Auto-fit cards with min 300px width
2. Blog layout: Content + sidebar with sticky sidebar
3. Gallery: Masonry-style image grid
4. Pricing table: 3-column equal height cards

Use CSS Grid with Tailwind utilities.
Ensure layouts work without JavaScript.
```

## Practice Exercise

Style these WebDevHub components:

1. **ProjectCard** — Modern card with image, hover effects, dark mode
2. **Dashboard** — Complete responsive dashboard layout
3. **Login Page** — Clean, centered login form with validation states

Write detailed styling prompts, then implement with your AI tool.

## What's Next

In the next lesson, we'll cover common prompt mistakes that web developers make and how to avoid them.
