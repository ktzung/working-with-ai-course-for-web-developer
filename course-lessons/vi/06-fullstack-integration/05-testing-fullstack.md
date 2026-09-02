# Kiểm thử ứng dụng Full-Stack với AI

## Mục tiêu học tập
- Viết kiểm thử tích hợp cho endpoint API
- Tạo kiểm thử端到端 với Playwright
- Sử dụng AI để tạo bộ kiểm thử

## Tại sao kiểm thử quan trọng?

Lỗi phát hiện trong开发 tốn ít hơn 10 lần so với lỗi发现 trong production. Kiểm thử đảm bảo ứng dụng hoạt động đúng trước khi người dùng gặp sự cố.

## Kim tự tháp kiểm thử

1. **Kiểm thử đơn vị**: Kiểm tra hàm riêng lẻ (nhanh, nhiều)
2. **Kiểm thử tích hợp**: Kiểm tra endpoint API và cơ sở dữ liệu (tốc độ trung bình)
3. **Kiểm thử E2E**: Kiểm tra luồng người dùng hoàn chỉnh (chậm, ít)

## Kiểm thử tích hợp API

Kiểm tra API Express với Supertest:

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
    it('nên tạo người dùng mới', async () => {
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

    it('nên trả về 400 cho email trùng lặp', async () => {
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
    it('nên yêu cầu xác thực', async () => {
      const res = await request(app).get('/api/users');
      expect(res.status).toBe(401);
    });

    it('nên trả về người dùng phân trang', async () => {
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

## Kiểm thử E2E với Playwright

```javascript
// tests/e2e/auth.spec.js
const { test, expect } = require('@playwright/test');

test.describe('Xác thực', () => {
  test('nên đăng nhập thành công', async ({ page }) => {
    await page.goto('/login');

    await page.fill('[data-testid="email"]', 'user@example.com');
    await page.fill('[data-testid="password"]', 'password123');
    await page.click('[data-testid="login-button"]');

    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('[data-testid="user-menu"]')).toBeVisible();
  });

  test('nên显示 lỗi cho thông tin xác thực không hợp lệ', async ({ page }) => {
    await page.goto('/login');

    await page.fill('[data-testid="email"]', 'wrong@example.com');
    await page.fill('[data-testid="password"]', 'wrongpassword');
    await page.click('[data-testid="login-button"]');

    await expect(page.locator('[data-testid="error-message"]'))
      .toContainText('Thông tin xác thực không hợp lệ');
  });

  test('nên bảo vệ route dashboard', async ({ page }) => {
    await page.goto('/dashboard');
    await expect(page).toHaveURL('/login');
  });
});
```

## Kiểm thử thành phần

```javascript
// tests/components/TaskList.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import TaskList from '../../components/TaskList';
import { server } from '../mocks/server';
import { rest } from 'msw';

test('render danh sách nhiệm vụ', async () => {
  render(<TaskList />);

  await waitFor(() => {
    expect(screen.getByText('Nhiệm vụ 1')).toBeInTheDocument();
    expect(screen.getByText('Nhiệm vụ 2')).toBeInTheDocument();
  });
});

test('xử lý trạng thái trống', async () => {
  server.use(
    rest.get('/api/tasks', (req, res, ctx) => {
      return res(ctx.json({ success: true, data: [] }));
    })
  );

  render(<TaskList />);

  await waitFor(() => {
    expect(screen.getByText('Không tìm thấy nhiệm vụ')).toBeInTheDocument();
  });
});
```

## Prompt AI cho tạo kiểm thử

```
Tạo bộ kiểm thử toàn diện cho API quản lý nhiệm vụ:
1. Kiểm thử đơn vị cho hàm tiện ích và trình xác thực
2. Kiểm thử tích hợp cho tất cả endpoint CRUD
3. Kiểm thử xác thực và授权
4. Kiểm thử xử lý lỗi (xác thực, không tìm thấy,未经授权)
5. Kiểm thử phân trang và lọc
6. Kiểm thử tải tệp lên
7. Kiểm thử E2E cho luồng người dùng quan trọng

Bao gồm nhà máy dữ liệu kiểm thử và trợ giúp thiết lập/dỡ bỏ.
```

## Giả lập với MSW

```javascript
// tests/mocks/handlers.js
import { rest } from 'msw';

export const handlers = [
  rest.get('/api/tasks', (req, res, ctx) => {
    return res(
      ctx.json({
        success: true,
        data: [
          { id: '1', title: 'Nhiệm vụ 1', status: 'TODO' },
          { id: '2', title: 'Nhiệm vụ 2', status: 'DONE' }
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

## Bài tập thực hành

Thêm kiểm thử vào ứng dụng quản lý nhiệm vụ:
- Kiểm thử đơn vị cho schema xác thực
- Kiểm thử tích hợp cho tất cả endpoint API
- Kiểm thử E2E cho đăng nhập, tạo nhiệm vụ và quản lý nhiệm vụ
- Kiểm thử thành phần cho thành phần React
- Đạt 80%覆盖率 mã

## Điểm chính

- Kim tự tháp kiểm thử: nhiều kiểm thử đơn vị, một số tích hợp, ít E2E
- Supertest kiểm tra endpoint API mà không cần启动 server
- Playwright kiểm tra tương tác trình duyệt thực tế
- AI tạo bộ kiểm thử toàn diện từ mã của bạn
