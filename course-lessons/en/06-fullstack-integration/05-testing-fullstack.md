# Testing Full-Stack Applications with AI

## Learning Objectives
- Write integration tests for API endpoints
- Create end-to-end tests with Playwright
- Use AI to generate test suites

## Why Testing Matters

Bugs caught in development cost 10x less than bugs found in production. Testing ensures your application works correctly before users encounter issues.

## Testing Pyramid

1. **Unit tests**: Test individual functions (fast, many)
2. **Integration tests**: Test API endpoints and database (medium speed)
3. **E2E tests**: Test complete user flows (slow, few)

## API Integration Tests

Test your Express API with Supertest:

```javascript
// tests/api/users.test.js
const request = require('supertest');
const app = require('../../app');
const User = require('../../models/User');

describe('Users API', () => {
  beforeEach(async () => {
    await User.deleteMany({});
  });

  describe('POST /api/users/register', () => {
    it('should create a new user', async () => {
      const res = await request(app)
        .post('/api/users/register')
        .send({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test User'
        });

      expect(res.status).toBe(201);
      expect(res.body.success).toBe(true);
      expect(res.body.data.email).toBe('test@example.com');
      expect(res.body.data.password).toBeUndefined();
    });

    it('should return 400 for duplicate email', async () => {
      await User.create({
        email: 'test@example.com',
        password: 'hashed',
        name: 'Existing'
      });

      const res = await request(app)
        .post('/api/users/register')
        .send({
          email: 'test@example.com',
          password: 'password123',
          name: 'Test User'
        });

      expect(res.status).toBe(400);
      expect(res.body.error.code).toBe('DUPLICATE_VALUE');
    });
  });

  describe('GET /api/users', () => {
    it('should require authentication', async () => {
      const res = await request(app).get('/api/users');
      expect(res.status).toBe(401);
    });

    it('should return paginated users', async () => {
      const token = await getAuthToken();

      const res = await request(app)
        .get('/api/users?page=1&limit=10')
        .set('Authorization', `Bearer ${token}`);

      expect(res.status).toBe(200);
      expect(res.body.data).toBeInstanceOf(Array);
      expect(res.body.pagination).toBeDefined();
    });
  });
});
```

## E2E Tests with Playwright

```javascript
// tests/e2e/auth.spec.js
const { test, expect } = require('@playwright/test');

test.describe('Authentication', () => {
  test('should login successfully', async ({ page }) => {
    await page.goto('/login');

    await page.fill('[data-testid="email"]', 'user@example.com');
    await page.fill('[data-testid="password"]', 'password123');
    await page.click('[data-testid="login-button"]');

    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('[data-testid="user-menu"]')).toBeVisible();
  });

  test('should show error for invalid credentials', async ({ page }) => {
    await page.goto('/login');

    await page.fill('[data-testid="email"]', 'wrong@example.com');
    await page.fill('[data-testid="password"]', 'wrongpassword');
    await page.click('[data-testid="login-button"]');

    await expect(page.locator('[data-testid="error-message"]'))
      .toContainText('Invalid credentials');
  });

  test('should protect dashboard route', async ({ page }) => {
    await page.goto('/dashboard');
    await expect(page).toHaveURL('/login');
  });
});
```

## Component Testing

```javascript
// tests/components/TaskList.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import TaskList from '../../components/TaskList';
import { server } from '../mocks/server';
import { rest } from 'msw';

test('renders task list', async () => {
  render(<TaskList />);

  await waitFor(() => {
    expect(screen.getByText('Task 1')).toBeInTheDocument();
    expect(screen.getByText('Task 2')).toBeInTheDocument();
  });
});

test('handles empty state', async () => {
  server.use(
    rest.get('/api/tasks', (req, res, ctx) => {
      return res(ctx.json({ success: true, data: [] }));
    })
  );

  render(<TaskList />);

  await waitFor(() => {
    expect(screen.getByText('No tasks found')).toBeInTheDocument();
  });
});
```

## AI Prompt for Test Generation

```
Generate a comprehensive test suite for a task management API:
1. Unit tests for utility functions and validators
2. Integration tests for all CRUD endpoints
3. Authentication and authorization tests
4. Error handling tests (validation, not found, unauthorized)
5. Pagination and filtering tests
6. File upload tests
7. E2E tests for critical user flows

Include test data factories and setup/teardown helpers.
```

## Mocking with MSW

```javascript
// tests/mocks/handlers.js
import { rest } from 'msw';

export const handlers = [
  rest.get('/api/tasks', (req, res, ctx) => {
    return res(
      ctx.json({
        success: true,
        data: [
          { id: '1', title: 'Task 1', status: 'TODO' },
          { id: '2', title: 'Task 2', status: 'DONE' }
        ]
      })
    );
  }),

  rest.post('/api/tasks', (req, res, ctx) => {
    return res(
      ctx.status(201),
      ctx.json({
        success: true,
        data: { id: '3', ...req.body }
      })
    );
  })
];
```

## Practice Exercise

Add tests to your Task Management application:
- Unit tests for validation schemas
- Integration tests for all API endpoints
- E2E tests for login, task creation, and task management
- Component tests for React components
- Achieve 80% code coverage

## Key Takeaways

- Testing pyramid: many unit tests, some integration, few E2E
- Supertest tests API endpoints without starting the server
- Playwright tests real browser interactions
- AI generates comprehensive test suites from your code
