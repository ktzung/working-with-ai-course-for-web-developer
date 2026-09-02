# Prompt tạo Component

## Tạo Component với AI

Component là khối xây dựng của ứng dụng web hiện đại. Dù bạn dùng React, Vue hay Angular, AI có thể tạo component có cấu trúc tốt khi bạn cung cấp prompt đúng. Hãy cùng nắm vững kỹ năng tạo component.

## Prompt Component React

### Component cơ bản

```
Tạo component React tên UserAvatar dùng TypeScript.

Props:
- src: string (URL hình ảnh)
- alt: string (văn bản thay thế)
- size: 'sm' | 'md' | 'lg' (mặc định: 'md')
- showStatus: boolean (mặc định: false)
- status: 'online' | 'offline' | 'away'

Tính năng:
- Avatar hình tròn với viền
- Chấm trạng thái (góc dưới phải)
- Hiển thị chữ cái đầu nếu hình ảnh lỗi
- Hiệu ứng hover phóng to nhẹ

Styling: Tailwind CSS
File: src/components/ui/UserAvatar.tsx
Export là named export.
```

### Component phức tạp với State

```
Tạo component React tên SearchFilter dùng TypeScript.

Component này cung cấp thanh tìm kiếm với bộ lọc dropdown cho danh sách dự án.

Props:
- onFilterChange: (filters: FilterState) => void
- availableTags: string[]
- availableStatuses: string[]

State (dùng useReducer):
- searchQuery: string
- selectedTags: string[]
- selectedStatus: string
- sortBy: 'name' | 'date' | 'stars'
- sortOrder: 'asc' | 'desc'

Tính năng:
- Input tìm kiếm debounce (300ms)
- Bộ lọc tag đa chọn với checkbox
- Dropdown trạng thái đơn chọn
- Điều khiển sắp xếp với nút đảo chiều
- Nút xóa tất cả bộ lọc
- Badge đếm bộ lọc đang active

Styling: Tailwind CSS hỗ trợ dark mode
File: src/components/filters/SearchFilter.tsx
Bao gồm kiểu TypeScript trong file types riêng.
```

### Component tích hợp API

```
Tạo component React tên ProjectList dùng TypeScript.

Component này fetch và hiển thị danh sách dự án phân trang.

Props:
- initialPage?: number (mặc định: 1)
- pageSize?: number (mặc định: 12)
- filters?: FilterState

Tính năng:
- Fetch dự án từ /api/projects với phân trang
- Skeleton loading khi đang fetch
- Trạng thái lỗi với nút thử lại
- Trạng thái trống với hình minh họa
- Infinite scroll hoặc nút "Tải thêm"
- Card dự án trong lưới responsive (1 cột mobile, 2 tablet, 3 desktop)

Mỗi ProjectCard hiển thị:
- Ảnh thu nhỏ dự án
- Tiêu đề và mô tả
- Badge tech stack
- Số sao và ngày cập nhật
- Link đến chi tiết dự án

Styling: Tailwind CSS
File: src/components/projects/ProjectList.tsx
Dùng custom hook useProjects từ hooks/useProjects.ts
```

## Prompt Component Vue

### Component Vue cơ bản

```
Tạo component Vue 3 dùng Composition API và TypeScript.

Tên component: TechBadge
File: src/components/ui/TechBadge.vue

Props:
- name: string (tên công nghệ)
- icon?: string (class icon hoặc URL)
- color?: string (màu badge, mặc định theo tech)
- size: 'sm' | 'md' | 'lg'

Tính năng:
- Badge hình pill bo tròn với icon và chữ
- Mã màu theo công nghệ (React=xanh dương, Vue=xanh lá, etc.)
- Hiệu ứng hover phóng to nhẹ
- Click emit sự kiện 'select' với tên tech

Styling: Tailwind CSS với scoped style
Dùng cú pháp <script setup>.
```

### Component Vue phức tạp

```
Tạo component Vue 3 dùng Composition API và TypeScript.

Tên component: KanbanBoard
File: src/components/dashboard/KanbanBoard.vue

Props:
- projectId: string

Tính năng:
- Ba cột: Cần làm, Đang làm, Hoàn thành
- Kéo thả giữa các cột dùng @vueuse/core's useDraggable
- Thêm task mới với form inline
- Card task với tiêu đề, avatar người phụ trách, badge ưu tiên
- Đếm task mỗi cột
- Lưu thứ tự vào API khi thay đổi

Quản lý state: Pinia store
Styling: Tailwind CSS
Dùng <script setup> và defineProps/defineEmits.
```

## Pattern Component cần yêu cầu

### Compound Component
```
Tạo compound component tên Tabs với:
- Tabs.Container (quản lý state)
- Tabs.List (nút tab)
- Tabs.Tab (tab riêng lẻ)
- Tabs.Panels (vùng nội dung)
- Tabs.Panel (panel riêng lẻ)

Hỗ trợ điều hướng bàn phím (phím mũi tên, Home, End).
Dùng React Context để chia sẻ state giữa các sub-component.
```

### Render Props Component
```
Tạo component DataFetcher dùng pattern render props:
- url: string
- render: (data, loading, error) => ReactNode
- fallback?: ReactNode

Xử lý trạng thái loading, error và success.
Bao gồm logic thử lại và caching.
```

### Higher-Order Component
```
Tạo HOC withAuth:
- Bọc bất kỳ component nào
- Kiểm tra trạng thái xác thực
- Chuyển hướng đến /login nếu chưa đăng nhập
- Truyền dữ liệu user làm prop
- Giữ nguyên displayName của component
```

## Mẹo Prompt cho Component tốt hơn

### Cụ thể về Type
```
// ❌ Mơ hồ
Props: đối tượng user

// ✅ Cụ thể
Props: user: {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  role: 'admin' | 'member' | 'viewer';
  createdAt: Date;
}
```

### Mô tả tương tác
```
Khi người dùng nhấn nút "Xóa":
1. Hiển thị modal xác nhận
2. Nếu xác nhận, gọi DELETE /api/projects/:id
3. Hiển thị spinner loading trên nút
4. Thành công, xóa khỏi danh sách với hiệu ứng mờ dần
5. Lỗi, hiển thị thông báo toast
```

### Bao gồm accessibility
```
- Dùng HTML ngữ nghĩa (button, nav, main, etc.)
- Thêm aria-label cho nút chỉ có icon
- Hỗ trợ điều hướng bàn phím
- Quản lý focus cho modal
- Bao gồm phân cấp heading đúng
```

## Bài tập thực hành

Tạo các component sau cho WebDevHub:

1. **ProjectCard** — Hiển thị dự án với ảnh thu nhỏ, tiêu đề, tech stack và thống kê
2. **SnippetEditor** — Trình chỉnh sửa code với syntax highlighting và chọn ngôn ngữ
3. **StatsWidget** — Widget dashboard hiển thị thống kê dự án với biểu đồ

Viết prompt chi tiết cho mỗi component, sau đó tạo code với công cụ AI.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ học cách prompt AI để thiết kế API — tạo endpoint RESTful, schema GraphQL và tài liệu API.
