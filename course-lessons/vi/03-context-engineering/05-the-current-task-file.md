# File current-task.md

## Context tập trung cho thứ đang xây dựng

Trong khi `project-context.md` mô tả toàn bộ dự án, `current-task.md` tập trung AI vào thứ bạn đang xây dựng lúc này. Điều này ngăn AI gợi ý thay đổi ở phần không liên quan.

## Tại sao current-task.md quan trọng

Nếu thiếu focus task, AI có thể:
- Gợi ý refactor code bạn không đang làm
- Tạo tính năng xung đột với công việc hiện tại
- Bỏ sót yêu cầu cụ thể của task
- Tạo code không tích hợp với thay đổi hiện tại

## Cấu trúc

```markdown
# Task hiện tại: Xác thực người dùng

## Mục tiêu
Triển khai xác thực người dùng với NextAuth.js, bao gồm đăng nhập,
đăng ký và route bảo vệ.

## Yêu cầu
- [ ] Trang đăng nhập với email/mật khẩu
- [ ] Trang đăng ký với validation form
- [ ] Nhà cung cấp OAuth GitHub
- [ ] Nhà cung cấp OAuth Google
- [ ] Route dashboard bảo vệ
- [ ] Hồ sơ người dùng trong header
- [ ] Chức năng đăng xuất

## Chi tiết kỹ thuật

### File cần sửa
- `src/app/(auth)/login/page.tsx` — Trang đăng nhập
- `src/app/(auth)/register/page.tsx` — Trang đăng ký
- `src/app/(auth)/layout.tsx` — Layout xác thực
- `src/components/auth/LoginForm.tsx` — Component form đăng nhập
- `src/components/auth/RegisterForm.tsx` — Component form đăng ký
- `src/lib/auth.ts` — Cấu hình NextAuth
- `src/middleware.ts` — Bảo vệ route

### File cần tạo
- `src/app/api/auth/[...nextauth]/route.ts` — API route NextAuth
- `src/components/auth/ProtectedRoute.tsx` — Component bảo vệ route
- `src/hooks/useAuth.ts` — Hook xác thực

### Dependencies
- next-auth@4.x
- @next-auth/prisma-adapter
- bcrypt (mã hóa mật khẩu)

### Thay đổi database
- Thêm model Account (cho OAuth)
- Thêm model Session
- Thêm model VerificationToken
- Cập nhật model User với trường password

## Tiến độ hiện tại
- [x] Đã cài next-auth
- [x] Đã tạo schema Prisma
- [ ] Cấu hình NextAuth
- [ ] Trang đăng nhập
- [ ] Trang đăng ký
- [ ] Route bảo vệ

## Vấn đề đã biết
- Cần xử lý CSRF token đúng
- Mã hóa mật khẩu nên dùng bcrypt 12 vòng
- Phải validate định dạng email trước khi lưu

## Context cho AI
Khi tạo code cho task này:
- Dùng NextAuth.js v4 với App Router
- Theo pattern dự án hiện có trong project-context.md
- Dùng Prisma adapter cho session database
- Triển khai xử lý lỗi đúng
- Thêm trạng thái loading cho thao tác xác thực
```

## Cách sử dụng current-task.md

### Khi bắt đầu phiên
1. Mở `current-task.md` trong editor
2. Tham chiếu trong prompt đầu:
   ```
   Tôi đang làm xác thực người dùng. Đây là task hiện tại:
   [paste current-task.md]
   
   Giúp tôi triển khai cấu hình NextAuth.
   ```

### Trong quá trình phát triển
Cập nhật file khi làm việc:
- Đánh dấu yêu cầu đã hoàn thành
- Thêm vấn đề mới phát hiện
- Cập nhật chi tiết kỹ thuật

### Khi chuyển task
Tạo `current-task.md` mới hoặc cập nhật hiện có với focus mới.

## Mẫu task

### Triển khai tính năng
```markdown
# Task hiện tại: [Tên tính năng]

## Mục tiêu
[Mô tả một câu về thứ đang xây dựng]

## Yêu cầu
- [ ] Yêu cầu 1
- [ ] Yêu cầu 2
- [ ] Yêu cầu 3

## Chi tiết kỹ thuật
### File cần sửa
- file1.tsx — [thay đổi gì]
- file2.ts — [thay đổi gì]

### File cần tạo
- newfile.tsx — [mục đích]

### Dependencies
- package@version — [lý do]

## Tiến độ hiện tại
- [x] Bước đã hoàn thành
- [ ] Bước còn lại

## Context cho AI
[Hướng dẫn cụ thể cho AI]
```

### Sửa bug
```markdown
# Task hiện tại: Sửa [Mô tả bug]

## Vấn đề
[Đang xảy ra vs. Nên xảy ra]

## Các bước tái hiện
1. Đến [trang]
2. Nhấn [nút]
3. Thấy [lỗi]

## Hành vi mong đợi
[Nên xảy ra gì]

## Hành vi hiện tại
[Thực tế xảy ra gì]

## Chi tiết lỗi
```
[Paste thông báo lỗi/stack trace]
```

## Điều tra
- [ ] Kiểm tra [vùng 1]
- [ ] Kiểm tra [vùng 2]
- [ ] Kiểm tra [vùng 3]

## Context cho AI
[Bạn cần giúp gì]
```

### Refactor
```markdown
# Task hiện tại: Refactor [Vùng]

## Trạng thái hiện tại
[Mô tả triển khai hiện tại]

## Vấn đề
- Vấn đề 1
- Vấn đề 2
- Vấn đề 3

## Trạng thái mục tiêu
[Mô tả triển khai mong muốn]

## File bị ảnh hưởng
- file1.tsx — [cần thay đổi]
- file2.ts — [cần thay đổi]

## Ràng buộc
- Phải giữ tương thích ngược
- Không thể thay đổi API công khai
- Phải vượt qua test hiện có

## Context cho AI
[Bạn cần giúp gì]
```

## Kết hợp với project-context.md

Dùng cả hai file cùng nhau:

```
Context dự án: [paste phần liên quan từ project-context.md]
Task hiện tại: [paste current-task.md]

Tạo cấu hình NextAuth khớp pattern dự án
và triển khai yêu cầu trong task hiện tại.
```

## Bài tập thực hành

1. Tạo `current-task.md` cho tính năng đang làm
2. Dùng nó trong cuộc hội thoại với AI
3. Cập nhật khi tiến triển
4. Nhận ra nó giữ AI tập trung vào task thực tế thế nào

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ thiết lập workspace phát triển hoàn chỉnh — cấu trúc thư mục, Git workflow và tất cả file context hoạt động cùng nhau.
