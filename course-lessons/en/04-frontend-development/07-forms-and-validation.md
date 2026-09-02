# Forms and Validation with AI

## Building Robust Forms

Forms are everywhere in web applications. AI can help you build forms that are accessible, validated, and provide great user experience.

## React Hook Form + Zod

### Registration Form

```
Create a registration form using React Hook Form and Zod:

Fields:
- name: string (required, min 2 chars)
- email: string (required, valid email format)
- password: string (required, min 8 chars, must include uppercase, lowercase, number)
- confirmPassword: string (must match password)
- role: 'developer' | 'designer' | 'manager' (required)
- acceptTerms: boolean (must be true)

Features:
- Real-time validation on blur
- Error messages below each field
- Password strength indicator
- Show/hide password toggle
- Submit button disabled while submitting
- Loading spinner on submit
- Server error display
- Success redirect

Zod schema:
const registerSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email address'),
  password: z.string()
    .min(8, 'Password must be at least 8 characters')
    .regex(/[A-Z]/, 'Must include uppercase letter')
    .regex(/[a-z]/, 'Must include lowercase letter')
    .regex(/[0-9]/, 'Must include number'),
  confirmPassword: z.string(),
  role: z.enum(['developer', 'designer', 'manager']),
  acceptTerms: z.literal(true, {
    errorMap: () => ({ message: 'You must accept the terms' }),
  }),
}).refine((data) => data.password === data.confirmPassword, {
  message: 'Passwords don\'t match',
  path: ['confirmPassword'],
});

Use @hookform/resolvers/zod for integration.
File: components/auth/RegisterForm.tsx
```

### Multi-Step Form

```
Create a multi-step form for project creation:

Step 1: Basic Info
- name: string (required)
- description: string (optional, max 500 chars)
- visibility: 'public' | 'private'

Step 2: Technical Details
- techStack: string[] (at least one)
- repository: string (valid URL, optional)
- deploymentUrl: string (valid URL, optional)

Step 3: Team Setup
- inviteEmails: string[] (valid emails)
- defaultRole: 'member' | 'viewer'

Features:
- Step indicator showing progress
- Back/Next navigation
- Validation per step (can't proceed if invalid)
- Data persists when navigating between steps
- Summary review before submit
- Submit all data at once

Use React Hook Form with Zod.
Use a stepper component for navigation.
File: components/projects/CreateProjectForm.tsx
```

## Form Patterns

### Dynamic Form Fields

```
Create a form with dynamic fields for tech stack:

Features:
- Add/remove tech stack items
- Autocomplete suggestions from predefined list
- Max 20 items
- Drag to reorder
- Each item has: name, proficiency level (1-5), years of experience

Implementation:
- useFieldArray from React Hook Form
- Custom autocomplete component
- Drag and drop with @dnd-kit/core
- Validation for each field

File: components/projects/TechStackForm.tsx
```

### File Upload Form

```
Create a file upload form for project screenshots:

Features:
- Drag and drop zone
- Click to browse files
- Preview before upload
- Multiple file support (max 5)
- File type validation (jpg, png, webp)
- File size validation (max 5MB each)
- Upload progress indicator
- Remove uploaded files

Implementation:
- React Hook Form for form state
- Custom useFileUpload hook
- Preview with FileReader API
- Progress with XMLHttpRequest

File: components/projects/ScreenshotUpload.tsx
```

## Validation Strategies

### Client-Side Validation

```
Implement client-side validation for all forms:

Rules:
- Required fields: Show error on blur if empty
- Email: Validate format on blur
- Password: Real-time strength indicator
- Confirm password: Validate on change of either field
- Min/max length: Show character count
- Custom rules: Validate on blur

Error display:
- Red border on invalid fields
- Error message below field
- Icon indicator (checkmark or X)
- Screen reader announcement

Use Zod schemas for all validation.
```

### Server-Side Validation

```
Handle server-side validation errors:

When the server returns validation errors:
1. Parse the error response
2. Map field errors to form fields
3. Display errors next to relevant fields
4. Focus the first invalid field
5. Show general error message if no field-specific errors

Error format from server:
{
  error: {
    code: 'VALIDATION_ERROR',
    message: 'Validation failed',
    details: [
      { field: 'email', message: 'Email already exists' },
      { field: 'name', message: 'Name is required' }
    ]
  }
}

Use setError from React Hook Form to set server errors.
```

## Accessibility for Forms

```
Ensure all forms are accessible:

Requirements:
- All inputs have associated labels
- Error messages linked with aria-describedby
- Required fields marked with aria-required
- Invalid fields marked with aria-invalid
- Focus management after errors
- Keyboard navigation works
- Screen reader announcements for success/error

Implementation:
- Use htmlFor on labels
- Use aria-describedby for error messages
- Use role="alert" for error messages
- Manage focus with useRef
```

## Form Components

### Input Component

```
Create a reusable Input component:

Props:
- label: string
- name: string
- type?: string
- placeholder?: string
- error?: string
- helpText?: string
- isRequired?: boolean
- isDisabled?: boolean
- leftIcon?: ReactNode
- rightIcon?: ReactNode

Features:
- Label with required indicator
- Error state with red border and message
- Help text below input
- Icon support
- Focus ring
- Disabled state

Integrate with React Hook Form using Controller.
File: components/ui/Input.tsx
```

### Select Component

```
Create a reusable Select component:

Props:
- label: string
- name: string
- options: Array<{ value: string; label: string; disabled?: boolean }>
- placeholder?: string
- error?: string
- isRequired?: boolean
- isMulti?: boolean

Features:
- Custom styled dropdown
- Search/filter options
- Multi-select with tags
- Keyboard navigation
- Disabled options
- Clear button

Use @headlessui/listbox for accessibility.
File: components/ui/Select.tsx
```

## Practice Exercise

Build these forms for WebDevHub:

1. **Login Form** — Email/password with remember me
2. **Project Edit Form** — Multi-section form with validation
3. **Profile Form** — Avatar upload, bio, social links

Write detailed prompts, implement with React Hook Form + Zod, then test.

## What's Next

In the next lesson, we'll put everything together by building a complete UI page for WebDevHub.
