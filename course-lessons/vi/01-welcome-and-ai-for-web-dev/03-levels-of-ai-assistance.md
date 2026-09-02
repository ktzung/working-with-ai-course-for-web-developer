# Các cấp độ hỗ trợ AI

## Từ Autocomplete đến tạo toàn bộ Feature

Không phải hỗ trợ AI nào cũng giống nhau. Hãy tưởng tượng nó như một quang phổ — từ hoàn thành dòng đơn giản đến AI tạo toàn bộ feature. Hiểu các cấp độ này giúp bạn chọn cách tiếp cận phù hợp cho từng task.

## Cấp độ 1: Autocomplete nội dòng

Đây là nơi hầu hết lập trình viên bắt đầu. Công cụ AI gợi ý vài ký tự hoặc dòng tiếp theo khi bạn gõ.

**Ví dụ**: Bạn bắt đầu gõ `const formatDate =` và Copilot gợi ý toàn bộ thân hàm.

**Khi nào nên dùng**:
- Viết hàm tiện ích
- Hoàn thành pattern lặp lại
- Hoàn thành import statement
- Viết type definition

**Ưu điểm**: Nhanh, ít cản trở, không gây phiền
**Hạn chế**: Giới hạn ở những gì có thể dự đoán từ context cục bộ

## Cấp độ 2: Comment thành Code

Bạn viết comment mô tả điều bạn muốn, và AI tạo ra phần triển khai.

**Ví dụ**:
```typescript
// Tạo custom hook quản lý danh sách người dùng phân trang
// kèm tìm kiếm, sắp xếp và trạng thái loading
```

AI tạo ra hook `usePaginatedUsers` hoàn chỉnh với đầy đủ logic.

**Khi nào nên dùng**:
- Xây dựng hàm hoặc hook mới
- Tạo API endpoint
- Viết test case
- Tạo boilerplate

**Ưu điểm**: Tuyệt vời cho task được định nghĩa rõ, tạo triển khai hoàn chỉnh
**Hạn chế**: Cần comment rõ ràng, cụ thể

## Cấp độ 3: Tạo qua Chat

Bạn có cuộc hội thoại với AI về những gì cần, lặp lại cho đến khi có kết quả đúng.

**Ví dụ hội thoại**:
- "Tôi cần component card hồ sơ người dùng"
- "Cho nó nhận type User với name, avatar, role và bio"
- "Thêm hiệu ứng hover và làm avatar hình tròn"
- "Giờ thêm trạng thái skeleton loading"

**Khi nào nên dùng**:
- Thiết kế component API
- Khám phá các cách triển khai khác nhau
- Xây dựng feature với nhiều yêu cầu
- Debug vấn đề phức tạp

**Ưu điểm**: Lặp lại, có thể tinh chỉnh kết quả, xử lý yêu cầu phức tạp
**Hạn chế**: Chậm hơn gợi ý nội dòng, cần qua lại nhiều

## Cấp độ 4: Tạo đa file

AI tạo code trên nhiều file để triển khai một feature hoàn chỉnh.

**Ví dụ**: "Tạo hệ thống xác thực người dùng với trang login, auth context, route bảo vệ và API middleware."

AI tạo ra:
- `LoginPage.tsx` — Component form đăng nhập
- `AuthContext.tsx` — Quản lý trạng thái xác thực
- `ProtectedRoute.tsx` — Component bảo vệ route
- `authMiddleware.ts` — Xác thực phía server
- `useAuth.ts` — Custom hook xác thực

**Khi nào nên dùng**:
- Thiết lập feature mới
- Tạo project boilerplate
- Triển khai pattern trải nhiều file

**Ưu điểm**: Tiết kiệm thời gian lớn khi scaffold
**Hạn chế**: Cần review kỹ, có thể không khớp quy ước chính xác

## Cấp độ 5: Hướng dẫn kiến trúc

AI giúp bạn đưa ra quyết định cấp cao về cấu trúc dự án và lựa chọn công nghệ.

**Ví dụ**: "Tôi đang xây dashboard thời gian thực. Nên dùng WebSocket hay Server-Sent Events? Nên cấu trúc state management thế nào?"

**Khi nào nên dùng**:
- Bắt đầu dự án mới
- Đánh giá lựa chọn công nghệ
- Lên kế hoạch kiến trúc feature
- Xác định vấn đề scalability tiềm ẩn

**Ưu điểm**: Tận dụng kiến thức rộng về pattern và best practice
**Hạn chế**: Khuyến nghị cần xác nhận cho ngữ cảnh cụ thể

## Tìm sự cân bằng

Hầu hết lập trình viên web làm việc chủ yếu ở Cấp độ 1-3, thỉnh thoảng sang Cấp độ 4 để scaffold và Cấp độ 5 để lên kế hoạch. Hướng dẫn thực tế:

| Độ phức tạp task | Cấp độ khuyến nghị |
|----------------|-------------------|
| Hàm tiện ích đơn giản | Cấp độ 1-2 |
| Component mới | Cấp độ 2-3 |
| Feature nhiều file | Cấp độ 3-4 |
| Setup dự án mới | Cấp độ 4-5 |
| Quyết định kiến trúc | Cấp độ 5 |

## Nguy hiểm của việc phụ thuộc quá mức

Lưu ý: cấp độ hỗ trợ AI càng cao thì càng cần review kỹ hơn. Khi AI tạo một hàm đơn, bạn có thể nhanh chóng kiểm tra. Khi nó tạo toàn bộ feature trên năm file, bug có thể ẩn trong tương tác giữa các file.

**Best practice**: Luôn review code do AI tạo, đặc biệt ở Cấp độ 4-5. Hiểu từng file làm gì trước khi tiếp tục.

## Tiếp theo là gì?

Giờ bạn đã hiểu quang phổ hỗ trợ AI, hãy nói về điều quan trọng không kém: cách sử dụng AI có trách nhiệm trong khi vẫn đảm bảo chất lượng code và bảo mật.
