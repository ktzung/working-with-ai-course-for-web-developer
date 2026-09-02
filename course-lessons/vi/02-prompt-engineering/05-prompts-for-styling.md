# Prompt cho Styling

## Styling với AI

Thiết kế đẹp, responsive là yếu tố thiết yếu cho ứng dụng web hiện đại. AI có thể tạo style ấn tượng khi bạn mô tả rõ ràng. Hãy nắm vững prompt styling cho Tailwind CSS và CSS hiện đại.

## Prompt Tailwind CSS

### Styling Component

```
Style component card React này dùng Tailwind CSS:

Yêu cầu:
- Thiết kế card sạch, hiện đại với shadow nhẹ
- Bo góc (lg)
- Hiệu ứng hover: nâng nhẹ với shadow tăng
- Padding responsive: nhỏ trên mobile, lớn trên desktop
- Hỗ trợ dark mode
- Phần hình ảnh ở trên với tỷ lệ 16:9
- Vùng nội dung với tiêu đề, mô tả và metadata
- Footer với nút hành động

Bảng màu: Xanh dương chính (#3B82F6), xám trung tính
Kiểu chữ: Sans-serif sạch, phân cấp tốt
```

### Layout Dashboard

```
Tạo layout dashboard responsive dùng Tailwind CSS:

Cấu trúc:
- Sidebar cố định (280px trên desktop, ẩn trên mobile với nút hamburger)
- Header trên cùng với thanh tìm kiếm và menu người dùng
- Vùng nội dung chính có cuộn
- Panel phải (tùy chọn, ẩn trên tablet/mobile)

Sidebar:
- Nền tối (slate-900)
- Logo ở trên
- Mục điều hướng với icon
- Trạng thái active với highlight
- Thu nhỏ chỉ còn icon trên màn hình nhỏ hơn

Header:
- Nền trắng với viền dưới
- Input tìm kiếm (căn giữa, max-width 600px)
- Avatar người dùng và dropdown (phải)
- Chuông thông báo với badge

Breakpoint responsive:
- Mobile: < 768px (sidebar ẩn, nội dung full-width)
- Tablet: 768px - 1024px (sidebar thu gọn, panel phải ẩn)
- Desktop: > 1024px (layout đầy đủ)
```

### Styling Form

```
Style form đăng ký dùng Tailwind CSS:

Trường:
- Họ tên (input text)
- Email (input email)
- Mật khẩu (có nút hiện/ẩn)
- Xác nhận mật khẩu
- Chọn vai trò (dropdown)
- Checkbox điều khoản
- Nút gửi

Yêu cầu thiết kế:
- Layout card căn giữa với max-width 480px
- Style input sạch với focus ring
- Trạng thái lỗi với viền đỏ và thông báo
- Trạng thái thành công với dấu tích xanh
- Trạng thái loading trên nút gửi
- Nút đăng nhập mạng xã hội (Google, GitHub)
- Link đến trang đăng nhập

Accessibility:
- Liên kết label đúng
- Trạng thái focus rõ ràng
- Thông báo lỗi liên kết với input
```

## Prompt Animation

### Micro-interaction

```
Thêm các micro-interaction sau dùng Tailwind CSS và Framer Motion:

1. Hover nút: Scale 1.02, shadow tăng, transition 200ms
2. Card xuất hiện: Fade in + slide lên, cách nhau 100ms giữa các card
3. Mở modal: Nền mờ + nội dung scale từ 0.95
4. Thông báo toast: Trượt vào từ phải, tự ẩn sau 3 giây
5. Skeleton loading: Nhấp nháy
6. Chuyển tab: Animation gạch chân trượt
7. Dropdown: Fade + trượt xuống, 150ms

Dùng tiện ích transition của Tailwind khi có thể.
Dùng Framer Motion cho animation phức tạp.
Giữ animation tinh tế và hiệu quả.
```

### Chuyển trang

```
Tạo chuyển trang cho app Next.js:

- Trang vào: Fade in + chuyển động nhẹ lên (300ms)
- Trang ra: Fade out + chuyển động nhẹ xuống (200ms)
- Trạng thái loading: Skeleton khớp layout trang
- Thay đổi route: Thanh tiến trình ở đầu trang

Dùng AnimatePresence của Framer Motion.
Xử lý cả tải lần đầu và chuyển hướng.
Đảm bảo animation không chặn tương tác.
```

## Prompt Responsive Design

### Cách tiếp cận Mobile-First

