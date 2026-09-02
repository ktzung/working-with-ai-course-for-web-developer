# Kiến trúc Component với AI

## Thiết kế phân cấp Component

Kiến trúc component tốt là nền tảng cho code frontend dễ bảo trì. AI có thể giúp bạn thiết kế phân cấp component modular, tái sử dụng và dễ hiểu.

## Quy trình tư duy Component

Trước khi tạo code, hãy suy nghĩ:

1. **UI bao gồm những gì?** — Xác định thành phần trực quan
2. **Mỗi thành phần cần dữ liệu gì?** — Định nghĩa props
3. **Các thành phần tương tác thế nào?** — Ánh xạ mối quan hệ
4. **Cái gì có thể tái sử dụng?** — Trích xuất component chung

## Thiết kế kiến trúc với AI

### Prompt phân tích Component

```
Tôi đang xây dashboard dự án cho WebDevHub. Đây là mô tả UI:

Dashboard có:
- Header với thông tin người dùng và thông báo
- Sidebar với điều hướng
- Vùng nội dung chính với:
  - Thẻ thống kê (tổng dự án, task hoàn thành, cộng tác viên)
  - Danh sách dự án với bộ lọc
  - Feed hoạt động gần đây
  - Panel hành động nhanh

Giúp tôi thiết kế phân cấp component. Với mỗi component, chỉ định:
- Tên component
- Props cần có
- Là container hay presentational component
- Nằm ở đâu trong cấu trúc file
```

### Ví dụ phản hồi AI

```
Phân cấp Component:

src/components/
├── layout/
│   ├── DashboardLayout.tsx        # Container: quản lý trạng thái layout
│   ├── Header.tsx                 # Presentational: hiển thị thông tin user
│   ├── Sidebar.tsx                # Presentational: link điều hướng
│   └── MainContent.tsx            # Presentational: wrapper nội dung
├── dashboard/
│   ├── StatsSection.tsx           # Container: fetch và quản lý thống kê
│   ├── StatsCard.tsx              # Presentational: hiển thị một thống kê
│   ├── ProjectListSection.tsx     # Container: quản lý danh sách dự án
│   ├── ProjectCard.tsx            # Presentational: một dự án
│   ├── ActivityFeed.tsx           # Container: fetch hoạt động
│   ├── ActivityItem.tsx           # Presentational: một hoạt động
│   └── QuickActions.tsx           # Presentational: nút hành động
└── shared/
    ├── FilterBar.tsx              # Tái sử dụng: điều khiển bộ lọc
    └── EmptyState.tsx             # Tái sử dụng: trạng thái trống
```

## Pattern thiết kế Component

### Tách Container/Presentational

```
Thiết kế hệ thống component dùng pattern Container/Presentational:

Container component:
- Xử lý fetch dữ liệu
- Quản lý state
- Xử lý logic nghiệp vụ
- Truyền dữ liệu xuống qua props

Presentational component:
- Nhận dữ liệu qua props
- Chỉ xử lý logic hiển thị
- Phát sự kiện qua callback
- Không gọi API trực tiếp

Cho tính năng danh sách dự án, tạo:
1. ProjectListContainer (fetch, lọc, phân trang)
2. ProjectList (render lưới)
3. ProjectCard (hiển thị một dự án)
4. ProjectCardSkeleton (trạng thái loading)
```

### Compound Component

```
Thiết kế compound component cho bảng dữ liệu:

DataTable
├── DataTable.Header
│   └── DataTable.ColumnHeader (sắp xếp được)
├── DataTable.Body
│   └── DataTable.Row
│       └── DataTable.Cell
├── DataTable.Footer
│   └── DataTable.Pagination
└── DataTable.Empty

Tính năng:
- Cột sắp xếp được
- Hàng chọn được
- Phân trang
- Skeleton loading
- Trạng thái trống

Dùng React Context để chia sẻ state giữa các sub-component.
```

## Prompt cho kiến trúc

### Prompt phân tích

```
Phân tích UI này và gợi ý kiến trúc component:

[paste ảnh chụp hoặc mô tả UI]

Yêu cầu:
- Tối đa 3 cấp lồng nhau
- Mỗi component có một trách nhiệm
- Xác định component tái sử dụng
- Gợi ý cách quản lý state
- Cân nhắc trạng thái loading và lỗi
```

### Prompt refactor

```
Tôi có component lớn (500+ dòng) xử lý toàn bộ trang dự án.

Đây là code: [paste code]

Giúp tôi chia nhỏ thành các component tập trung hơn:
1. Xác định ranh giới logic
2. Trích xuất phần tái sử dụng
3. Xác định state thuộc đâu
4. Giữ nguyên chức năng
```

## Bản ghi quyết định kiến trúc

Ghi chép quyết định:

```markdown
# ADR: Kiến trúc Component

## Context
Cần cách tiếp cận nhất quán cho tổ chức component.

## Quyết định
- Dùng pattern Container/Presentational
- Tối đa 3 cấp lồng component
- Một component mỗi file
- Đặt file liên quan cạnh nhau (component + test + style)

## Hệ quả
- Dễ test presentational component hơn
- Tách biệt rõ ràng
- Có thể nhiều file hơn cho tính năng đơn giản
```

## Bài tập thực hành

Thiết kế kiến trúc component cho tính năng quản lý snippet của WebDevHub:

Yêu cầu:
- Danh sách code snippet với tìm kiếm
- Card snippet với xem trước
- Trình chỉnh sửa snippet với syntax highlighting
- Bộ lọc ngôn ngữ
- Quản lý tag

Viết prompt tạo kiến trúc hoàn chỉnh, sau đó triển khai component.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ dùng AI để xây component React — từ thành phần UI đơn giản đến tính năng tương tác phức tạp.
