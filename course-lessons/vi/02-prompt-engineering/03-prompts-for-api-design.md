# Prompt thiết kế API

## Thiết kế API với AI

Thiết kế API là kỹ năng quan trọng với lập trình viên web. Dù bạn xây dựng REST endpoint, GraphQL schema hay tRPC router, AI có thể giúp bạn thiết kế API sạch, nhất quán và có tài liệu tốt.

## Prompt REST API

### Endpoint CRUD cơ bản

```
Tạo REST API cho dự án dùng Next.js 14 App Router.

Endpoint: /api/projects

Methods:
- GET /api/projects — Liệt kê tất cả dự án (phân trang, 10 mỗi trang)
  - Query params: page, limit, search, sortBy, sortOrder
  - Trả về: { data: Project[], pagination: { page, limit, total, totalPages } }

- GET /api/projects/[id] — Lấy dự án đơn
  - Trả về: Project kèm task và tech stack liên quan
  - 404 nếu không tìm thấy

- POST /api/projects — Tạo dự án
  - Body: { name: string, description?: string, techStack: string[], repoUrl?: string }
  - Validation với Zod
  - Trả về: Project đã tạo với status 201

- PUT /api/projects/[id] — Cập nhật dự án
  - Hỗ trợ cập nhật từng phần
  - 404 nếu không tìm thấy
  - Trả về: Project đã cập nhật

- DELETE /api/projects/[id] — Xóa dự án
  - Xóa mềm (set deletedAt)
  - 404 nếu không tìm thấy
  - Trả về: 204 No Content

Bao gồm:
- Kiểu TypeScript cho tất cả request/response
- Schema Zod cho validation
- Xử lý lỗi với định dạng lỗi nhất quán
- Kiểm tra xác thực (yêu cầu đăng nhập)
- Rate limiting
```

### Tài nguyên lồng nhau

```
Tạo REST API cho task dự án (lồng dưới dự án).

Endpoints:
- GET /api/projects/[projectId]/tasks — Liệt kê task của dự án
- POST /api/projects/[projectId]/tasks — Tạo task trong dự án
- GET /api/projects/[projectId]/tasks/[taskId] — Lấy task đơn
- PUT /api/projects/[projectId]/tasks/[taskId] — Cập nhật task
- DELETE /api/projects/[projectId]/tasks/[taskId] — Xóa task

Model Task:
- id: string (UUID)
- title: string
- description?: string
- status: 'todo' | 'in_progress' | 'done'
- priority: 'low' | 'medium' | 'high'
- assigneeId?: string
- dueDate?: Date
- createdAt: Date
- updatedAt: Date

Bao gồm phân quyền đúng (chỉ thành viên dự án mới quản lý được task).
```

## Prompt GraphQL

### Định nghĩa Schema

```
Tạo GraphQL schema cho API quản lý dự án.

Types:
- User: id, name, email, avatar, role, projects, createdAt
- Project: id, name, description, techStack, status, owner, members, tasks, createdAt
- Task: id, title, description, status, priority, assignee, project, dueDate

Queries:
- me: User (người dùng hiện tại)
- projects(filter: ProjectFilter, pagination: PaginationInput): ProjectConnection
- project(id: ID!): Project
- tasks(projectId: ID!, filter: TaskFilter): [Task]

Mutations:
- createProject(input: CreateProjectInput!): Project
- updateProject(id: ID!, input: UpdateProjectInput!): Project
- deleteProject(id: ID!): Boolean
- createTask(projectId: ID!, input: CreateTaskInput!): Task
- updateTask(id: ID!, input: UpdateTaskInput!): Task

Bao gồm:
- Input types cho mutation
- Connection type cho phân trang
- Filter input types
- Custom scalars (DateTime, URL)
- Xử lý lỗi với union types
```

### Triển khai Resolver

