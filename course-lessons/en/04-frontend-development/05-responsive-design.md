# Responsive Design

## Mobile-First with AI

Responsive design ensures your application works beautifully on every device. AI can help you implement mobile-first designs that adapt gracefully to different screen sizes.

## The Mobile-First Approach

Mobile-first means designing for the smallest screen first, then adding complexity for larger screens. This approach:
- Forces you to prioritize content
- Results in faster mobile experiences
- Is easier to scale up than to scale down

## AI Prompts for Responsive Design

### Responsive Dashboard Layout

```
Create a responsive dashboard layout for WebDevHub:

Mobile (< 768px):
- Single column, full width
- Bottom tab navigation (Home, Projects, Snippets, Profile)
- Collapsible header with hamburger menu
- Cards stacked vertically
- Pull to refresh on lists

Tablet (768px - 1024px):
- Two-column layout
- Sidebar navigation (collapsible)
- Header with search bar
- Cards in 2-column grid

Desktop (> 1024px):
- Three-column layout (sidebar + content + optional panel)
- Fixed sidebar navigation
- Header with full search and user menu
- Cards in 3-column grid

Use Tailwind responsive prefixes.
Ensure touch targets are at least 44px on mobile.
```

### Responsive Data Table

```
Create a responsive data table for project list:

Desktop:
- Full table with all columns visible
- Sortable headers
- Row hover effects
- Pagination at bottom

Tablet:
- Hide less important columns (created date, last updated)
- Keep essential columns (name, status, tech stack)
- Horizontal scroll if needed

Mobile:
- Card view instead of table
- Each card shows: name, status, tech stack
- Expandable for full details
- Swipe actions (edit, delete)

Use CSS Grid or Flexbox for layout.
Use Tailwind responsive classes.
```

## Responsive Typography

```
Create a responsive typography system:

Headings:
- h1: text-2xl sm:text-3xl md:text-4xl lg:text-5xl
- h2: text-xl sm:text-2xl md:text-3xl lg:text-4xl
- h3: text-lg sm:text-xl md:text-2xl lg:text-3xl
- h4: text-base sm:text-lg md:text-xl lg:text-2xl

Body:
- Large: text-base sm:text-lg
- Base: text-sm sm:text-base
- Small: text-xs sm:text-sm

Line heights:
- Headings: leading-tight
- Body: leading-relaxed
- Compact: leading-snug

Use Tailwind responsive text utilities.
```

## Responsive Images

```
Implement responsive images for project thumbnails:

Requirements:
- Different sizes for different screens
- Lazy loading for below-fold images
- Placeholder blur while loading
- Aspect ratio preservation (16:9 for thumbnails)
- WebP format with fallback

Generate:
1. ResponsiveImage component
2. Image optimization utilities
3. Placeholder generation

Use Next.js Image component with responsive sizes.
```

## Responsive Forms

```
Create a responsive form layout:

Mobile:
- Full width inputs
- Stacked vertically
- Large touch targets (min-height: 44px)
- Full width buttons
- Floating labels

Tablet:
- Two-column layout for related fields
- Side-by-side buttons
- Inline validation messages

Desktop:
- Multi-column layout
- Compact spacing
- Inline labels
- Tooltip help text

Use Tailwind responsive classes.
Ensure form is usable with keyboard only.
```

## Responsive Navigation Patterns

### Bottom Tab Bar (Mobile)

```
Create a bottom tab bar for mobile navigation:

Tabs:
- Home (icon + label)
- Projects (icon + label + badge count)
- Snippets (icon + label)
- Profile (icon + label)

Features:
- Fixed at bottom
- Active tab highlighted
- Badge for notifications
- Safe area padding for notched devices
- Hide on scroll down, show on scroll up

Use Tailwind CSS with fixed positioning.
```

### Sidebar Navigation (Desktop)

```
Create a collapsible sidebar for desktop:

Features:
- Fixed position, full height
- Logo at top
- Navigation items with icons
- Active state with highlight
- Collapse to icon-only mode
- Smooth transition animation
- Remember collapsed state in localStorage

Use Tailwind CSS with transitions.
```

## Responsive Breakpoints Strategy

```
Define a breakpoint strategy for WebDevHub:

Breakpoints:
- sm: 640px (large phones)
- md: 768px (tablets)
- lg: 1024px (small desktops)
- xl: 1280px (large desktops)
- 2xl: 1536px (extra large)

Usage guidelines:
- sm: Adjust phone layouts
- md: Tablet-specific layouts
- lg: Desktop layouts
- xl: Wider content areas
- 2xl: Maximum content width

Document when to use each breakpoint.
```

## Testing Responsive Design

```
Create a responsive testing checklist:

Visual checks:
- [ ] All content readable on mobile
- [ ] No horizontal scroll on mobile
- [ ] Touch targets at least 44px
- [ ] Images scale properly
- [ ] Text doesn't overflow containers

Interaction checks:
- [ ] Navigation works on all sizes
- [ ] Forms usable on mobile
- [ ] Modals fit on small screens
- [ ] Dropdowns don't overflow viewport

Performance checks:
- [ ] Images lazy loaded
- [ ] No unnecessary re-renders
- [ ] Smooth scrolling
- [ ] Fast tap response
```

## Practice Exercise

Make these WebDevHub components responsive:

1. **Project Grid** — 1 col mobile, 2 tablet, 3 desktop
2. **Navigation** — Bottom tabs mobile, sidebar desktop
3. **Form** — Stacked mobile, side-by-side desktop

Write responsive prompts, implement, then test on different screen sizes.

## What's Next

In the next lesson, we'll tackle state management — using AI to implement React Context, Zustand, and other state solutions.
