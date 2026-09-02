# Thực hành: Xây trang UI hoàn chỉnh

## Kết hợp tất cả cùng nhau

Đến lúc áp dụng mọi thứ đã học! Trong bài thực hành này, bạn sẽ xây trang dashboard hoàn chỉnh cho WebDevHub với sự hỗ trợ của AI.

## Thử thách

Xây trang **Dashboard dự án** với các tính năng:

1. **Header** — Thông tin user, tìm kiếm, thông báo
2. **Phần thống kê** — 4 thẻ số liệu
3. **Lưới dự án** — Card dự án có bộ lọc
4. **Feed hoạt động** — Hoạt động gần đây
5. **Hành động nhanh** — Panel hành động thường dùng

## Bước 1: Lên kế hoạch kiến trúc

Viết prompt để lên kế hoạch kiến trúc component:

```
Tôi đang xây trang dashboard dự án cho WebDevHub.

Trang bao gồm:
- Header với avatar người dùng, thanh tìm kiếm và chuông thông báo
- Phần thống kê với 4 thẻ (tổng dự án, task hoàn thành, cộng tác viên, sao)
- Lưới dự án với thanh bộ lọc (tìm kiếm, lọc trạng thái, lọc tech)
- Feed hoạt động hiển thị hành động gần đây
- Panel hành động nhanh (dự án mới, snippet mới, mời thành viên)

Tech stack: Next.js 14, TypeScript, Tailwind CSS, Zustand

Giúp tôi lên kế hoạch:
1. Phân cấp component
2. Yêu cầu dữ liệu cho mỗi component
3. Cách tiếp cận quản lý state
4. Cấu trúc file
```

## Bước 2: Xây phần thống kê

```
Tạo component StatsSection cho dashboard.

Hiển thị 4 thẻ thống kê trong lưới responsive:
- Mobile: Lưới 2x2
- Tablet: 4 cột
- Desktop: 4 cột

Mỗi thẻ thống kê hiển thị:
- Icon (từ Lucide React)
- Nhãn (vd: "Tổng dự án")
- Giá trị (số)
- Chỉ báo xu hướng (mũi tên lên/xuống với phần trăm)
- Biểu đồ sparkline (7 ngày qua)

Dữ liệu thống kê:
- Tổng dự án: 24 (+12% so với tuần trước)
- Task hoàn thành: 156 (+8% so với tuần trước)
- Cộng tác viên: 8 (+2 mới tháng này)
- Tổng sao: 342 (+23% so với tuần trước)

Dùng Tailwind CSS cho styling.
Bao gồm trạng thái skeleton loading.
```

## Bước 3: Xây lưới dự án

```
Tạo component ProjectGrid với bộ lọc.

Tính năng:
- Thanh bộ lọc với:
  - Input tìm kiếm (debounce)
  - Lọc trạng thái (Tất cả, Active, Đã lưu trữ, Nháp)
  - Lọc tech stack (đa chọn)
  - Sắp xếp theo (Tên, Cập nhật, Sao)
- Card dự án trong lưới responsive (1/2/3 cột)
- Skeleton loading
- Trạng thái trống
- Nút "Tải thêm"

Mỗi ProjectCard hiển thị:
- Ảnh thu nhỏ
- Tên dự án
- Mô tả (rút gọn)
- Badge tech stack
- Badge trạng thái
- Số sao
- Thời gian cập nhật
- Menu hành động (Sửa, Lưu trữ, Xóa)

Dùng Zustand cho state bộ lọc.
Dùng React Query cho fetch dữ liệu.
```

## Bước 4: Xây feed hoạt động

```
Tạo component ActivityFeed.

Tính năng:
- Danh sách hoạt động gần đây
- Mỗi hoạt động hiển thị:
  - Avatar người dùng
  - Mô tả hành động
  - Thời gian (tương đối: "2 giờ trước")
  - Link đến mục liên quan
- Skeleton loading
- Link "Xem tất cả" ở dưới

Loại hoạt động:
- project_created: "đã tạo dự án {name}"
- task_completed: "hoàn thành task {title} trong {project}"
- member_joined: "tham gia với vai trò cộng tác viên"
- star_received: "nhận sao cho {project}"

Dùng date-fns cho thời gian tương đối.
```

## Bước 5: Xây panel hành động nhanh

```
Tạo component QuickActions.

Hành động:
- Dự án mới (mở modal tạo dự án)
- Snippet mới (mở trình chỉnh sửa snippet)
- Mời thành viên (mở dialog mời)
- Xem phân tích (chuyển đến trang phân tích)

Tính năng:
- Nút icon với nhãn
- Hiệu ứng hover
- Hiển thị phím tắt
- Layout responsive (ngang trên desktop, lưới trên mobile)
```

## Bước 6: Lắp ráp trang

```
Tạo component DashboardPage lắp ráp tất cả phần.

Layout:
- Header full-width
- Vùng nội dung với container max-width
- Phần thống kê ở trên
- Nội dung chính: Layout 2 cột
  - Trái (8/12): Lưới dự án
  - Phải (4/12): Feed hoạt động + Hành động nhanh
- Trên mobile: Đơn cột, xếp chồng

Bao gồm:
- Tiêu đề trang và breadcrumb
- Trạng thái loading cho mỗi phần
- Error boundary
- Layout responsive
```

## Bước 7: Thêm tương tác

```
Thêm các tương tác sau vào dashboard:

1. Tìm kiếm: Debounce cập nhật lưới dự án
2. Bộ lọc: Thay đổi bộ lọc cập nhật URL params
3. Click card dự án: Chuyển đến chi tiết dự án
4. Hành động nhanh: Mở modal/trang tương ứng
5. Thẻ thống kê: Nhấn để xem phân tích chi tiết
6. Mục hoạt động: Nhấn để chuyển đến mục liên quan
7. Kéo để làm mới trên mobile
```

## Bước 8: Hoàn thiện và tối ưu

```
Hoàn thiện dashboard:

Hiệu suất:
- Lazy load phần dưới màn hình
- Memoize tính toán tốn kém
- Tối ưu re-render

Accessibility:
- Điều hướng bàn phím
- Hỗ trợ trình đọc màn hình
- Quản lý focus
- Nhãn ARIA

Trực quan:
- Animation mượt
- Skeleton loading
- Trạng thái lỗi
- Trạng thái trống
```

## Tiêu chí đánh giá

Đánh giá triển khai:

| Tiêu chí | Điểm |
|----------|--------|
| Kiến trúc component | 0-2 |
| Responsive design | 0-2 |
| Quản lý state | 0-2 |
| Xử lý form | 0-2 |
| Accessibility | 0-2 |
| Hiệu suất | 0-2 |
| Chất lượng code | 0-2 |
| Hoàn thiện trực quan | 0-2 |
| **Tổng** | **0-16** |

## Bạn đã học được gì

Khi hoàn thành bài thực hành này, bạn đã thể hiện:

1. **Prompt Engineering** — Viết prompt hiệu quả cho tính năng phức tạp
2. **Context Engineering** — Dùng context dự án cho code nhất quán
3. **Kiến trúc Component** — Thiết kế hệ thống component modular
4. **Phát triển React** — Xây component với hook và state
5. **Styling** — Tạo thiết kế responsive với Tailwind CSS
6. **Quản lý State** — Triển khai Zustand và React Query
7. **Xử lý Form** — Xây form có validation với React Hook Form

## Tiếp theo là gì?

Chúc mừng hoàn thành Phần 4! Trong các phần tiếp theo, bạn sẽ học về phát triển backend, thiết kế API, tích hợp database và triển khai — tất cả với sự hỗ trợ AI.
