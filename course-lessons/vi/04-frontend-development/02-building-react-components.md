# Xây dựng Component React với AI

## Từ Prompt đến Component Production

Hãy xây component React thật cho WebDevHub dùng AI. Từ thành phần UI đơn giản đến tính năng tương tác phức tạp.

## Component UI đơn giản

### Component Button

```
Tạo component Button cho WebDevHub dùng React 18, TypeScript và Tailwind CSS.

Variants:
- primary: Nền xanh dương, chữ trắng
- secondary: Nền xám, chữ tối
- outline: Trong suốt với viền
- ghost: Trong suốt, không viền
- danger: Nền đỏ, chữ trắng

Kích thước: sm, md, lg

Trạng thái:
- Mặc định
- Hover (tối nhẹ)
- Active (hiệu ứng nhấn)
- Disabled (giảm độ mờ, không pointer)
- Loading (spinner thay icon/chữ)

Props:
- variant: ButtonVariant
- size: ButtonSize
- isLoading?: boolean
- isDisabled?: boolean
- leftIcon?: ReactNode
- rightIcon?: ReactNode
- children: ReactNode
- onClick?: () => void
- type?: 'button' | 'submit' | 'reset'
- className?: string

Dùng forwardRef để truyền ref.
Bao gồm thuộc tính aria đúng.
Export là named export từ components/ui/Button.tsx.
```

### Component Card

```
Tạo component Card dùng React 18, TypeScript và Tailwind CSS.

Sub-component:
- Card (wrapper)
- Card.Header (phần trên với tiêu đề và hành động)
- Card.Body (nội dung chính)
- Card.Footer (phần dưới với hành động)

Tính năng:
- Hiệu ứng hover (tăng shadow nhẹ)
- Xử lý click (tùy chọn)
- Biến thể padding (sm, md, lg)
- Tùy chọn viền (none, light, full)
- Hỗ trợ dark mode

Props cho Card:
- padding?: 'sm' | 'md' | 'lg'
- border?: 'none' | 'light' | 'full'
- isClickable?: boolean
- onClick?: () => void
- className?: string
- children: ReactNode

Dùng pattern compound component với dot notation.
Export từ components/ui/Card.tsx.
```

## Component tương tác phức tạp

### Search Input với Autocomplete

```
Tạo component SearchInput với gợi ý tự động hoàn thành.

Props:
- placeholder?: string
- onSearch: (query: string) => void
- onSelect: (item: SearchResult) => void
- suggestions: SearchResult[]
- isLoading?: boolean
- debounceMs?: number (mặc định: 300)

Tính năng:
- Input debounce (trễ có thể cấu hình)
- Dropdown danh sách gợi ý
- Điều hướng bàn phím (mũi tên lên/xuống, enter để chọn, escape để đóng)
- Spinner loading khi tìm kiếm
- Nút xóa
- Tìm kiếm gần đây (lưu trong localStorage)
- Highlight chữ khớp trong gợi ý

Kiểu SearchResult:
interface SearchResult {
  id: string;
  title: string;
  description?: string;
  icon?: ReactNode;
}

Accessibility:
- aria-expanded cho dropdown
- aria-activedescendant cho điều hướng bàn phím
- role="listbox" cho gợi ý
- role="option" cho mỗi gợi ý

Styling: Tailwind CSS với dark mode
File: components/ui/SearchInput.tsx
```

### Modal với Form

```
Tạo component Modal có thể chứa form.

Props:
- isOpen: boolean
- onClose: () => void
- title: string
- size?: 'sm' | 'md' | 'lg' | 'xl'
- children: ReactNode
- footer?: ReactNode

Tính năng:
- Click nền để đóng
- Phím Escape để đóng
- Bẫy focus (tab xoay vòng trong modal)
- Khóa cuộn body khi mở
- Animation vào/ra (mờ + scale)
- Nút đóng trong header
- Responsive (toàn màn hình trên mobile)

Tích hợp form:
- Tự động focus input đầu khi mở
- Ngăn đóng khi click nền nếu form có thay đổi
- Hiển thị dialog xác nhận nếu form có thay đổi chưa lưu

Accessibility:
- role="dialog"
- aria-modal="true"
- aria-labelledby cho tiêu đề
- Trả focus về phần tử kích hoạt khi đóng

Dùng React Portal để render.
File: components/ui/Modal.tsx
```

## Component hiển thị dữ liệu

### Bảng dữ liệu

```
Tạo component DataTable hiển thị dữ liệu dự án.

Props:
- data: T[]
- columns: Column<T>[]
- isLoading?: boolean
- onRowClick?: (row: T) => void
- sortable?: boolean
- selectable?: boolean
- onSelectionChange?: (selected: T[]) => void

Kiểu Column:
interface Column<T> {
  key: keyof T;
  header: string;
  render?: (value: T[keyof T], row: T) => ReactNode;
  sortable?: boolean;
  width?: string;
}

Tính năng:
- Cột sắp xếp được (click header để sắp xếp)
- Chọn hàng (checkbox)
- Skeleton loading
- Trạng thái trống
- Responsive (cuộn ngang trên mobile)
- Header dính

Styling: Tailwind CSS
File: components/ui/DataTable.tsx
```

## Mẹo Prompt cho Component React

### Chỉ định API Component
```
// ❌ Mơ hồ
Tạo component dropdown

// ✅ Cụ thể
Tạo component Dropdown với:
- trigger: ReactNode (nút mở dropdown)
- items: Array<{ label: string, onClick: () => void, icon?: ReactNode }>
- placement: 'bottom-start' | 'bottom-end' | 'top-start' | 'top-end'
- closeOnSelect: boolean (mặc định: true)
```

### Bao gồm kiểu TypeScript
```
Bao gồm các kiểu TypeScript sau:

interface ProjectCardProps {
  project: Project;
  variant: 'default' | 'compact' | 'featured';
  onSelect?: (id: string) => void;
  onEdit?: (id: string) => void;
  onDelete?: (id: string) => void;
}

interface Project {
  id: string;
  name: string;
  description: string;
  techStack: string[];
  status: 'active' | 'archived' | 'draft';
  stars: number;
  updatedAt: Date;
}
```

### Mô tả tương tác
```
Khi người dùng nhấn nút sao:
1. Cập nhật lạc quan số sao (+1)
2. Gọi POST /api/projects/:id/star
3. Nếu API thất bại, hoàn nguyên số sao và hiện toast lỗi
4. Nếu người dùng nhấn lại, bỏ sao (POST /api/projects/:id/unstar)
```

## Bài tập thực hành

Xây các component sau cho WebDevHub:

1. **ProjectCard** — Card với hình ảnh, tiêu đề, badge tech stack, số sao
2. **FilterBar** — Tìm kiếm + bộ lọc với pill bộ lọc active
3. **ActivityFeed** — Danh sách hoạt động gần đây với thời gian

Viết prompt chi tiết, tạo code, sau đó review và tinh chỉnh.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ xây component Vue với AI — cho thấy nguyên tắc tương tự áp dụng trên nhiều framework.
