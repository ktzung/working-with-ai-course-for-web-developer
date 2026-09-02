# Form và Validation với AI

## Xây dựng Form vững chắc

Form có mặt khắp nơi trong ứng dụng web. AI có thể giúp bạn xây form accessible, có validation và mang lại trải nghiệm tuyệt vời.

## React Hook Form + Zod

### Form đăng ký

```
Tạo form đăng ký dùng React Hook Form và Zod:

Trường:
- name: string (bắt buộc, tối thiểu 2 ký tự)
- email: string (bắt buộc, định dạng email hợp lệ)
- password: string (bắt buộc, tối thiểu 8 ký tự, phải có chữ hoa, thường, số)
- confirmPassword: string (phải khớp password)
- role: 'developer' | 'designer' | 'manager' (bắt buộc)
- acceptTerms: boolean (phải là true)

Tính năng:
- Validation thời gian thực khi blur
- Thông báo lỗi dưới mỗi trường
- Thanh chỉ báo độ mạnh mật khẩu
- Nút hiện/ẩn mật khẩu
- Nút gửi disabled khi đang gửi
- Spinner loading khi gửi
- Hiển thị lỗi server
- Chuyển hướng thành công

Schema Zod:
const registerSchema = z.object({
  name: z.string().min(2, 'Tên phải có ít nhất 2 ký tự'),
  email: z.string().email('Địa chỉ email không hợp lệ'),
  password: z.string()
    .min(8, 'Mật khẩu phải có ít nhất 8 ký tự')
    .regex(/[A-Z]/, 'Phải có chữ hoa')
    .regex(/[a-z]/, 'Phải có chữ thường')
    .regex(/[0-9]/, 'Phải có số'),
  confirmPassword: z.string(),
  role: z.enum(['developer', 'designer', 'manager']),
  acceptTerms: z.literal(true, {
    errorMap: () => ({ message: 'Bạn phải chấp nhận điều khoản' }),
  }),
}).refine((data) => data.password === data.confirmPassword, {
  message: 'Mật khẩu không khớp',
  path: ['confirmPassword'],
});

Dùng @hookform/resolvers/zod để tích hợp.
File: components/auth/RegisterForm.tsx
```

### Form nhiều bước

```
Tạo form nhiều bước cho tạo dự án:

Bước 1: Thông tin cơ bản
- name: string (bắt buộc)
- description: string (tùy chọn, tối đa 500 ký tự)
- visibility: 'public' | 'private'

Bước 2: Chi tiết kỹ thuật
- techStack: string[] (ít nhất một)
- repository: string (URL hợp lệ, tùy chọn)
- deploymentUrl: string (URL hợp lệ, tùy chọn)

Bước 3: Thiết lập nhóm
- inviteEmails: string[] (email hợp lệ)
- defaultRole: 'member' | 'viewer'

Tính năng:
- Chỉ báo bước hiển thị tiến trình
- Điều hướng Quay lại/Tiếp
- Validation mỗi bước (không thể tiếp nếu không hợp lệ)
- Dữ liệu giữ nguyên khi điều hướng giữa các bước
- Xem lại tóm tắt trước khi gửi
- Gửi tất cả dữ liệu cùng lúc

Dùng React Hook Form với Zod.
Dùng component stepper cho điều hướng.
File: components/projects/CreateProjectForm.tsx
```

## Pattern Form

### Trường động

```
Tạo form với trường động cho tech stack:

Tính năng:
- Thêm/xóa mục tech stack
- Gợi ý autocomplete từ danh sách có sẵn
- Tối đa 20 mục
- Kéo để sắp xếp lại
- Mỗi mục có: tên, trình độ (1-5 năm), kinh nghiệm

Triển khai:
- useFieldArray từ React Hook Form
- Component autocomplete tùy chỉnh
- Kéo thả với @dnd-kit/core
- Validation cho mỗi trường

File: components/projects/TechStackForm.tsx
```

### Upload file

