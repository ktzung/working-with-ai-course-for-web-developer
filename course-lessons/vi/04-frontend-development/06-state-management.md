# Quản lý State với AI

## Chọn giải pháp State phù hợp

Quản lý state rất quan trọng cho ứng dụng web phức tạp. AI có thể giúp bạn triển khai giải pháp phù hợp cho từng loại state.

## Các loại State

### State cục bộ
- Input form
- Trạng thái toggle
- State UI (modal, dropdown)
- Dữ liệu riêng component

### State server
- Dữ liệu API
- Response cache
- Cập nhật lạc quan
- Dữ liệu thời gian thực

### State toàn cục
- Xác thực người dùng
- Tùy chọn theme
- Giỏ hàng
- Thông báo

## React Context

### Theme Context

```
Tạo ThemeContext cho WebDevHub dùng React Context:

Tính năng:
- Chuyển đổi chế độ sáng/tối
- Phát hiện tùy chọn hệ thống
- Lưu tùy chọn trong localStorage
- Chuyển đổi mượt giữa các theme

Triển khai:
- Component ThemeProvider
- Hook useTheme
- Component ThemeToggle

Types:
type Theme = 'light' | 'dark' | 'system';

interface ThemeContextType {
  theme: Theme;
  resolvedTheme: 'light' | 'dark';
  setTheme: (theme: Theme) => void;
}

Dùng dark mode Tailwind CSS với chiến lược class.
```

### Auth Context

```
Tạo AuthContext cho xác thực người dùng:

Tính năng:
- Hàm đăng nhập/đăng xuất
- Lưu trữ dữ liệu người dùng
- Trạng thái loading
- Hỗ trợ route bảo vệ

Triển khai:
- Component AuthProvider
- Hook useAuth
- Component ProtectedRoute

Types:
interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  role: 'admin' | 'member' | 'viewer';
}

interface AuthContextType {
  user: User | null;
  isLoading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => Promise<void>;
  register: (data: RegisterInput) => Promise<void>;
}
```

## Zustand

### Project Store

```
Tạo Zustand store cho quản lý dự án:

State:
- projects: Project[]
- currentProject: Project | null
- filters: FilterState
- isLoading: boolean
- error: string | null

Actions:
- fetchProjects(): Promise<void>
- fetchProject(id: string): Promise<void>
- createProject(data: CreateProjectInput): Promise<Project>
- updateProject(id: string, data: UpdateProjectInput): Promise<Project>
- deleteProject(id: string): Promise<void>
- setFilters(filters: Partial<FilterState>): void
- clearFilters(): void

Tính năng:
- Kiểu TypeScript cho tất cả state và actions
- Tích hợp devtools
- Persist filter trong localStorage
- Cập nhật lạc quan cho UX tốt hơn

Dùng Zustand với TypeScript.
```

### UI Store

```
Tạo Zustand store cho state UI:

State:
- sidebarCollapsed: boolean
- activeModal: string | null
- notifications: Notification[]
- theme: 'light' | 'dark'

Actions:
- toggleSidebar(): void
- openModal(id: string): void
- closeModal(): void
- addNotification(notification: Notification): void
- removeNotification(id: string): void
- setTheme(theme: 'light' | 'dark'): void

Tính năng:
- Persist trạng thái sidebar
- Tự động ẩn thông báo sau thời gian chờ
- Quản lý hàng đợi thông báo
```

## State server với React Query

```
Thiết lập React Query cho quản lý state server:

Cấu hình:
- Stale time: 5 phút
- Cache time: 10 phút
- Retry: 3 lần
- Refetch khi focus cửa sổ

Hook cần tạo:
- useProjects: Fetch dự án phân trang
- useProject: Fetch dự án đơn
- useCreateProject: Mutation tạo dự án
- useUpdateProject: Mutation cập nhật dự án
- useDeleteProject: Mutation xóa dự án

Tính năng:
- Tự động refetch nền
- Cập nhật lạc quan
- Vô hiệu hóa cache
- Trạng thái loading và lỗi
- Hỗ trợ infinite scroll
```

## Pattern quản lý State

### Derived State

```
Tạo pattern derived state:

1. Danh sách dự án đã lọc (derived từ projects + filters)
2. Thống kê dự án (derived từ danh sách dự án)
3. Quyền người dùng (derived từ vai trò)
4. Số thông báo chưa đọc (derived từ notifications)

Dùng useMemo cho tính toán tốn kém.
Tách derived state khỏi state nguồn.
```

### State Machine

```
Tạo state machine cho trạng thái dự án:

States:
- draft → active (khi publish)
- active → archived (khi archive)
- archived → active (khi restore)
- any → deleted (khi delete)

Triển khai:
- Hook useProjectStateMachine
- Map chuyển đổi hợp lệ
- Xử lý hành động cho mỗi chuyển đổi
- Hàm bảo vệ cho phân quyền
```

## Prompt cho quản lý State

### Chọn giải pháp phù hợp

```
Tôi cần quản lý state cho WebDevHub. Giúp tôi quyết định:

Loại state:
1. Xác thực người dùng (đăng nhập, đăng xuất, dữ liệu user)
2. Tùy chọn theme (sáng/tối)
3. Danh sách dự án (fetch từ API)
4. State form (form nhiều bước)
5. State UI (sidebar, modal, thông báo)

Với mỗi loại, đề xuất:
- Giải pháp tốt nhất (Context, Zustand, React Query, state cục bộ)
- Tại sao là lựa chọn tốt nhất
- Cách tiếp cận triển khai
```

### Prompt triển khai

```
Triển khai quản lý state cho tính năng danh sách dự án:

Yêu cầu:
- Fetch dự án từ /api/projects
- Hỗ trợ phân trang, tìm kiếm và bộ lọc
- Cache kết quả 5 phút
- Cập nhật lạc quan cho tạo/xóa
- Trạng thái loading và lỗi
- Hỗ trợ infinite scroll

Dùng Zustand cho state client và React Query cho state server.
Bao gồm kiểu TypeScript cho tất cả state và actions.
```

## Bài tập thực hành

Triển khai quản lý state cho WebDevHub:

1. **Theme Context** — Chế độ sáng/tối với persist
2. **Auth Context** — Đăng nhập/đăng xuất với route bảo vệ
3. **Project Store** — Zustand store cho CRUD dự án

Viết prompt cho mỗi phần, triển khai, sau đó kiểm tra luồng state.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ xây form với AI — xử lý validation, gửi và trạng thái lỗi.
