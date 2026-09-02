# Tạo project-context.md

## Kinh thánh AI cho dự án

File `project-context.md` là tài liệu quan trọng nhất cho phát triển hỗ trợ AI. Nó cho AI biết mọi thứ cần biết về dự án. Hãy tạo một file cho WebDevHub.

## Cấu trúc

Một `project-context.md` tốt có các phần sau:

```markdown
# Context dự án: WebDevHub

## Tổng quan
WebDevHub là nền tảng quản lý dự án và portfolio cho lập trình viên web.
Cho phép lập trình viên showcase dự án, quản lý task và chia sẻ code snippet.

## Tech Stack

### Frontend
- **Framework**: Next.js 14.2.x với App Router
- **Ngôn ngữ**: TypeScript 5.x (strict mode)
- **Styling**: Tailwind CSS 3.4.x
- **Quản lý state**: Zustand 4.x
- **Form**: React Hook Form 7.x + Zod 3.x
- **Icon**: Lucide React
- **Biểu đồ**: Recharts

### Backend
- **API**: Next.js API Routes (App Router)
- **Database**: PostgreSQL 16
- **ORM**: Prisma 5.x
- **Auth**: NextAuth.js 4.x (nhà cung cấp GitHub, Google)
- **Validation**: Zod 3.x

### DevOps
- **Hosting**: Vercel
- **CI/CD**: GitHub Actions
- **Container hóa**: Docker (dev local)

## Kiến trúc

### Cấu trúc App Router
```
src/app/
├── (auth)/          # Trang xác thực (login, register)
├── (dashboard)/     # Trang dashboard bảo vệ
├── api/             # API routes
├── layout.tsx       # Root layout
└── page.tsx         # Trang đích
```

### Tổ chức Component
```
src/components/
├── ui/              # UI component cơ bản (Button, Card, Input)
├── layout/          # Component layout (Header, Sidebar, Footer)
├── features/        # Component theo feature
└── shared/          # Component tiện ích chung
```

### Luồng dữ liệu
1. Component gọi hook (useProjects, useTasks)
2. Hook gọi service function (projectService.getProjects)
3. Service gửi request API đến /api/*
4. API route validate với Zod, query với Prisma
5. Response theo định dạng chuẩn: { data, error, pagination }

## Quy ước code

### Đặt tên file
- Component: PascalCase (`ProjectCard.tsx`)
- Hook: camelCase với tiền tố `use` (`useProjects.ts`)
- Utils: camelCase (`formatDate.ts`)
- Types: PascalCase (`Project.ts`)
- API routes: kebab-case (`route.ts`)

### Pattern Component
- Chỉ dùng functional component (không class component)
- Named export (không default export)
- Props interface đặt tên `{TênComponent}Props`
- Hooks ở đầu component
- JSX return ở cuối

### TypeScript
- Bật strict mode
- Không dùng kiểu `any` (dùng `unknown` nếu cần)
- Interface cho cấu trúc object, type cho union
- Kiểu trả về tường minh cho hàm

### Styling
- Class tiện ích Tailwind CSS
- Thiết kế responsive mobile-first
- Hỗ trợ dark mode qua tiền tố `dark:`
- Màu tùy chỉnh trong tailwind.config.ts

## Schema Database

### Model chính
- **User**: id, name, email, avatar, role, createdAt
- **Project**: id, name, description, techStack[], status, ownerId
- **Task**: id, title, description, status, priority, projectId, assigneeId
- **Snippet**: id, title, code, language, tags[], userId

### Quan hệ
- User có nhiều Project (1:N)
- Project có nhiều Task (1:N)
- User có nhiều Snippet (1:N)
- Project có nhiều Member (M:N qua ProjectMember)

## Quy ước API

### Định dạng response
```typescript
// Thành công
{ data: T }
{ data: T[], pagination: { page, limit, total, totalPages } }

// Lỗi
{ error: { code: string, message: string, details?: any } }
```

### HTTP Methods
- GET: Đọc (danh sách hoặc đơn)
- POST: Tạo
- PUT: Cập nhật đầy đủ
- PATCH: Cập nhật từng phần
- DELETE: Xóa (ưu tiên xóa mềm)

### Status Codes
- 200: Thành công
- 201: Đã tạo
- 204: Đã xóa
- 400: Lỗi validation
- 401: Chưa xác thực
- 403: Không có quyền
- 404: Không tìm thấy
- 500: Lỗi server

## Tiêu chuẩn test

- Unit test cho hàm tiện ích
- Test component với React Testing Library
- Test API với supertest
- Coverage tối thiểu 80%
- Đặt tên file test: `{tên}.test.ts` hoặc `{tên}.spec.ts`

## Mục tiêu Sprint hiện tại

- [ ] Hoàn thành luồng xác thực người dùng
- [ ] Xây thao tác CRUD dự án
- [ ] Triển khai bảng Kanban
- [ ] Thêm trình quản lý code snippet

## Vấn đề đã biết

- Chuyển dark mode cần lưu trạng thái
- Animation sidebar mobile bị giật
- Tìm kiếm phân biệt hoa thường (nên không phân biệt)
```

## Cách sử dụng file này

### Với GitHub Copilot
Lưu dưới dạng `.github/copilot-instructions.md` ở gốc dự án.

### Với ChatGPT/Claude
Upload khi bắt đầu mỗi cuộc hội thoại hoặc paste phần liên quan.

### Với Cursor
Lưu dưới dạng `.cursorrules` hoặc tham chiếu trong prompt.

## Cập nhật thường xuyên

Cập nhật `project-context.md` khi:
- Thêm công nghệ mới
- Thay đổi quyết định kiến trúc
- Cập nhật quy ước code
- Hoàn thành mục tiêu sprint
- Phát hiện vấn đề mới

## Bài tập thực hành

Tạo `project-context.md` cho dự án của bạn. Bao gồm:
1. Tổng quan tech stack
2. Mô tả kiến trúc
3. Quy ước code
4. Mục tiêu hiện tại

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ học cách dùng đặc tả API làm context — giúp code do AI tạo khớp chính xác với hợp đồng API.
