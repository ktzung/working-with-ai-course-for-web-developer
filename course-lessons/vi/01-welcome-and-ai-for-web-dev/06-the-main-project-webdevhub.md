# Dự án chính: WebDevHub

## Dự án sẵn sàng Portfolio

Xuyên suốt khóa học, bạn sẽ xây dựng **WebDevHub** — nền tảng quản lý dự án và portfolio được thiết kế đặc biệt cho lập trình viên web. Đây không phải dự án demo; mà là ứng dụng thật với tính năng cấp production mà bạn có thể tùy chỉnh và triển khai.

## WebDevHub là gì?

WebDevHub là ứng dụng web full-stack cho phép lập trình viên:

- **Showcase sản phẩm** — Trang portfolio đẹp với card dự án, badge tech stack và demo trực tiếp
- **Theo dõi dự án** — Bảng Kanban quản lý task và deadline
- **Quản lý snippet** — Thư viện code snippet với syntax highlighting và tagging
- **Cộng tác** — Chia sẻ dự án với đồng đội và nhận phản hồi
- **Triển khai dễ dàng** — Cấu hình deployment sẵn cho Vercel, Netlify hoặc server riêng

## Tech Stack

Chúng tôi chọn stack hiện đại, sẵn sàng production:

### Frontend
- **Next.js 14** — React framework với App Router, Server Component và API route
- **TypeScript** — An toàn kiểu trên toàn ứng dụng
- **Tailwind CSS** — Styling utility-first với design token tùy chỉnh
- **Zustand** — Quản lý state nhẹ
- **React Hook Form** — Xử lý form với validation

### Backend
- **Next.js API Routes** — API endpoint serverless
- **Prisma** — ORM an toàn kiểu
- **PostgreSQL** — Cơ sở dữ liệu quan hệ
- **NextAuth.js** — Xác thực với nhiều nhà cung cấp

### DevOps
- **GitHub Actions** — CI/CD pipeline
- **Vercel** — Nền tảng triển khai
- **Docker** — Container hóa cho phát triển local

## Cấu trúc dự án

```
webdevhub/
├── src/
│   ├── app/                    # Next.js App Router pages
│   │   ├── (auth)/            # Nhóm xác thực (login, register)
│   │   ├── (dashboard)/       # Nhóm dashboard
│   │   ├── api/               # API routes
│   │   ├── layout.tsx         # Root layout
│   │   └── page.tsx           # Trang chủ
│   ├── components/
│   │   ├── ui/                # UI component cơ bản
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Input.tsx
│   │   │   └── Modal.tsx
│   │   ├── layout/            # Component layout
│   │   │   ├── Header.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── Footer.tsx
│   │   ├── portfolio/         # Tính năng portfolio
│   │   │   ├── ProjectCard.tsx
│   │   │   ├── TechBadge.tsx
│   │   │   └── ProjectGrid.tsx
│   │   ├── dashboard/         # Tính năng dashboard
│   │   │   ├── KanbanBoard.tsx
│   │   │   ├── TaskCard.tsx
│   │   │   └── StatsWidget.tsx
│   │   └── snippets/          # Code snippet
│   │       ├── SnippetCard.tsx
│   │       ├── CodeBlock.tsx
│   │       └── SnippetEditor.tsx
│   ├── hooks/                 # Custom hook
│   ├── lib/                   # Thư viện tiện ích
│   ├── services/              # Hàm service API
│   ├── types/                 # Kiểu TypeScript
│   └── styles/                # Style toàn cục
├── prisma/
│   └── schema.prisma          # Schema database
├── public/                    # Tài nguyên tĩnh
├── tests/                     # File test
├── .github/                   # GitHub workflow
├── docker-compose.yml         # Cấu hình Docker
└── package.json
```

## Tính năng sẽ xây dựng

### Giai đoạn 1: Nền tảng (Phần 1-4)
- Thiết lập và cấu hình dự án
- Thư viện component với hỗ trợ AI
- Layout responsive
- Pattern quản lý state

### Giai đoạn 2: Tính năng cốt lõi (Phần 5-8)
- Showcase portfolio với card dự án
- Bảng Kanban quản lý dự án
- Trình quản lý code snippet
- Xác thực người dùng

### Giai đoạn 3: Tính năng nâng cao (Phần 9-12)
- Thiết kế và triển khai API
- Tích hợp cơ sở dữ liệu
- Cập nhật thời gian thực
- Triển khai và tối ưu hóa

## AI hỗ trợ thế nào ở mỗi giai đoạn

| Tính năng | Hỗ trợ AI |
|---------|---------------|
| Thư viện Component | Tạo component cơ bản, gợi ý pattern |
| Trang Portfolio | Tạo layout responsive, tối ưu hình ảnh |
| Bảng Kanban | Triển khai kéo thả, quản lý state |
| Trình quản lý Snippet | Syntax highlighting, tìm kiếm |
| Xác thực | Thiết lập NextAuth, route bảo vệ |
| API Routes | Thiết kế endpoint, validation, xử lý lỗi |
| Database | Schema Prisma, migration, truy vấn |
| Triển khai | Cấu hình Docker, CI/CD pipeline |

## Task đầu tiên

Trước bài tiếp theo, hãy thiết lập môi trường phát triển:

1. **Tạo dự án Next.js mới**:
   ```bash
   npx create-next-app@latest webdevhub --typescript --tailwind --eslint --app --src-dir
   ```

2. **Cài dependencies chính**:
   ```bash
   cd webdevhub
   npm install zustand react-hook-form @hookform/resolvers zod
   npm install prisma @prisma/client next-auth
   ```

3. **Khởi tạo Prisma**:
   ```bash
   npx prisma init
   ```

4. **Mở trong VS Code**:
   ```bash
   code .
   ```

## Tiếp theo là gì?

Bắt đầu từ Phần 2, chúng ta sẽ đi sâu vào prompt engineering — học cách giao tiếp với công cụ AI để tạo đúng code bạn cần. Bạn sẽ thực hành với component và tính năng WebDevHub thật.

Dự án là sân chơi của bạn. Mọi kỹ thuật học được sẽ được áp dụng trực tiếp vào xây dựng WebDevHub. Đến cuối khóa, bạn sẽ có ứng dụng hoàn chỉnh, sẵn sàng triển khai và kỹ năng để xây dựng nhiều ứng dụng hơn nữa.
