# Giai đoạn 3: Xây dựng Backend

## Động cơ phía sau TaskFlow

Frontend là bộ mặt của ứng dụng, nhưng backend là bộ não. Trong giai đoạn này, bạn sẽ xây API, database, hệ thống xác thực và tính năng thời gian thực cho TaskFlow.

## Thiết lập Express Server

### Cấu trúc dự án

"Giúp tôi tổ chức backend Express cho TaskFlow với cấu trúc thư mục sạch: routes, controllers, middleware, services và models."

AI sẽ tạo:
- Định nghĩa route cho mỗi tài nguyên
- Hàm controller với logic nghiệp vụ
- Middleware cho auth, xác thực và xử lý lỗi
- Service layer cho thao tác database
- Định nghĩa model với Prisma

### Xử lý lỗi

"Thiết lập xử lý lỗi tập trung cho Express API. Bao gồm lớp lỗi tùy chỉnh, bắt lỗi async và phản hồi lỗi nhất quán."

AI sẽ triển khai:
- Lớp AppError tùy chỉnh với mã trạng thái
- Async wrapper bắt promise rejections
- Middleware xử lý lỗi toàn cục
- Định dạng phản hồi lỗi nhất quán

## Thiết lập Database với Prisma

### Triển khai schema

"Giúp tôi triển khai schema Prisma cho TaskFlow dựa trên thiết kế. Bao gồm tất cả models, quan hệ và indexes."

AI sẽ tạo:
- Model User với trường auth
- Models Workspace và membership
- Models Board, column và task
- Models Comment và attachment
- Quan hệ đúng và xóa liên kết

### Thao tác database

"Viết hàm service Prisma cho TaskFlow: tạo không gian làm việc, thêm thành viên, tạo bảng, di chuyển task và thêm bình luận."

AI sẽ tạo thao tác database type-safe với:
- Hỗ trợ giao dịch cho thao tác nhiều bước
- Xử lý lỗi đúng
- Phân trang cho truy vấn danh sách
- Lọc và sắp xếp

## Hệ thống xác thực

### Triển khai JWT

"Triển khai xác thực JWT cho TaskFlow. Bao gồm đăng ký, đăng nhập, làm mới token và đặt lại mật khẩu."

AI sẽ xây:
- Đăng ký với băm mật khẩu (bcrypt)
- Đăng nhập với tạo JWT token
- Cơ chế làm mới token
- Đặt lại mật khẩu với xác minh email
- Middleware bảo vệ route

### Kiểm soát truy cập theo vai trò

"Triển khai kiểm soát truy cập theo vai trò cho TaskFlow. Người dùng có thể là admin không gian, thành viên hoặc người xem với quyền khác nhau."

AI sẽ tạo:
- Định nghĩa vai trò và ma trận quyền
- Middleware kiểm tra quyền
- Bảo vệ route dựa trên vai trò
- Hệ thống mời với gán vai trò

## Tính năng thời gian thực với Socket.io

### Thiết lập WebSocket

"Thiết lập Socket.io cho cập nhật thời gian thực trong TaskFlow. Người dùng thấy thay đổi trực tiếp khi đồng đội cập nhật bảng."

AI sẽ triển khai:
- Tích hợp Socket.io server với Express
- Messaging theo phòng (một phòng cho mỗi không gian)
- Xử lý sự kiện cho cập nhật, di chuyển và bình luận task
- Xác thực cho kết nối WebSocket

### Sự kiện thời gian thực

"Triển khai sự kiện thời gian thực cho TaskFlow: task được tạo, task di chuyển, task cập nhật, bình luận thêm và người dùng tham gia."

AI sẽ tạo:
- Event emitters cho mỗi hành động
- Phát sóng đến phòng phù hợp
- Hỗ trợ cập nhật lạc quan cho frontend
- Xử lý kết nối lại

## Hệ thống tải tệp

"Triển khai tải tệp cho TaskFlow. Người dùng có thể đính kèm tệp vào task với hỗ trợ xem trước."

AI sẽ xây:
- Multer middleware cho xử lý tệp
- Xác thực loại tệp và giới hạn kích thước
- Tích hợp lưu trữ cục bộ hoặc S3
- Tạo thumbnail cho hình ảnh
- Lưu metadata tệp vào database

## Xác thực API

"Thêm xác thực request cho tất cả endpoints API TaskFlow dùng Zod."

AI sẽ triển khai:
- Zod schemas cho mỗi endpoint
- Middleware xác thực
- Thông báo lỗi xác thực chi tiết
- Type inference cho TypeScript

## Sản phẩm Giai đoạn 3

- [ ] Express server với kiến trúc sạch
- [ ] Schema Prisma và thao tác database
- [ ] Hệ thống xác thực JWT
- [ ] Kiểm soát truy cập theo vai trò
- [ ] Tính năng thời gian thực Socket.io
- [ ] Hệ thống tải tệp
- [ ] Xác thực request với Zod
- [ ] Tài liệu API

## Điểm mấu chốt

AI xử lý phần lặp lại của phát triển backend — thao tác CRUD, code mẫu xác thực, schemas xác thực — trong khi bạn tập trung vào logic nghiệp vụ và quyết định kiến trúc. Kết quả là backend mạnh mẽ, có cấu trúc tốt sẵn sàng cho production.
