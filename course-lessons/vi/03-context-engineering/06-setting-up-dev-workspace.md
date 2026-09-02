# Thiết lập Workspace phát triển

## Kết hợp tất cả cùng nhau

Bạn đã học về project context, API spec, coding standard và task file. Giờ hãy thiết lập workspace phát triển hoàn chỉnh nơi tất cả file context hoạt động cùng nhau.

## Cấu trúc Workspace hoàn chỉnh

```
webdevhub/
├── .github/
│   ├── copilot-instructions.md    # Context cho GitHub Copilot
│   └── workflows/
│       └── ci.yml                 # CI/CD pipeline
├── .vscode/
│   ├── settings.json              # Cài đặt VS Code
│   ├── extensions.json            # Extension khuyến nghị
│   └── launch.json                # Cấu hình debug
├── docs/
│   ├── context/
│   │   ├── project-context.md     # Context dự án chính
│   │   ├── coding-standards.md    # Quy ước code
│   │   └── architecture.md        # Quyết định kiến trúc
│   ├── api/
│   │   └── openapi.yaml           # Đặc tả API
│   └── current-task.md            # Focus task hiện tại
├── src/
│   ├── app/                       # Next.js App Router
│   ├── components/                # Component React
│   ├── hooks/                     # Custom hook
│   ├── services/                  # Service API
│   ├── types/                     # Kiểu TypeScript
│   └── utils/                     # Hàm tiện ích
├── prisma/
│   └── schema.prisma              # Schema database
├── tests/                         # File test
├── .cursorrules                   # Context Cursor AI
├── .eslintrc.json                 # Config ESLint
├── .prettierrc                    # Config Prettier
├── tailwind.config.ts             # Config Tailwind
├── tsconfig.json                  # Config TypeScript
└── package.json                   # Dependencies
```

## Thiết lập từng file Context

### 1. Hướng dẫn Copilot

Tạo `.github/copilot-instructions.md`:

```markdown
# WebDevHub - Hướng dẫn Copilot

## Tổng quan dự án
WebDevHub là nền tảng quản lý dự án và portfolio cho lập trình viên web.

## Tech Stack
- Next.js 14 với App Router
- TypeScript (strict mode)
- Tailwind CSS
- Prisma + PostgreSQL
- NextAuth.js

## Quy ước
- Functional component với hook
- Chỉ dùng named export
- PascalCase cho component, camelCase cho hàm
- Thiết kế responsive mobile-first
- Error boundary cho lỗi component

## Focus hiện tại
Xem docs/current-task.md cho mục tiêu sprint hiện tại.
```

### 2. Cài đặt VS Code

Tạo `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "typescript.updateImportsOnFileMove.enabled": "always",
  "typescript.suggest.autoImports": true,
  "github.copilot.enable": true,
  "github.copilot.inlineSuggest.enable": true,
  "editor.inlineSuggest.enabled": true
}
```

### 3. Extension khuyến nghị

Tạo `.vscode/extensions.json`:

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "bradlc.vscode-tailwindcss",
    "prisma.prisma",
    "ms-vscode.vscode-typescript-next",
    "github.copilot",
    "github.copilot-chat"
  ]
}
```

### 4. Cursor Rules

Tạo `.cursorrules`:

```
Bạn đang làm việc trên WebDevHub, nền tảng quản lý dự án và portfolio.

Tech Stack:
- Next.js 14 với App Router
- TypeScript (strict mode)
- Tailwind CSS
- Prisma + PostgreSQL
- NextAuth.js

Quy tắc:
1. Dùng functional component với hook
2. Dùng named export (không default export)
3. PascalCase cho component, camelCase cho hàm
4. TypeScript strict mode (không dùng kiểu any)
5. Dùng Tailwind CSS cho styling
6. Dùng Zod cho validation
7. Thiết kế responsive mobile-first
8. Bao gồm xử lý lỗi trong mọi API call
9. Dùng Prisma cho truy vấn database
10. Theo pattern trong docs/context/coding-standards.md
```

## Git Workflow

### Đặt tên nhánh
```
feature/user-authentication
feature/project-kanban
fix/login-redirect
refactor/api-services
```

### Commit message
```
feat: thêm xác thực người dùng với NextAuth
fix: sửa vòng lặp chuyển hướng đăng nhập
refactor: tách service layer dự án
docs: cập nhật đặc tả API
```

### Pre-commit hook

Thiết lập với Husky:

```bash
npm install --save-dev husky lint-staged
npx husky install
```

Thêm vào `package.json`:
```json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
}
```

## Workflow file Context

### Bắt đầu task mới
1. Cập nhật `docs/current-task.md` với task mới
2. Mở file liên quan trong VS Code
3. Tham chiếu context trong prompt AI đầu tiên

### Trong quá trình phát triển
1. Giữ `current-task.md` cập nhật
2. Thêm pattern mới vào `coding-standards.md`
3. Cập nhật `project-context.md` khi kiến trúc thay đổi

### Hoàn thành task
1. Đánh dấu tất cả yêu cầu hoàn thành trong `current-task.md`
2. Ghi chép pattern hoặc quyết định mới
3. Cập nhật spec API nếu endpoint thay đổi

## Tham chiếu nhanh

### Vị trí file Context
| File | Mục đích | Vị trí |
|------|---------|----------|
| Project Context | Thông tin dự án tổng thể | `docs/context/project-context.md` |
| Coding Standards | Quy ước code | `docs/context/coding-standards.md` |
| API Spec | Hợp đồng API | `docs/api/openapi.yaml` |
| Current Task | Focus hiện tại | `docs/current-task.md` |
| Copilot Instructions | Context Copilot | `.github/copilot-instructions.md` |
| Cursor Rules | Context Cursor | `.cursorrules` |

### Khi cập nhật mỗi file
| File | Cập nhật khi |
|------|-------------|
| project-context.md | Kiến trúc thay đổi, công nghệ mới |
| coding-standards.md | Pattern mới, thay đổi quy ước |
| openapi.yaml | API thay đổi |
| current-task.md | Bắt đầu task mới |
| copilot-instructions.md | Thay đổi lớn dự án |

## Bài tập thực hành

1. Thiết lập cấu trúc workspace hoàn chỉnh cho WebDevHub
2. Tạo tất cả file context
3. Thực hiện commit dùng Git workflow
4. Bắt đầu task mới dùng hệ thống context

## Tiếp theo là gì?

Chúc mừng! Bạn đã hoàn thành phần context engineering. Trong Phần 4, chúng ta sẽ áp dụng mọi thứ đã học vào phát triển frontend — xây dựng component và tính năng thật với hỗ trợ AI.
