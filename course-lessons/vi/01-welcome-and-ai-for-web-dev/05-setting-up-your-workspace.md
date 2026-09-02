# Thiết lập Workspace

## Xây dựng môi trường phát triển hỗ trợ AI

Workspace được cấu hình tốt giúp công cụ AI hoạt động hiệu quả hơn đáng kể. Trong bài này, chúng ta sẽ thiết lập VS Code với extension, cài đặt và workflow phù hợp cho phát triển web hỗ trợ AI.

## Bước 1: Cài Extension VS Code

Các extension này tạo nền tảng cho workspace hỗ trợ AI:

### Extension AI thiết yếu

**GitHub Copilot** — Trợ lý code AI chính
- Cài từ Extensions marketplace
- Đăng nhập bằng tài khoản GitHub (cần đăng ký Copilot)
- Bật gợi ý nội dòng trong settings

**GitHub Copilot Chat** — AI hội thoại trong editor
- Hoạt động cùng Copilot cho các cuộc thảo luận dài hơn
- Dùng `@workspace` để tham chiếu toàn bộ dự án
- Dùng lệnh `/explain`, `/fix`, `/test`

**Cursor** (Lựa chọn thay thế) — Nếu bạn thích Cursor hơn
- AI tích hợp sẵn với hiểu biết codebase
- Không cần extension bổ sung

### Extension phát triển Web thiết yếu

**ESLint** — Bắt vấn đề chất lượng code
```json
// .eslintrc.json
{
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "plugins": ["react", "react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

**Prettier** — Định dạng code nhất quán
```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

**Tailwind CSS IntelliSense** — Tự động hoàn thành class Tailwind
- Tự động hoàn thành class
- Lint class Tailwind
- Xem trước CSS khi hover

**TypeScript Hero** — Tự động import và quản lý TypeScript

## Bước 2: Cài đặt VS Code

Mở settings (`Cmd+,` trên Mac, `Ctrl+,` trên Windows) và cấu hình:

```json
{
  // Cài đặt AI
  "github.copilot.enable": true,
  "github.copilot.inlineSuggest.enable": true,
  "editor.inlineSuggest.enabled": true,
  
  // Cài đặt Editor
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  
  // Cài đặt TypeScript
  "typescript.updateImportsOnFileMove.enabled": "always",
  "typescript.suggest.autoImports": true,
  
  // Liên kết file
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

## Bước 3: Thiết lập cấu trúc dự án

Cấu trúc dự án nhất quán giúp công cụ AI hiểu codebase tốt hơn:

```
my-web-project/
├── src/
│   ├── components/        # UI component tái sử dụng
│   │   ├── ui/           # Thành phần UI cơ bản
│   │   ├── layout/       # Component layout
│   │   └── features/     # Component theo feature
│   ├── pages/            # Component trang
│   ├── hooks/            # Custom React hook
│   ├── utils/            # Hàm tiện ích
│   ├── types/            # Định nghĩa type TypeScript
│   ├── services/         # API call và dịch vụ ngoài
│   ├── context/          # React context provider
│   └── styles/           # Style toàn cục
├── public/               # Tài nguyên tĩnh
├── tests/                # File test
├── docs/                 # Tài liệu
└── .github/              # GitHub workflow
```

## Bước 4: Tạo file context AI

Các file này giúp công cụ AI hiểu dự án tốt hơn:

### `.github/copilot-instructions.md`
```markdown
# Context dự án

## Tech Stack
- Framework: Next.js 14 với App Router
- Ngôn ngữ: TypeScript
- Styling: Tailwind CSS
- State: Zustand
- API: tRPC
- Database: PostgreSQL với Prisma

## Quy ước
- Dùng functional component với hook
- Ưu tiên named export
- Dùng TypeScript strict mode
- Tuân thủ Airbnb style guide

## Đặt tên file
- Component: PascalCase (UserProfile.tsx)
- Hook: camelCase với tiền tố 'use' (useUserData.ts)
- Utils: camelCase (formatDate.ts)
```

### `CONTEXT.md` (cho công cụ AI khác)
Nội dung tương tự được định dạng cho ChatGPT, Claude hoặc Cursor.

## Bước 5: Thiết lập Git Workflow

Cấu hình Git cho phát triển hỗ trợ AI:

```bash
# Alias hữu ích
git config --global alias.ai-commit '!git add -A && git commit -m "$(git diff --staged --stat)"'

# Pre-commit hook cho chất lượng code
npm install --save-dev husky lint-staged
npx husky install
```

## Bước 6: Tích hợp Terminal

Thiết lập terminal để truy cập AI nhanh:

```bash
# Cài GitHub CLI
brew install gh

# Cài công cụ AI CLI
npm install -g @anthropic-ai/claude-cli
```

## Tham chiếu nhanh

Giữ bảng này bên cạnh khi bắt đầu làm việc:

| Hành động | Phím tắt/Lệnh |
|--------|------------------|
| Chấp nhận gợi ý Copilot | `Tab` |
| Bỏ qua gợi ý | `Esc` |
| Mở Copilot Chat | `Cmd+Shift+I` |
| Giải thích code | Chọn code, rồi `/explain` |
| Sửa code | Chọn code, rồi `/fix` |
| Tạo test | Chọn code, rồi `/test` |

## Tiếp theo là gì?

Workspace đã sẵn sàng! Trong bài tiếp theo, chúng ta sẽ giới thiệu dự án WebDevHub — dự án chính bạn sẽ xây dựng xuyên suốt khóa học bằng tất cả kỹ năng AI đã học.
