# Đặc tả API làm Context

## Sử dụng đặc tả API để hướng dẫn AI

Đặc tả API là một trong những dạng context mạnh nhất cho AI. Khi bạn chia sẻ spec OpenAPI/Swagger, schema GraphQL hoặc định nghĩa router tRPC, AI có thể tạo code khớp hoàn hảo với hợp đồng API.

## Tại sao đặc tả API quan trọng

Không có đặc tả API, AI có thể:
- Đoán tên trường (là `userName` hay `name` hay `username`?)
- Bỏ sót trường bắt buộc
- Sai kiểu dữ liệu
- Quên phân trang
- Bỏ qua response lỗi

Có đặc tả API, AI sẽ:
- Dùng đúng tên trường và kiểu
- Xử lý tất cả định dạng response
- Bao gồm xử lý lỗi đúng
- Theo pattern phân trang

## Đặc tả OpenAPI/Swagger

### Tạo spec OpenAPI

```yaml
# openapi.yaml
openapi: 3.0.0
info:
  title: WebDevHub API
  version: 1.0.0
  description: API cho nền tảng quản lý dự án WebDevHub

servers:
  - url: http://localhost:3000/api
    description: Phát triển local

paths:
  /projects:
    get:
      summary: Liệt kê dự án
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
        - name: search
          in: query
          schema:
            type: string
      responses:
        '200':
          description: Thành công
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Project'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

    post:
      summary: Tạo dự án
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateProjectInput'
      responses:
        '201':
          description: Đã tạo
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    $ref: '#/components/schemas/Project'

components:
  schemas:
    Project:
      type: object
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
        description:
          type: string
        techStack:
          type: array
          items:
            type: string
        status:
          type: string
          enum: [active, archived, draft]
        ownerId:
          type: string
          format: uuid
        createdAt:
          type: string
          format: date-time

    CreateProjectInput:
      type: object
      required:
        - name
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 100
        description:
          type: string
          maxLength: 500
        techStack:
          type: array
          items:
            type: string
          maxItems: 20

    Pagination:
      type: object
      properties:
        page:
          type: integer
        limit:
          type: integer
        total:
          type: integer
        totalPages:
          type: integer
```

### Dùng OpenAPI với AI

Khi prompt AI, tham chiếu spec:

```
Tôi có spec OpenAPI tại docs/api/openapi.yaml.

Tạo API client TypeScript cho endpoint dự án:
- Dùng native fetch API
- Bao gồm tất cả kiểu từ spec
- Xử lý phân trang
- Xử lý lỗi với định dạng lỗi chuẩn
- Bao gồm interceptor request/response
```

## Schema GraphQL

### Schema làm Context

```graphql
# schema.graphql
type User {
  id: ID!
  name: String!
  email: String!
  avatar: String
  role: Role!
  projects: [Project!]!
  createdAt: DateTime!
}

type Project {
  id: ID!
  name: String!
  description: String
  techStack: [String!]!
  status: ProjectStatus!
  owner: User!
  members: [User!]!
  tasks: [Task!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Task {
  id: ID!
  title: String!
  description: String
  status: TaskStatus!
  priority: Priority!
  assignee: User
  project: Project!
  dueDate: DateTime
}

enum Role { ADMIN MEMBER VIEWER }
enum ProjectStatus { ACTIVE ARCHIVED DRAFT }
enum TaskStatus { TODO IN_PROGRESS DONE }
enum Priority { LOW MEDIUM HIGH }

type Query {
  me: User!
  projects(filter: ProjectFilter, pagination: PaginationInput): ProjectConnection!
  project(id: ID!): Project
  tasks(projectId: ID!, filter: TaskFilter): [Task!]!
}

type Mutation {
  createProject(input: CreateProjectInput!): Project!
  updateProject(id: ID!, input: UpdateProjectInput!): Project!
  deleteProject(id: ID!): Boolean!
  createTask(projectId: ID!, input: CreateTaskInput!): Task!
  updateTask(id: ID!, input: UpdateTaskInput!): Task!
}
```

### Dùng schema GraphQL với AI

```
Đây là schema GraphQL: [paste schema]

Tạo React hook cho:
- useProjects (danh sách với bộ lọc và phân trang)
- useProject (dự án đơn theo ID)
- useCreateProject (mutation)
- useUpdateProject (mutation)

Dùng Apollo Client với TypeScript.
Bao gồm trạng thái loading, error và success.
```

## Định nghĩa Router tRPC

### Router làm Context

```typescript
// server/routers/project.ts
import { z } from 'zod';
import { router, protectedProcedure } from '../trpc';

export const projectRouter = router({
  list: protectedProcedure
    .input(z.object({
      page: z.number().min(1).default(1),
      limit: z.number().min(1).max(100).default(10),
      search: z.string().optional(),
    }))
    .query(async ({ input, ctx }) => {
      // Triển khai
    }),

  getById: protectedProcedure
    .input(z.object({ id: z.string().uuid() }))
    .query(async ({ input, ctx }) => {
      // Triển khai
    }),

  create: protectedProcedure
    .input(z.object({
      name: z.string().min(1).max(100),
      description: z.string().max(500).optional(),
      techStack: z.array(z.string()).max(20),
    }))
    .mutation(async ({ input, ctx }) => {
      // Triển khai
    }),
});
```

### Dùng định nghĩa tRPC với AI

```
Đây là router project tRPC: [paste router]

Tạo component React:
- Liệt kê dự án với tìm kiếm và phân trang
- Dùng hook trpc.project.list.useQuery
- Hiển thị skeleton loading khi fetch
- Xử lý lỗi nhẹ nhàng
- Bao gồm input tìm kiếm với debounce
```

## Kết hợp nhiều spec

Với dự án phức tạp, kết hợp specs:

```
Context cho task này:

1. Spec OpenAPI: [paste endpoint liên quan]
2. Schema Prisma: [paste model liên quan]
3. Kiểu TypeScript: [paste kiểu liên quan]

Tạo service layer:
- Triển khai API endpoint
- Dùng Prisma cho truy vấn database
- Trả về response có kiểu khớp spec OpenAPI
```

## Bài tập thực hành

1. Tạo spec OpenAPI cho API snippet của WebDevHub
2. Tạo client TypeScript dùng spec
3. Tạo React hook dùng client

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ thiết lập coding standard làm context — đảm bảo code do AI tạo tuân thủ quy ước team.
