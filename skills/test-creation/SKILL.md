# Skill: Test Creation

> Generate comprehensive unit, integration, and end-to-end tests for web applications.

## Purpose

This skill enables AI tools to create well-structured test suites that cover critical functionality, edge cases, and accessibility requirements — using Jest, React Testing Library, and Playwright.

## When to Use

- Writing tests for new components or features
- Improving test coverage for existing code
- Creating E2E tests for user workflows
- Setting up test utilities and mocks
- Generating regression tests from bug reports

## Instructions

### Step 1: Analyze the Target

Before generating tests, identify:
1. **Type** — Component, utility, API route, or E2E flow
2. **Inputs** — What data does it receive?
3. **Outputs** — What does it render/return?
4. **Side Effects** — API calls, state changes, navigation
5. **Edge Cases** — Empty data, errors, loading states

### Step 2: Generate Unit Tests (Components)

```
Write unit tests for this React component:

[paste component code]

Test cases:
1. Renders with minimum required props
2. Renders all visual elements correctly
3. Handles user interactions (click, input, submit)
4. Shows loading state
5. Shows error state with message
6. Shows empty state
7. Calls callback props correctly
8. Applies custom className

Use: Jest + React Testing Library
File: src/components/__tests__/{Name}.test.tsx
```

### Step 3: Generate API Tests

```
Write integration tests for this API route:

[paste route handler code]

Test cases:
1. Returns 200 with valid request
2. Returns 201 on successful creation
3. Returns 400 with invalid body
4. Returns 401 without auth
5. Returns 404 for missing resource
6. Handles database errors gracefully
7. Validates query parameters

Use: Jest + supertest
File: app/api/{resource}/__tests__/route.test.ts
```

### Step 4: Generate E2E Tests

```
Write Playwright E2E tests for this user flow:

Flow: [describe the user journey]

Steps:
1. Navigate to [page]
2. Fill in [form fields]
3. Click [button]
4. Verify [expected result]

Include:
- Happy path
- Error scenarios
- Mobile viewport test
- Accessibility checks (axe-core)

File: e2e/{feature}.spec.ts
```

## Test Utilities

### Mock Setup

```typescript
// API mock
const mockFetch = jest.fn();
global.fetch = mockFetch;

// Router mock
jest.mock('next/navigation', () => ({
  useRouter: () => ({ push: jest.fn() }),
  usePathname: () => '/',
}));

// Session mock
jest.mock('next-auth/react', () => ({
  useSession: () => ({ data: mockSession, status: 'authenticated' }),
}));
```

### Custom Render

```typescript
// test-utils.tsx
function renderWithProviders(ui: ReactElement, options = {}) {
  return render(ui, {
    wrapper: ({ children }) => (
      <QueryClientProvider client={queryClient}>
        {children}
      </QueryClientProvider>
    ),
    ...options,
  });
}
```

## Quality Checklist

- [ ] Tests are isolated (no shared mutable state)
- [ ] Descriptive test names (it should read like documentation)
- [ ] Arrange-Act-Assert pattern
- [ ] Mocks are properly cleaned up
- [ ] No implementation details tested (test behavior, not internals)
- [ ] Edge cases covered (null, empty, boundary values)
- [ ] Async operations properly awaited
- [ ] Tests run fast (<100ms per unit test)

## Coverage Targets

| Area | Target |
|------|--------|
| Components | >80% |
| API Routes | >90% |
| Utilities | >95% |
| E2E Critical Paths | 100% |

## Tools

- **GitHub Copilot Chat** — Best for generating test cases from code
- **Cursor** — Best for bulk test generation across files
- **ChatGPT** — Best for designing test strategies and edge cases