```
Chuyển thiết kế desktop sang mobile-first dùng Tailwind CSS:

Layout desktop:
- Lưới 3 cột cho card dự án
- Điều hướng sidebar
- Header ngang với tất cả điều khiển

Yêu cầu mobile:
- Xếp đơn cột
- Thanh điều hướng dưới cùng
- Header thu gọn với menu hamburger
- Vùng chạm tối thiểu (44px)
- Vuốt cho hành động card
- Kéo để làm mới trên danh sách

Tablet (768px+):
- Lưới 2 cột
- Sidebar có thể thu gọn
- Header đầy đủ

Dùng tiền tố responsive Tailwind (sm:, md:, lg:, xl:).
```

### Thang chữ

```
Tạo hệ thống chữ responsive dùng Tailwind CSS:

Tiêu đề:
- h1: 2.5rem mobile, 3.5rem desktop
- h2: 2rem mobile, 2.75rem desktop
- h3: 1.5rem mobile, 2rem desktop
- h4: 1.25rem mobile, 1.5rem desktop

Nội dung:
- Lớn: 1.125rem
- Cơ bản: 1rem
- Nhỏ: 0.875rem
- Chú thích: 0.75rem

Chiều cao dòng:
- Tiêu đề: 1.2
- Nội dung: 1.6
- Chặt: 1.3

Độ dày chữ:
- Tiêu đề: 700
- Nội dung: 400
- Nhấn mạnh: 600

Dùng tiện ích text- của Tailwind với biến thể responsive.
```

## Prompt Theme và Design System

### Bảng màu

```
Tạo bảng màu cho trang portfolio lập trình viên:

Chính: Xanh dương (cho CTA, link)
Phụ: Tím (cho điểm nhấn, badge)
Thành công: Xanh lá (cho trạng thái, xác nhận)
Cảnh báo: Vàng hổ phách (cho cảnh báo, trạng thái chờ)
Lỗi: Đỏ (cho lỗi, hành động phá hủy)
Trung tính: Slate (cho chữ, nền, viền)

Cho mỗi màu, cung cấp:
- Shade từ 50 đến 950
- Cách dùng khuyến nghị cho mỗi shade
- Biến thể dark mode
- Tỷ lệ tương phản_accessible_

Định dạng dưới dạng mở rộng config Tailwind CSS.
```

### Design Token

```
Tạo hệ thống design token cho WebDevHub:

Màu sắc:
- Màu thương hiệu với shade
- Màu ngữ nghĩa (thành công, cảnh báo, lỗi, thông tin)
- Bảng màu trung tính cho chữ và nền

Khoảng cách:
- Thang khoảng cách nhất quán (cơ sở 4px)
- Khoảng cách riêng cho component

Kiểu chữ:
- Font family (tiêu đề, nội dung, mono)
- Kích thước chữ với chiều cao dòng
- Độ dày chữ

Bóng:
- Biến thể sm, md, lg, xl
- Bóng màu cho nhấn mạnh

Bo góc:
- Biến thể sm, md, lg, full

Định dạng dưới dạng mở rộng theme Tailwind CSS.
```

## Prompt CSS cụ thể

### CSS Custom Properties

```
Tạo hệ thống CSS custom properties cho theming:

:root {
  /* Tạo các biến này */
  --color-primary-50 đến --color-primary-900
  --color-secondary-50 đến --color-secondary-900
  --color-neutral-50 đến --color-neutral-900
  --spacing-1 đến --spacing-16
  --font-size-xs đến --font-size-4xl
  --shadow-sm đến --shadow-2xl
  --radius-sm đến --radius-full
}

Bao gồm ghi đè dark mode dùng prefers-color-scheme.
```

### Grid Layout

```
Tạo layout CSS Grid cho:

1. Grid dashboard: Card auto-fit với min-width 300px
2. Layout blog: Nội dung + sidebar với sidebar dính
3. Thư viện ảnh: Grid ảnh kiểu masonry
4. Bảng giá: 3 cột card bằng nhau

Dùng CSS Grid với tiện ích Tailwind.
Đảm bảo layout hoạt động không cần JavaScript.
```

## Bài tập thực hành

Style các component WebDevHub:

1. **ProjectCard** — Card hiện đại với hình ảnh, hiệu ứng hover, dark mode
2. **Dashboard** — Layout dashboard responsive hoàn chỉnh
3. **Trang đăng nhập** — Form đăng nhập sạch, căn giữa với trạng thái validation

Viết prompt styling chi tiết, sau đó triển khai với công cụ AI.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ đề cập những lỗi prompt thường gặp của lập trình viên web và cách tránh.
