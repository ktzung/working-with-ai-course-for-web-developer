# Giai đoạn 2: Xây dựng Frontend

## Từ thiết kế đến components

Với dự án đã lập kế hoạch và thiết lập, đã đến lúc xây frontend. Đây là nơi AI tỏa sáng — tạo components, viết styles và triển khai tương tác phức tạp.

## Thiết lập nền tảng UI

### Hệ thống thiết kế

"Giúp tôi tạo hệ thống thiết kế cho TaskFlow dùng Tailwind CSS. Tôi cần bảng màu, thang typographic, hệ thống khoảng cách và biến thể component."

AI sẽ tạo:
- Cấu hình Tailwind tùy chỉnh với màu thương hiệu
- Classes typographic cho tiêu đề, văn bản và phần tử UI
- Tiện ích khoảng cách và layout
- Biến thể component nút, input và card

### Components layout

"Tạo layout chính cho TaskFlow với thanh điều hướng bên, header và vùng nội dung. Cần responsive."

AI sẽ xây:
- Thanh bên có thể thu gọn với liên kết điều hướng
- Header với menu người dùng và thông báo
- Vùng nội dung responsive thích ứng kích thước màn hình

## Xây components cốt lõi

### Trang xác thực

"Xây trang đăng nhập và đăng ký cho TaskFlow. Bao gồm xác thực form, xử lý lỗi và trạng thái tải."

AI sẽ tạo:
- Form đăng nhập với trường email/mật khẩu
- Form đăng ký với xác thực
- Luồng đặt lại mật khẩu
- Wrapper route được bảo vệ

### Bảng Kanban

"Xây component bảng kanban với kéo thả. Cột có thể sắp xếp lại, task có thể kéo giữa các cột."

Đây là nơi AI thực sự giúp với logic phức tạp:
- Kéo thả dùng thư viện @dnd-kit
- Sắp xếp lại cột
- Thẻ task với chỉ báo ưu tiên
- Animation mượt

### Modal chi tiết task

"Tạo modal chi tiết task với tiêu đề, mô tả, người được giao, ngày hạn, nhãn và phần bình luận."

AI sẽ xây:
- Modal với trường form
- Tích hợp chọn ngày
- Dropdown chọn người dùng
- Chuỗi bình luận với cập nhật thời gian thực

## Quản lý state

### Thiết lập state toàn cục

"Thiết lập Zustand cho quản lý state TaskFlow. Tôi cần stores cho auth, không gian làm việc, bảng và state UI."

AI sẽ tạo:
- Auth store với hành động đăng nhập/đăng xuất
- Workspace store với thao tác CRUD
- Board store với đồng bộ thời gian thực
- UI store cho modal, thanh bên và tùy chọn

### Lấy dữ liệu

"Thiết lập React Query cho lấy dữ liệu API trong TaskFlow. Bao gồm caching, cập nhật lạc quan và xử lý lỗi."

AI sẽ cấu hình:
- Query hooks cho mỗi tài nguyên
- Mutation hooks với cập nhật lạc quan
- Chiến lược vô hiệu hóa cache
- Tích hợp error boundary

## Thiết kế responsive

"Làm bảng kanban TaskFlow hoạt động trên mobile. Trên màn hình nhỏ, cột nên xếp dọc với điều hướng vuốt."

AI sẽ triển khai:
- Breakpoints responsive mobile-first
- Kéo thả thân thiện cảm ứng
- Cử chỉ vuốt cho điều hướng cột
- Thanh bên thu gọn trên mobile

## Animation và hoàn thiện

"Thêm animation mượt cho TaskFlow. Thẻ task nên animate khi di chuyển, modal có animation vào/ra, thanh bên nên trượt."

AI sẽ dùng Framer Motion cho:
- Animation kéo
- Chuyển đổi modal
- Chuyển đổi trang
- Tương tác vi mô

## Sản phẩm Giai đoạn 2

- [ ] Hệ thống thiết kế với cấu hình Tailwind
- [ ] Components layout (thanh bên, header, nội dung)
- [ ] Trang xác thực
- [ ] Bảng kanban với kéo thả
- [ ] Modal chi tiết task
- [ ] Quản lý state với Zustand
- [ ] Lấy dữ liệu với React Query
- [ ] Thiết kế responsive
- [ ] Animation và chuyển đổi

## Điểm mấu chốt

AI tăng tốc phát triển frontend bằng cách tạo code mẫu, triển khai tương tác phức tạp và xử lý thiết kế responsive. Việc của bạn là hướng dẫn AI, kiểm tra đầu ra và đảm bảo mọi thứ hoạt động cùng nhau.
