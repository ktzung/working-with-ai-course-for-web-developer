# Coding Standard làm Context

## Đảm bảo nhất quán với AI

Coding standard đảm bảo code do AI tạo trông như được viết bởi team bạn. Khi chia sẻ tiêu chuẩn làm context, AI sẽ tuân thủ quy ước đặt tên, quy tắc format và pattern kiến trúc.

## Cấu hình ESLint

ESLint thực thi quy tắc chất lượng code. Chia sẻ config với AI:

```json
// .eslintrc.json
{
  "extends": [
    "next/core-web-vitals",
    "eslint:recommended",
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint", "react", "react-hooks"],
  "rules": {
    // Quy tắc TypeScript
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_" }],
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/prefer-const": "error",
    
    // Quy tắc React
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    
    // Quy tắc chung
    "no-console": ["warn", { "allow": ["warn", "error"] }],
    "prefer-template": "error",
    "no-var": "error",
    "eqeqeq": ["error", "always"]
  }
}
```

### Dùng ESLint làm Context

```
Config ESLint của tôi thực thi:
- Không dùng kiểu any tường minh
- Kiểu trả về hàm tường minh
- Ưu tiên const
- Không dùng biến không dùng (trừ tiền tố _)
- Ưu tiên template literal
- So sánh nghiêm ngặt (===)

Tạo code tuân thủ các quy tắc này.
```

## Cấu hình Prettier

Prettier đảm bảo format nhất quán:

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

### Dùng Prettier làm Context

```
Config Prettier:
- Dấu chấm phẩy: có
- Nháy đơn: có
- Dấu phẩy cuối: kiểu ES5
- Độ rộng: 80 ký tự
- Tab: 2 dấu cách

Format tất cả code tạo ra khớp cài đặt này.
```

## Quy ước đặt tên

### Đặt tên file

```
Quy ước đặt tên dự án:

Files:
- Component: PascalCase (UserProfile.tsx, ProjectCard.tsx)
- Hook: camelCase với tiền tố 'use' (useProjects.ts, useAuth.ts)
- Utils: camelCase (formatDate.ts, validateEmail.ts)
- Types: PascalCase (Project.ts, User.ts)
- Hằng số: camelCase (config.ts, constants.ts)
- API routes: route.ts (quy ước Next.js)
- Test: {tên}.test.ts hoặc {tên}.spec.ts

Thư mục:
- Component: kebab-case (user-profile/, project-card/)
- Features: kebab-case (authentication/, project-management/)
```

### Đặt tên biến

```
Quy ước đặt tên biến:

- Biến: camelCase (userName, projectList)
- Hằng số: UPPER_SNAKE_CASE (API_BASE_URL, MAX_RETRY_COUNT)
- Types/Interfaces: PascalCase (UserProfile, ProjectStatus)
- Enums: Tên PascalCase, thành viên PascalCase (TaskStatus.InProgress)
- Biến boolean: tiền tố has/is/should (isLoading, hasError, shouldRefresh)
- Hàm: camelCase, động từ đứng đầu (getUserData, formatDate, validateInput)
- Event handler: tiền tố handle (handleSubmit, handleClick)
- Component React: PascalCase (UserCard, ProjectList)
```

## Pattern Component

### Mẫu component chuẩn

```typescript
// Pattern component chuẩn cho dự án

import { type FC } from 'react';
import { type Project } from '@/types/Project';
import { Card } from '@/components/ui/Card';
import { Badge } from '@/components/ui/Badge';

interface ProjectCardProps {
  project: Project;
  onSelect?: (id: string) => void;
  className?: string;
}

export const ProjectCard: FC<ProjectCardProps> = ({
  project,
  onSelect,
  className,
}) => {
  const handleClick = () => {
    onSelect?.(project.id);
  };

  return (
    <Card className={className} onClick={handleClick}>
      <Card.Header>
        <h3>{project.name}</h3>
        <Badge variant={project.status}>{project.status}</Badge>
      </Card.Header>
      <Card.Body>
        <p>{project.description}</p>
        <div className="flex gap-2">
          {project.techStack.map((tech) => (
            <Badge key={tech} variant="outline">{tech}</Badge>
          ))}
        </div>
      </Card.Body>
    </Card>
  );
};
```

