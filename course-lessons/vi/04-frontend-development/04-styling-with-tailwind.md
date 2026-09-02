# Styling với Tailwind CSS

## Workflow Styling hỗ trợ AI

Tailwind CSS và AI là cặp đôi hoàn hảo. Class tiện ích Tailwind mô tả rõ, giúp AI dễ tạo thiết kế đẹp, responsive từ mô tả của bạn.

## Tạo class Tailwind

### Từ mô tả đến class

```
Tôi cần section hero cho WebDevHub với:
- Chiều cao toàn viewport
- Nền gradient (xanh dương sang tím)
- Nội dung căn giữa
- Tiêu đề lớn chữ trắng
- Phụ đề chữ nhạt hơn
- Hai nút CTA (primary đặc, secondary viền)
- Pattern nền trang trí

Tạo class Tailwind CSS cho layout này.
```

### Phản hồi AI

```tsx
<section className="relative min-h-screen bg-gradient-to-br from-blue-600 to-purple-700 flex items-center justify-center overflow-hidden">
  {/* Pattern nền */}
  <div className="absolute inset-0 opacity-10">
    <div className="absolute inset-0 bg-[url('/grid.svg')] bg-center [mask-image:linear-gradient(180deg,white,rgba(255,255,255,0))]" />
  </div>
  
  <div className="relative z-10 text-center px-4 max-w-4xl mx-auto">
    <h1 className="text-5xl md:text-7xl font-bold text-white mb-6 leading-tight">
      Xây dựng
      <span className="block text-transparent bg-clip-text bg-gradient-to-r from-blue-200 to-purple-200">
        Dự án Web tuyệt vời
      </span>
    </h1>
    
    <p className="text-xl md:text-2xl text-blue-100 mb-10 max-w-2xl mx-auto">
      Showcase sản phẩm, quản lý dự án và cộng tác với lập trình viên toàn cầu.
    </p>
    
    <div className="flex flex-col sm:flex-row gap-4 justify-center">
      <button className="px-8 py-4 bg-white text-blue-600 font-semibold rounded-lg hover:bg-blue-50 transition-colors shadow-lg">
        Bắt đầu miễn phí
      </button>
      <button className="px-8 py-4 border-2 border-white text-white font-semibold rounded-lg hover:bg-white/10 transition-colors">
        Xem Demo
      </button>
    </div>
  </div>
</section>
```

## Prompt Responsive Design

### Layout Mobile-First

```
Tạo lưới dự án responsive dùng Tailwind CSS:

Yêu cầu:
- Mobile (< 640px): 1 cột, card full-width
- Tablet (640px - 1024px): 2 cột
- Desktop (> 1024px): 3 cột
- Gap: 4 trên mobile, 6 trên desktop
- Card bằng chiều cao

Bao gồm:
- Padding responsive cho container
- Kích thước chữ responsive
- Khoảng cách responsive giữa các section
```

### Điều hướng Responsive

```
Tạo thanh điều hướng responsive:

Desktop:
- Layout ngang
- Logo bên trái
- Link nav căn giữa
- Menu người dùng bên phải
- Chiều cao: 16 (h-16)

Mobile:
- Nút menu hamburger
- Sidebar trượt ra từ trái
- Overlay phía sau sidebar
- Nút đóng trong sidebar

Dùng tiền tố responsive Tailwind (sm:, md:, lg:).
Dùng CSS transition cho animation mượt.
```

## Triển khai Dark Mode

```
Triển khai dark mode cho WebDevHub dùng Tailwind CSS:

Yêu cầu:
- Nút chuyển đổi trong header
- Lưu tùy chọn trong localStorage
- Theo dõi tùy chọn hệ thống (prefers-color-scheme)
- Chuyển đổi mượt giữa các chế độ

Ánh xạ màu:
- Nền sáng: bg-white, tối: bg-gray-900
- Chữ sáng: text-gray-900, tối: text-gray-100
- Card sáng: bg-white, tối: bg-gray-800
- Viền sáng: border-gray-200, tối: border-gray-700

Tạo:
1. Component ThemeProvider
2. Component ThemeToggle
3. Cập nhật tailwind.config.ts với darkMode: 'class'
```

## Pattern Styling Component

### Biến thể Card

```
Tạo class Tailwind CSS cho biến thể card:

Card mặc định:
- Nền trắng, shadow nhẹ, rounded-lg
- Hover: shadow-md, nâng nhẹ

Card nổi:
- Nền trắng, shadow-md, rounded-xl
- Hover: shadow-lg, translate-y-[-2px]

Card phẳng:
- Nền xám nhạt, không shadow, rounded-lg
- Viền: border-gray-200

Card tương tác:
- Clickable với cursor-pointer
- Hover: border-blue-300, ring-2 ring-blue-100

Tất cả biến thể hỗ trợ dark mode.
```

### Styling Form

```
Tạo class Tailwind CSS cho phần tử form:

Trường input:
- Cơ bản: border, rounded-md, px-4, py-2
- Focus: ring-2, ring-blue-500, border-blue-500
- Lỗi: border-red-500, ring-red-500
- Disabled: bg-gray-100, cursor-not-allowed

Label:
- Block, text-sm, font-medium, text-gray-700
- Chỉ báo bắt buộc: text-red-500

Thông báo lỗi:
- text-sm, text-red-600, mt-1

Text trợ giúp:
- text-sm, text-gray-500, mt-1
```

## Animation với Tailwind

```
Thêm các animation sau dùng Tailwind CSS:

1. Fade in: opacity-0 đến opacity-100
2. Slide lên: translate-y-4 đến translate-y-0
3. Scale in: scale-95 đến scale-100
4. Pulse: animate-pulse cho trạng thái loading
5. Spin: animate-spin cho spinner loading
6. Bounce: animate-bounce cho thu hút

Tạo component tiện ích cho animation vào:
- Hỗ trợ trễ (100ms, 200ms, 300ms)
- Hỗ trợ hướng (lên, dưới, trái, phải)
- Kích hoạt khi giao nhau (cuộn vào tầm nhìn)
```

## Cấu hình Tailwind

```
Mở rộng tailwind.config.ts cho WebDevHub:

Mở rộng theme:
- Màu: brand (xanh dương), accent (tím), success (xanh lá), warning (hổ phách), error (đỏ)
- Font: heading (Inter), body (Inter), mono (JetBrains Mono)
- Shadow: card, dropdown, modal
- Animation: fadeIn, slideUp, scaleIn
- BorderRadius: card, button, input

Plugins:
- @tailwindcss/forms (styling form)
- @tailwindcss/typography (styling prose)
- @tailwindcss/aspect-ratio (tỷ lệ khung hình)
```

## Bài tập thực hành

Style các trang WebDevHub với Tailwind CSS:

1. **Trang đích** — Hero, tính năng, đánh giá, CTA
2. **Dashboard** — Thẻ thống kê, lưới dự án, feed hoạt động
3. **Trang đăng nhập** — Form căn giữa với đăng nhập mạng xã hội

Viết prompt styling chi tiết, tạo code, sau đó tinh chỉnh.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ đi sâu hơn vào responsive design — tạo layout hoạt động đẹp trên mọi thiết bị.