```
Tạo form upload file cho ảnh chụp dự án:

Tính năng:
- Vùng kéo thả
- Nhấn để duyệt file
- Xem trước khi upload
- Hỗ trợ nhiều file (tối đa 5)
- Kiểm tra loại file (jpg, png, webp)
- Kiểm tra kích thước file (tối đa 5MB mỗi file)
- Thanh tiến trình upload
- Xóa file đã upload

Triển khai:
- React Hook Form cho state form
- Custom hook useFileUpload
- Xem trước với FileReader API
- Tiến trình với XMLHttpRequest

File: components/projects/ScreenshotUpload.tsx
```

## Chiến lược Validation

### Validation phía client

```
Triển khai validation phía client cho tất cả form:

Quy tắc:
- Trường bắt buộc: Hiện lỗi khi blur nếu trống
- Email: Validate định dạng khi blur
- Mật khẩu: Chỉ báo độ mạnh thời gian thực
- Xác nhận mật khẩu: Validate khi thay đổi một trong hai trường
- Độ dài tối thiểu/tối đa: Hiển thị đếm ký tự
- Quy tắc tùy chỉnh: Validate khi blur

Hiển thị lỗi:
- Viền đỏ trên trường không hợp lệ
- Thông báo lỗi dưới trường
- Chỉ báo icon (dấu tích hoặc X)
- Thông báo cho trình đọc màn hình

Dùng schema Zod cho tất cả validation.
```

### Validation phía server

```
Xử lý lỗi validation phía server:

Khi server trả về lỗi validation:
1. Phân tích response lỗi
2. Ánh xạ lỗi trường vào form
3. Hiển thị lỗi bên cạnh trường liên quan
4. Focus trường không hợp lệ đầu tiên
5. Hiển thị thông báo lỗi chung nếu không có lỗi cụ thể

Định dạng lỗi từ server:
{
  error: {
    code: 'VALIDATION_ERROR',
    message: 'Validation thất bại',
    details: [
      { field: 'email', message: 'Email đã tồn tại' },
      { field: 'name', message: 'Tên là bắt buộc' }
    ]
  }
}

Dùng setError từ React Hook Form để đặt lỗi server.
```

## Accessibility cho Form

```
Đảm bảo tất cả form accessible:

Yêu cầu:
- Tất cả input có label liên kết
- Thông báo lỗi liên kết với aria-describedby
- Trường bắt buộc đánh dấu aria-required
- Trường không hợp lệ đánh dấu aria-invalid
- Quản lý focus sau lỗi
- Điều hướng bàn phím hoạt động
- Thông báo cho trình đọc màn hình khi thành công/lỗi

Triển khai:
- Dùng htmlFor trên label
- Dùng aria-describedby cho thông báo lỗi
- Dùng role="alert" cho thông báo lỗi
- Quản lý focus với useRef
```

## Component Form

### Component Input

```
Tạo component Input tái sử dụng:

Props:
- label: string
- name: string
- type?: string
- placeholder?: string
- error?: string
- helpText?: string
- isRequired?: boolean
- isDisabled?: boolean
- leftIcon?: ReactNode
- rightIcon?: ReactNode

Tính năng:
- Label với chỉ báo bắt buộc
- Trạng thái lỗi với viền đỏ và thông báo
- Text trợ giúp dưới input
- Hỗ trợ icon
- Focus ring
- Trạng thái disabled

Tích hợp với React Hook Form dùng Controller.
File: components/ui/Input.tsx
```

### Component Select

```
Tạo component Select tái sử dụng:

Props:
- label: string
- name: string
- options: Array<{ value: string; label: string; disabled?: boolean }>
- placeholder?: string
- error?: string
- isRequired?: boolean
- isMulti?: boolean

Tính năng:
- Dropdown tùy chỉnh
- Tìm kiếm/lọc tùy chọn
- Đa chọn với tag
- Điều hướng bàn phím
- Tùy chọn disabled
- Nút xóa

Dùng @headlessui/listbox cho accessibility.
File: components/ui/Select.tsx
```

## Bài tập thực hành

Xây các form sau cho WebDevHub:

1. **Form đăng nhập** — Email/mật khẩu với ghi nhớ
2. **Form chỉnh sửa dự án** — Form nhiều phần với validation
3. **Form hồ sơ** — Upload avatar, tiểu sử, link mạng xã hội

Viết prompt chi tiết, triển khai với React Hook Form + Zod, sau đó kiểm tra.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ kết hợp mọi thứ bằng cách xây trang UI hoàn chỉnh cho WebDevHub.
