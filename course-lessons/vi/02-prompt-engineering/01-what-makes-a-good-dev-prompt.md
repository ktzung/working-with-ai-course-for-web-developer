# Thế nào là một Developer Prompt tốt?

## Cấu trúc của Prompt hiệu quả

Không phải prompt nào cũng như nhau. Prompt mơ hồ kiểu "tạo component nút" sẽ cho code chung chung. Prompt được cấu trúc tốt sẽ cho đúng thứ bạn cần. Hãy phân tích cấu trúc của một developer prompt tuyệt vời.

## Bốn yếu tố

Mỗi developer prompt hiệu quả chứa bốn yếu tố:

### 1. Context
Cho AI biết về dự án, tech stack và code hiện có.

### 2. Task
Mô tả rõ ràng điều bạn muốn xây dựng hoặc hoàn thành.

### 3. Tech Stack
Chỉ định chính xác công nghệ, phiên bản và pattern đang dùng.

### 4. Constraints
Định nghĩa giới hạn, yêu cầu và tiêu chuẩn chất lượng.

## Công thức

```
[Context] + [Task] + [Tech Stack] + [Constraints] = Code tuyệt vời
```

## Ví dụ: Tệ vs. Tốt

### Ví dụ 1: Tạo Button

**Prompt tệ:**
```
Tạo component nút
```

**Prompt tốt:**
```
Tôi đang xây app Next.js 14 với TypeScript và Tailwind CSS.

Tạo component Button tái sử dụng với yêu cầu sau:
- Variants: primary, secondary, outline, ghost
- Kích thước: sm, md, lg
- Hỗ trợ trạng thái loading với spinner
- Hỗ trợ icon (trái và phải)
- Trạng thái disabled
- Dùng Tailwind CSS để styling
- Export là named export từ components/ui/Button.tsx
- Bao gồm TypeScript interface cho props
```

### Ví dụ 2: API Route

**Prompt tệ:**
```
Tạo API endpoint
```

**Prompt tốt:**
```
Tôi dùng Next.js 14 App Router với Prisma và PostgreSQL.

Tạo API route tại /api/projects:
- GET: Trả về danh sách dự án phân trang (10 mỗi trang) với tìm kiếm theo tên
- POST: Tạo dự án mới với validation (tên bắt buộc, mô tả tùy chọn, techStack là mảng string)
- Xử lý lỗi đúng với HTTP status code phù hợp
- Dùng Zod cho validation request
- Bao gồm kiểu TypeScript cho request/response
- Thêm rate limiting (100 request mỗi phút mỗi user)
```

### Ví dụ 3: Custom Hook

**Prompt tệ:**
```
Tạo hook để fetch dữ liệu
```

**Prompt tốt:**
```
Tôi dùng React 18 với TypeScript và Zustand cho state management.

Tạo custom hook tên useProjects:
- Fetch dự án từ /api/projects
- Hỗ trợ phân trang (tham số page, limit)
- Bao gồm trạng thái loading, error và success
- Cache kết quả trong Zustand store
- Hỗ trợ refetch và invalidation
- Xử lý lỗi mạng nhẹ nhàng
- Trả về dữ liệu có kiểu: { projects: Project[], total: number, page: number }
- Bao gồm abort controller để cleanup
```

## Pattern Prompt cho phát triển Web

### Pattern Component
```
Tạo component [TênComponent] cho [framework] với:
- Props: [liệt kê props kèm kiểu]
- Tính năng: [liệt kê tính năng]
- Styling: [cách tiếp cận]
- File: [vị trí]
```

### Pattern API
```
Tạo API route tại [đường dẫn] dùng [framework]:
- [Method]: [mô tả]
- Validation: [thư viện]
- Xử lý lỗi: [cách tiếp cận]
- Auth: [yêu cầu]
```

### Pattern Hook
```
Tạo custom hook tên [tênHook]:
- [Chức năng chính]
- Quản lý state: [cách tiếp cận]
- Xử lý lỗi: [cách tiếp cận]
- Trả về: [định nghĩa kiểu]
```

### Pattern Fix
```
Tôi gặp lỗi này trong [file]:
[paste lỗi]

Đây là code liên quan:
[paste code]

Tech stack: [stack]
Đã thử: [các cách đã thử]
```

## Kỹ thuật Prompt nâng cao

### Chỉ định điều KHÔNG nên làm
```
Tạo component form:
- Dùng React Hook Form (KHÔNG dùng Formik)
- Validate bằng Zod (KHÔNG dùng Yup)
- Dùng controlled component (KHÔNG dùng uncontrolled)
- KHÔNG dùng thư viện UI ngoài
```

### Tham chiếu code hiện có
```
Theo cùng pattern với component UserProfile hiện có 
(tại src/components/UserProfile.tsx), tạo component ProjectCard 
hiển thị thông tin dự án với cách styling tương tự.
```

### Bao gồm edge case
```
Tạo hàm định dạng ngày:
- Xử lý ngày null/undefined
- Hỗ trợ nhiều định dạng (ISO, US, EU)
- Xử lý chuyển đổi múi giờ
- Trả về "Invalid Date" cho input sai định dạng
- Hoạt động với cả đối tượng Date và chuỗi ngày
```

## Bài tập thực hành

Lấy prompt mơ hồ này và làm cho nó cụ thể:

**Mơ hồ**: "Tạo tính năng tìm kiếm"

**Bài tập**: Viết lại bằng bốn yếu tố (Context, Task, Tech Stack, Constraints). Cân nhắc:
- Bạn dùng framework gì?
- Tìm kiếm cái gì? (dự án, người dùng, snippet?)
- Kết quả hiển thị thế nào?
- Trạng thái loading?
- Debouncing? Phím tắt?
```

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ đi sâu vào pattern prompt cụ thể để tạo component React và Vue — khối xây dựng của ứng dụng web hiện đại.
