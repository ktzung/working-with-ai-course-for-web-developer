# Workshop Prompt

## Thực hành với tình huống thực tế

Đến lúc kiểm tra kỹ năng prompt engineering! Trong workshop này, bạn sẽ làm qua 8 tình huống phát triển web thực tế. Với mỗi tình huống, bạn sẽ viết prompt, tạo code với AI và tinh chỉnh kết quả.

## Tình huống 1: Form đăng ký người dùng

**Yêu cầu**: Xây form đăng ký cho WebDevHub với email, mật khẩu, tên và chọn vai trò.

**Bài tập**: Viết prompt tạo:
- React Hook Form với Zod validation
- Thanh chỉ báo độ mạnh mật khẩu
- Kiểm tra email tồn tại (debounce)
- Chấp nhận điều khoản dịch vụ
- Trạng thái loading khi gửi
- Xử lý lỗi với thông báo thân thiện

**Mẫu prompt**:
```
Tạo component form đăng ký cho Next.js 14 với TypeScript.

[Thêm yêu cầu cụ thể ở đây]
```

**Tiêu chí đánh giá**:
- Form validate đúng không?
- Thông báo lỗi có hữu ích không?
- Chỉ báo mật khẩu có chính xác không?
- Xử lý lỗi mạng tốt không?

## Tình huống 2: Widget Dashboard dự án

**Yêu cầu**: Tạo widget thống kê hiển thị số liệu dự án.

**Bài tập**: Viết prompt cho component hiển thị:
- Tổng số dự án
- Task hoàn thành tuần này
- Cộng tác viên đang active
- Chỉ báo xu hướng (mũi tên lên/xuống với phần trăm)
- Biểu đồ sparkline nhỏ
- Layout responsive

**Thử thách**: Hoạt động với dữ liệu mẫu trước, sau đó kết nối API thật.

## Tình huống 3: Bảng Kanban kéo thả

**Yêu cầu**: Xây bảng Kanban quản lý task.

**Bài tập**: Viết prompt cho:
- Ba cột (Cần làm, Đang làm, Hoàn thành)
- Kéo thả giữa các cột
- Thêm task mới inline
- Card task với badge ưu tiên
- Đếm task mỗi cột
- Lưu thứ tự vào API

**Gợi ý**: Chỉ định thư viện kéo thả (dnd-kit, react-beautiful-dnd, hoặc HTML5 DnD thuần).

## Tình huống 4: Trình quản lý Code Snippet

**Yêu cầu**: Tạo component code snippet với syntax highlighting.

**Bài tập**: Viết prompt cho:
- Trình chỉnh sửa code với syntax highlighting (Prism.js hoặc highlight.js)
- Dropdown chọn ngôn ngữ
- Nút copy vào clipboard
- Số dòng
- Chuyển đổi theme tối/sáng
- Lưu và chia sẻ

**Thử thách**: Hỗ trợ 10+ ngôn ngữ lập trình.

## Tình huống 5: Tìm kiếm với bộ lọc

**Yêu cầu**: Xây tính năng tìm kiếm với nhiều tùy chọn lọc.

**Bài tập**: Viết prompt cho:
- Input tìm kiếm debounce
- Lọc theo tech stack (đa chọn)
- Lọc theo trạng thái (đơn chọn)
- Tùy chọn sắp xếp (tên, ngày, sao)
- Pill bộ lọc active với nút xóa
- Đồng bộ trạng thái URL (URL tìm kiếm chia sẻ được)

**Đánh giá**: Tìm kiếm có nhanh không? Bộ lọc có trực quan không?

## Tình huống 6: Điều hướng responsive

**Yêu cầu**: Tạo component điều hướng hoạt động trên mọi thiết bị.

**Bài tập**: Viết prompt cho:
- Desktop: Điều hướng ngang với dropdown
- Tablet: Điều hướng thu gọn với menu hamburger
- Mobile: Thanh tab dưới cùng
- Chỉ báo trạng thái active
- Avatar người dùng với dropdown menu
- Chuông thông báo với badge đếm
- Chuyển đổi mượt mà giữa các trạng thái

## Tình huống 7: Lớp tích hợp API

**Yêu cầu**: Tạo API client tái sử dụng cho WebDevHub.

**Bài tập**: Viết prompt cho:
- API client an toàn kiểu dùng TypeScript
- Tự động làm mới token
- Interceptor request/response
- Xử lý lỗi với kiểu lỗi tùy chỉnh
- Quản lý trạng thái loading
- Hủy request
- Logic thử lại cho request thất bại

**Gợi ý**: Cân nhắc dùng axios hoặc wrapper fetch tùy chỉnh.

## Tình huống 8: Thông báo thời gian thực

**Yêu cầu**: Thêm hệ thống thông báo vào WebDevHub.

**Bài tập**: Viết prompt cho:
- Thông báo toast (thành công, lỗi, cảnh báo, thông tin)
- Dropdown trung tâm thông báo
- Cập nhật thời gian thực qua WebSocket hoặc SSE
- Đánh dấu đã đọc/chưa đọc
- Tùy chọn thông báo
- Hỗ trợ âm thanh và thông báo desktop

## Quy trình Workshop

Với mỗi tình huống:

1. **Viết prompt** (5 phút)
   - Dùng bốn yếu tố: Context, Task, Tech Stack, Constraints
   - Cụ thể về kiểu, edge case và tương tác

2. **Tạo code** (5 phút)
   - Dùng công cụ AI ưa thích
   - Tạo triển khai ban đầu

3. **Review và tinh chỉnh** (10 phút)
   - Kiểm tra bug và edge case
   - Xác minh kiểu TypeScript
   - Test responsive
   - Kiểm tra accessibility

4. **Lặp lại** (5 phút)
   - Nhờ AI cải thiện khía cạnh cụ thể
   - Thêm tính năng còn thiếu
   - Tối ưu hiệu suất

## Thang điểm

Đánh giá prompt của bạn:

| Tiêu chí | Điểm |
|----------|--------|
| Tech stack cụ thể | 0-2 |
| Mô tả task rõ ràng | 0-2 |
| Edge case được đề cập | 0-2 |
| An toàn kiểu | 0-2 |
| Accessibility | 0-2 |
| **Tổng** | **0-10** |

## Sau Workshop

Suy ngẫm:
- Tình huống nào khó nhất để prompt?
- Bạn phát hiện pattern gì?
- Prompt cải thiện thế nào từ tình huống 1 đến 8?
- Lần sau bạn sẽ làm gì khác?

## Tiếp theo là gì?

Trong Phần 3, chúng ta sẽ đi sâu vào context engineering — học cách thiết lập dự án để công cụ AI hiểu codebase và tạo code phù hợp hơn.
