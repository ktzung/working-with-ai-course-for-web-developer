# Styling with Tailwind CSS

## AI-Powered Styling Workflow

Tailwind CSS and AI are a perfect match. Tailwind's utility classes are descriptive, making it easy for AI to generate beautiful, responsive designs from your descriptions.

## Generating Tailwind Classes

### From Description to Classes

```
I need a hero section for WebDevHub with:
- Full viewport height
- Gradient background (blue to purple)
- Centered content
- Large heading with white text
- Subtitle with lighter text
- Two CTA buttons (primary filled, secondary outline)
- Decorative background pattern

Generate the Tailwind CSS classes for this layout.
```

### AI Response

```tsx
<section className="relative min-h-screen bg-gradient-to-br from-blue-600 to-purple-700 flex items-center justify-center overflow-hidden">
  {/* Background pattern */}
  <div className="absolute inset-0 opacity-10">
    <div className="absolute inset-0 bg-[url('/grid.svg')] bg-center [mask-image:linear-gradient(180deg,white,rgba(255,255,255,0))]" />
  </div>
  
  <div className="relative z-10 text-center px-4 max-w-4xl mx-auto">
    <h1 className="text-5xl md:text-7xl font-bold text-white mb-6 leading-tight">
      Build Amazing
      <span className="block text-transparent bg-clip-text bg-gradient-to-r from-blue-200 to-purple-200">
        Web Projects
      </span>
    </h1>
    
    <p className="text-xl md:text-2xl text-blue-100 mb-10 max-w-2xl mx-auto">
      Showcase your work, manage projects, and collaborate with developers worldwide.
    </p>
    
    <div className="flex flex-col sm:flex-row gap-4 justify-center">
      <button className="px-8 py-4 bg-white text-blue-600 font-semibold rounded-lg hover:bg-blue-50 transition-colors shadow-lg">
        Get Started Free
      </button>
      <button className="px-8 py-4 border-2 border-white text-white font-semibold rounded-lg hover:bg-white/10 transition-colors">
        View Demo
      </button>
    </div>
  </div>
</section>
```

## Responsive Design Prompts

### Mobile-First Layout

```
Create a responsive project grid using Tailwind CSS:

Requirements:
- Mobile (< 640px): 1 column, full width cards
- Tablet (640px - 1024px): 2 columns
- Desktop (> 1024px): 3 columns
- Gap: 4 on mobile, 6 on desktop
- Cards should have equal height

Also include:
- Responsive padding for the container
- Responsive text sizes
- Responsive spacing between sections
```

### Responsive Navigation

```
Create a responsive navigation bar:

Desktop:
- Horizontal layout
- Logo on left
- Nav links centered
- User menu on right
- Height: 16 (h-16)

Mobile:
- Hamburger menu button
- Slide-out sidebar from left
- Overlay behind sidebar
- Close button in sidebar

Use Tailwind responsive prefixes (sm:, md:, lg:).
Use CSS transitions for smooth animations.
```

## Dark Mode Implementation

```
Implement dark mode for WebDevHub using Tailwind CSS:

Requirements:
- Toggle button in header
- Persist preference in localStorage
- Respect system preference (prefers-color-scheme)
- Smooth transition between modes

Color mapping:
- Light background: bg-white, dark: bg-gray-900
- Light text: text-gray-900, dark: text-gray-100
- Light cards: bg-white, dark: bg-gray-800
- Light borders: border-gray-200, dark: border-gray-700

Generate:
1. ThemeProvider component
2. ThemeToggle component
3. Updated tailwind.config.ts with darkMode: 'class'
```

## Component Styling Patterns

### Card Variants

```
Create Tailwind CSS classes for card variants:

Default card:
- White background, subtle shadow, rounded-lg
- Hover: shadow-md, slight lift

Elevated card:
- White background, shadow-md, rounded-xl
- Hover: shadow-lg, translate-y-[-2px]

Flat card:
- Gray-50 background, no shadow, rounded-lg
- Border: border-gray-200

Interactive card:
- Clickable with cursor-pointer
- Hover: border-blue-300, ring-2 ring-blue-100

All variants should support dark mode.
```

### Form Styling

```
Create Tailwind CSS classes for form elements:

Input field:
- Base: border, rounded-md, px-4, py-2
- Focus: ring-2, ring-blue-500, border-blue-500
- Error: border-red-500, ring-red-500
- Disabled: bg-gray-100, cursor-not-allowed

Label:
- Block, text-sm, font-medium, text-gray-700
- Required indicator: text-red-500

Error message:
- text-sm, text-red-600, mt-1

Help text:
- text-sm, text-gray-500, mt-1
```

## Animation with Tailwind

```
Add these animations using Tailwind CSS:

1. Fade in: opacity-0 to opacity-100
2. Slide up: translate-y-4 to translate-y-0
3. Scale in: scale-95 to scale-100
4. Pulse: animate-pulse for loading states
5. Spin: animate-spin for loading spinners
6. Bounce: animate-bounce for attention

Create a utility component for animated entrance:
- Supports delay (100ms, 200ms, 300ms)
- Supports direction (up, down, left, right)
- Triggers on intersection (scroll into view)
```

## Tailwind Configuration

```
Extend tailwind.config.ts for WebDevHub:

Theme extensions:
- Colors: brand (blue), accent (purple), success (green), warning (amber), error (red)
- Fonts: heading (Inter), body (Inter), mono (JetBrains Mono)
- Shadows: card, dropdown, modal
- Animations: fadeIn, slideUp, scaleIn
- BorderRadius: card, button, input

Plugins:
- @tailwindcss/forms (form styling)
- @tailwindcss/typography (prose styling)
- @tailwindcss/aspect-ratio (aspect ratios)
```

## Practice Exercise

Style these WebDevHub pages with Tailwind CSS:

1. **Landing Page** — Hero, features, testimonials, CTA
2. **Dashboard** — Stats cards, project grid, activity feed
3. **Login Page** — Centered form with social login

Write detailed styling prompts, generate the code, then refine.

## What's Next

In the next lesson, we'll dive deeper into responsive design — creating layouts that work beautifully on every device.