```
Triển khai GraphQL resolver cho type Project dùng Apollo Server với TypeScript.

Cho type Project:
- owner: Resolve User từ ownerId (batch với DataLoader)
- members: Resolve User[] từ memberIds (batch với DataLoader)
- tasks: Resolve Task[] với bộ lọc status tùy chọn
- taskCount: Trường tính toán đếm task theo status

Bao gồm:
- DataLoader để tránh N+1
- Middleware xác thực
- Kiểm tra phân quyền (chỉ thành viên mới xem được dự án riêng tư)
- Xử lý lỗi với ApolloError
- Kiểu TypeScript tạo từ schema
```

## Prompt tRPC

### Định nghĩa Router

```
Tạo tRPC router cho dự án dùng Next.js 14.

Router: projectRouter

Procedures:
- list: Công khai, danh sách dự án phân trang với tìm kiếm
  - Input: { page: number, limit: number, search?: string }
  - Trả về: { items: Project[], total: number }

- getById: Công khai, lấy dự án theo ID
  - Input: { id: string }
  - Trả về: Project kèm task và chủ sở hữu

- create: Cần đăng nhập, tạo dự án mới
  - Input: { name: string, description?: string, techStack: string[] }
  - Trả về: Project

- update: Cần đăng nhập, cập nhật dự án (chỉ chủ sở hữu)
  - Input: { id: string, name?: string, description?: string }
  - Trả về: Project

- delete: Cần đăng nhập, xóa dự án (chỉ chủ sở hữu)
  - Input: { id: string }
  - Trả về: { success: boolean }

Bao gồm:
- Validation input Zod
- Middleware xác thực
- Kiểm tra phân quyền
- Xử lý lỗi với TRPCError
- Truy vấn database với Prisma
```

## Prompt tài liệu API

### OpenAPI Spec

```
Tạo đặc tả OpenAPI 3.0 cho API dự án.

Bao gồm:
- Tất cả endpoint với method, đường dẫn và mô tả
- Schema request/response với ví dụ
- Xác thực (Bearer token)
- Phản hồi lỗi (400, 401, 403, 404, 500)
- Tham số phân trang
- Tùy chọn lọc và sắp xếp
- Header rate limiting

Định dạng: YAML
File: docs/api/openapi.yaml
```

### Tài liệu API

```
Tạo tài liệu API cho endpoint dự án.

Bao gồm:
- Phần tổng quan giải thích API
- Yêu cầu xác thực
- URL gốc và phiên bản
- Mỗi endpoint với:
  - Mô tả
  - HTTP method và đường dẫn
  - Tham số và body request
  - Định dạng response với ví dụ
  - Mã lỗi và thông báo
  - Thông tin rate limiting
- Ví dụ code bằng JavaScript/TypeScript
- Trường hợp sử dụng phổ biến và workflow

Định dạng: Markdown
File: docs/api/projects.md
```

## Best Practice cần đưa vào Prompt

### Xử lý lỗi
```
Dùng định dạng response lỗi nhất quán:
{
  error: {
    code: "VALIDATION_ERROR",
    message: "Thông báo dễ đọc",
    details: [{ field: "name", message: "Tên là bắt buộc" }]
  }
}

Ánh xạ sang HTTP status codes:
- 400: Lỗi validation
- 401: Cần xác thực
- 403: Không đủ quyền
- 404: Không tìm thấy tài nguyên
- 409: Xung đột (trùng lặp)
- 429: Vượt quá giới hạn
- 500: Lỗi server nội bộ
```

### Validation
```
Dùng Zod cho tất cả validation input:
- Validate ở ranh giới API
- Trả về lỗi validation chi tiết
- Sanitize input người dùng
- Suy luận kiểu từ schema Zod
```

## Bài tập thực hành

Thiết kế API cho WebDevHub:

1. **User API** — Đăng ký, đăng nhập, quản lý hồ sơ
2. **Snippet API** — CRUD cho code snippet với tagging
3. **Portfolio API** — Dữ liệu portfolio công khai cho hồ sơ lập trình viên

Viết prompt hoàn chỉnh cho mỗi API, sau đó triển khai với công cụ AI.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ học cách prompt AI để debug — sửa lỗi, giải quyết vấn đề type và xử lý sự cố phát triển web thường gặp.