### Mẫu hook chuẩn

```typescript
// Pattern hook chuẩn cho dự án

import { useState, useEffect, useCallback } from 'react';
import { type Project } from '@/types/Project';
import { projectService } from '@/services/projectService';

interface UseProjectsOptions {
  page?: number;
  limit?: number;
  search?: string;
}

interface UseProjectsResult {
  projects: Project[];
  total: number;
  isLoading: boolean;
  error: Error | null;
  refetch: () => void;
}

export const useProjects = (options: UseProjectsOptions = {}): UseProjectsResult => {
  const { page = 1, limit = 10, search } = options;
  
  const [projects, setProjects] = useState<Project[]>([]);
  const [total, setTotal] = useState(0);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  const fetchProjects = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    
    try {
      const response = await projectService.getProjects({ page, limit, search });
      setProjects(response.data);
      setTotal(response.pagination.total);
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Không thể tải dự án'));
    } finally {
      setIsLoading(false);
    }
  }, [page, limit, search]);

  useEffect(() => {
    fetchProjects();
  }, [fetchProjects]);

  return { projects, total, isLoading, error, refetch: fetchProjects };
};
```

## Tổ chức Import

```
Thứ tự import cho dự án:

1. Import React và Next.js
2. Thư viện bên thứ ba
3. Component nội bộ (ui, layout, features)
4. Hooks
5. Services
6. Types
7. Utils
8. Styles

Ví dụ:
import { useState, useEffect } from 'react';
import { useRouter } from 'next/navigation';

import { z } from 'zod';
import { format } from 'date-fns';

import { Button } from '@/components/ui/Button';
import { Card } from '@/components/ui/Card';
import { ProjectCard } from '@/components/features/projects/ProjectCard';

import { useProjects } from '@/hooks/useProjects';

import { projectService } from '@/services/projectService';

import { type Project } from '@/types/Project';

import { formatDate } from '@/utils/formatDate';

import styles from './ProjectList.module.css';
```

## Pattern xử lý lỗi

```
Quy ước xử lý lỗi:

1. Lỗi API: Dùng try/catch với response lỗi có kiểu
2. Lỗi component: Dùng Error Boundary
3. Lỗi form: Dùng Zod validation với thông báo thân thiện
4. Lỗi mạng: Hiển thị tùy chọn thử lại
5. Lỗi xác thực: Chuyển hướng đến đăng nhập

Response lỗi chuẩn:
interface ApiError {
  code: string;
  message: string;
  details?: { field: string; message: string }[];
}
```

## Pattern Testing

```
Quy ước testing:

1. Vị trí file test: Cùng thư mục với source, đặt tên {tên}.test.ts
2. Cấu trúc test: Arrange, Act, Assert
3. Mock dependencies bên ngoài
4. Test cả trường hợp thành công và lỗi
5. Dùng tên test mô tả

Ví dụ:
describe('ProjectCard', () => {
  it('hiển thị tên và mô tả dự án', () => {
    // Arrange
    const project = { id: '1', name: 'Test', description: 'Mô tả' };
    
    // Act
    render(<ProjectCard project={project} />);
    
    // Assert
    expect(screen.getByText('Test')).toBeInTheDocument();
    expect(screen.getByText('Mô tả')).toBeInTheDocument();
  });
});
```

## Tạo tài liệu tiêu chuẩn

Kết hợp tất cả tiêu chuẩn thành một tài liệu:

```markdown
# Coding Standard

## Tham chiếu nhanh
- TypeScript strict mode
- Functional component với hook
- Chỉ dùng named export
- PascalCase component, camelCase hàm
- Nháy đơn, dấu chấm phẩy, dấu phẩy cuối
- Thiết kế responsive mobile-first
- Error boundary cho lỗi component
- Zod cho validation

## Quy tắc chi tiết
[Bao gồm tài liệu ESLint, Prettier và pattern đầy đủ]
```

## Bài tập thực hành

1. Thiết lập ESLint và Prettier cho dự án
2. Tạo tài liệu coding standard
3. Tạo component dùng AI với tiêu chuẩn làm context
4. Xác minh code tạo ra tuân thủ tất cả tiêu chuẩn

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ tạo file current-task.md — tài liệu context tập trung cho thứ bạn đang xây dựng.
