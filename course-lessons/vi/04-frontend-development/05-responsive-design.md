# Responsive Design

## Mobile-First với AI

Responsive design đảm bảo ứng dụng hoạt động đẹp trên mọi thiết bị. AI có thể giúp bạn triển khai thiết kế mobile-first thích ứng mượt mà với mọi kích thước màn hình.

## Cách tiếp cận Mobile-First

Mobile-First nghĩa là thiết kế cho màn hình nhỏ nhất trước, sau đó thêm độ phức tạp cho màn hình lớn hơn. Cách tiếp cận này:
- Buộc bạn ưu tiên nội dung
- Cho trải nghiệm mobile nhanh hơn
- Dễ mở rộng hơn thu nhỏ

## Prompt AI cho Responsive Design

### Layout Dashboard Responsive

```
Tạo layout dashboard responsive cho WebDevHub:

Mobile (< 768px):
- Đơn cột, full-width
- Điều hướng tab dưới cùng (Trang chủ, Dự án, Snippet, Hồ sơ)
- Header có thể thu gọn với menu hamburger
- Card xếp dọc
- Kéo để làm mới trên danh sách

Tablet (768px - 1024px):
- Layout hai cột
- Điều hướng sidebar (có thể thu gọn)
- Header với thanh tìm kiếm
- Card trong lưới 2 cột

Desktop (> 1024px):
- Layout ba cột (sidebar + nội dung + panel tùy chọn)
- Sidebar điều hướng cố định
- Header với tìm kiếm đầy đủ và menu người dùng
- Card trong lưới 3 cột

Dùng tiền tố responsive Tailwind.
Đảm bảo vùng chạm tối thiểu 44px trên mobile.
```

### Bảng dữ liệu Responsive

```
Tạo bảng dữ liệu responsive cho danh sách dự án:

Desktop:
- Bảng đầy đủ với tất cả cột
- Header sắp xếp được
- Hiệu ứng hover hàng
- Phân trang ở dưới

Tablet:
- Ẩn cột ít quan trọng (ngày tạo, cập nhật lần cuối)
- Giữ cột thiết yếu (tên, trạng thái, tech stack)
- Cuộn ngang nếu cần

Mobile:
- Chế độ card thay vì bảng
- Mỗi card hiển thị: tên, trạng thái, tech stack
- Mở rộng để xem chi tiết
- Vuốt hành động (sửa, xóa)

Dùng CSS Grid hoặc Flexbox cho layout.
Dùng class responsive Tailwind.
```

## Typography Responsive

```
Tạo hệ thống typography responsive:

Tiêu đề:
- h1: text-2xl sm:text-3xl md:text-4xl lg:text-5xl
- h2: text-xl sm:text-2xl md:text-3xl lg:text-4xl
- h3: text-lg sm:text-xl md:text-2xl lg:text-3xl
- h4: text-base sm:text-lg md:text-xl lg:text-2xl

Nội dung:
- Lớn: text-base sm:text-lg
- Cơ bản: text-sm sm:text-base
- Nhỏ: text-xs sm:text-sm

Chiều cao dòng:
- Tiêu đề: leading-tight
- Nội dung: leading-relaxed
- Chặt: leading-snug

Dùng tiện ích text responsive Tailwind.
```

## Hình ảnh Responsive

```
Triển khai hình ảnh responsive cho ảnh thu nhỏ dự án:

Yêu cầu:
- Kích thước khác nhau cho màn hình khác nhau
- Lazy loading cho hình ảnh dưới màn hình
- Placeholder mờ khi đang tải
- Giữ tỷ lệ khung hình (16:9 cho ảnh thu nhỏ)
- Định dạng WebP với fallback

Tạo:
1. Component ResponsiveImage
2. Tiện ích tối ưu hình ảnh
3. Tạo placeholder

Dùng component Image của Next.js với sizes responsive.
```

## Form Responsive

```
Tạo layout form responsive:

Mobile:
- Input full-width
- Xếp dọc
- Vùng chạm lớn (min-height: 44px)
- Nút full-width
- Label nổi

Tablet:
- Layout hai cột cho trường liên quan
- Nút cạnh nhau
- Thông báo validation inline

Desktop:
- Layout đa cột
- Khoảng cách紧凑
- Label inline
- Text trợ giúp tooltip

Dùng class responsive Tailwind.
Đảm bảo form dùng được chỉ với bàn phím.
```

## Pattern điều hướng Responsive

### Thanh tab dưới (Mobile)

```
Tạo thanh tab dưới cho điều hướng mobile:

Tab:
- Trang chủ (icon + nhãn)
- Dự án (icon + nhãn + badge đếm)
- Snippet (icon + nhãn)
- Hồ sơ (icon + nhãn)

Tính năng:
- Cố định ở dưới
- Tab active được highlight
- Badge cho thông báo
- Padding vùng an toàn cho thiết bị có notch
- Ẩn khi cuộn xuống, hiện khi cuộn lên

Dùng Tailwind CSS với fixed positioning.
```

### Sidebar điều hướng (Desktop)

```
Tạo sidebar có thể thu gọn cho desktop:

Tính năng:
- Vị trí cố định, chiều cao đầy đủ
- Logo ở trên
- Mục điều hướng với icon
- Trạng thái active với highlight
- Thu gọn chỉ còn chế độ icon
- Animation chuyển đổi mượt
- Nhớ trạng thái thu gọn trong localStorage

Dùng Tailwind CSS với transitions.
```

## Chiến lược Breakpoint Responsive

```
Định nghĩa chiến lược breakpoint cho WebDevHub:

Breakpoints:
- sm: 640px (điện thoại lớn)
- md: 768px (tablet)
- lg: 1024px (desktop nhỏ)
- xl: 1280px (desktop lớn)
- 2xl: 1536px (cực lớn)

Hướng dẫn sử dụng:
- sm: Điều chỉnh layout điện thoại
- md: Layout riêng tablet
- lg: Layout desktop
- xl: Vùng nội dung rộng hơn
- 2xl: Chiều rộng nội dung tối đa

Ghi chép khi nào dùng mỗi breakpoint.
```

## Kiểm tra Responsive Design

```
Tạo checklist kiểm tra responsive:

Kiểm tra trực quan:
- [ ] Tất cả nội dung đọc được trên mobile
- [ ] Không cuộn ngang trên mobile
- [ ] Vùng chạm tối thiểu 44px
- [ ] Hình ảnh scale đúng
- [ ] Chữ không tràn container

Kiểm tra tương tác:
- [ ] Điều hướng hoạt động trên mọi kích thước
- [ ] Form dùng được trên mobile
- [ ] Modal vừa màn hình nhỏ
- [ ] Dropdown không tràn viewport

Kiểm tra hiệu suất:
- [ ] Hình ảnh lazy load
- [ ] Không re-render không cần thiết
- [ ] Cuộn mượt
- [ ] Phản hồi chạm nhanh
```

## Bài tập thực hành

Làm cho các component WebDevHub responsive:

1. **Lưới dự án** — 1 cột mobile, 2 tablet, 3 desktop
2. **Điều hướng** — Tab dưới mobile, sidebar desktop
3. **Form** — Xếp dọc mobile, cạnh nhau desktop

Viết prompt responsive, triển khai, sau đó kiểm tra trên kích thước màn hình khác nhau.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ xử lý quản lý state — dùng AI để triển khai React Context, Zustand và các giải pháp state khác.
