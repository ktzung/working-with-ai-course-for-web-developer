# Lỗi Prompt thường gặp

## Tránh bẫy trong phát triển hỗ trợ AI

Ngay cả lập trình viên giàu kinh nghiệm cũng mắc lỗi prompt. Nhận diện các pattern này sẽ giúp bạn đạt kết quả tốt hơn từ công cụ AI và tránh các cuộc hội thoại qua lại gây bực mình.

## Lỗi 1: Quá mơ hồ

**Tệ**: "Tạo trang đăng nhập"

**Tại sao thất bại**: AI không biết framework, cách styling, phương thức xác thực hay sở thích thiết kế.

**Tốt hơn**: "Tạo trang đăng nhập cho Next.js 14 với TypeScript. Dùng React Hook Form với Zod validation. Bao gồm trường email và mật khẩu, checkbox 'Ghi nhớ tôi', nút đăng nhập mạng xã hội (Google, GitHub), và link đến trang đăng ký. Style với Tailwind CSS, layout card căn giữa, max-width 400px."

## Lỗi 2: Không chỉ định Tech Stack

**Tệ**: "Tạo hàm lấy danh sách người dùng"

**Tại sao thất bại**: Nên dùng fetch, axios hay thư viện? Xử lý lỗi thế nào? Kiểu TypeScript?

**Tốt hơn**: "Tạo hàm dùng TypeScript fetch người dùng từ /api/users bằng native fetch API. Bao gồm tham số phân trang (page, limit), trạng thái loading, xử lý lỗi với try/catch, và kiểu trả về { users: User[], total: number }."

## Lỗi 3: Yêu cầu quá nhiều cùng lúc

**Tệ**: "Xây cho tôi nền tảng thương mại điện tử hoàn chỉnh với xác thực, danh sách sản phẩm, giỏ hàng, thanh toán, quản lý đơn và dashboard admin"

**Tại sao thất bại**: Quá rộng. AI sẽ tạo thứ chung chung không khớp nhu cầu.

**Tốt hơn**: Chia nhỏ:
1. "Tạo model dữ liệu sản phẩm với Prisma"
2. "Xây component ProductCard"
3. "Tạo API endpoint danh sách sản phẩm"
4. "Xây giỏ hàng với Zustand"

## Lỗi 4: Không cung cấp context

**Tệ**: "Sửa lỗi này: Cannot read property 'map' of undefined"

**Tại sao thất bại**: AI không biết lỗi xảy ra ở đâu hay cấu trúc dữ liệu bạn đang dùng.

**Tốt hơn**: "Tôi gặp lỗi 'Cannot read property map of undefined' trong component ProjectList khi API trả về response rỗng. Đây là code component: [paste]. Dữ liệu đến từ hook useProjects: [paste]. Xử lý thế nào khi projects là undefined?"

## Lỗi 5: Bỏ qua pattern code hiện có

**Tệ**: "Tạo API endpoint mới cho task"

**Tại sao thất bại**: AI có thể tạo code không khớp pattern hiện có.

**Tốt hơn**: "Theo cùng pattern với endpoint /api/projects hiện có (paste code), tạo endpoint tương tự cho task. Dùng cách xử lý lỗi, validation và định dạng response giống nhau."

## Lỗi 6: Không chỉ định edge case

**Tệ**: "Tạo hàm định dạng ngày"

**Tại sao thất bại**: Ngày null thì sao? Ngày không hợp lệ? Múi giờ? Định dạng khác nhau?

**Tốt hơn**: "Tạo hàm định dạng ngày xử lý: input null/undefined (trả về 'N/A'), ngày không hợp lệ (trả về 'Invalid Date'), chuyển đổi múi giờ sang UTC, và hỗ trợ định dạng: 'tương đối' (2 giờ trước), 'ngắn' (1 thg 1, 2024), 'dài' (1 tháng 1, 2024 lúc 3:00 CH)."

## Lỗi 7: Chấp nhận kết quả đầu tiên mà không review

**Vấn đề**: AI tạo code trông đúng nhưng có vấn đề tinh vi.

**Giải pháp**: Luôn review code do AI tạo:
- Lỗ hổng bảo mật (XSS, injection)
- Vấn đề hiệu suất (re-render không cần thiết, thiếu memoization)
- Edge case (kiểm tra null, xử lý lỗi)
- Nhất quán style code
- An toàn kiểu TypeScript

## Lỗi 8: Không lặp lại

**Tệ**: Nhận kết quả trung bình rồi bỏ cuộc.

**Tốt hơn**: Lặp lại! Nếu kết quả đầu chưa hoàn hảo:
- "Làm cho accessible hơn"
- "Thêm hỗ trợ dark mode"
- "Tối ưu cho mobile"
- "Thêm error boundary"
- "Bao gồm unit test"

## Lỗi 9: Copy-paste lỗi mà không có context

**Tệ**: Paste stack trace 50 dòng mà không giải thích.

**Tốt hơn**: "Tôi gặp lỗi này sau khi thêm tính năng tìm kiếm vào danh sách dự án. Lỗi xảy ra khi gõ vào input tìm kiếm. Đây là code và lỗi liên quan: [paste]. Tôi nghĩ liên quan đến hàm debounce."

## Lỗi 10: Không dùng AI để học

**Vấn đề**: Chỉ dùng AI để tạo code, không bao giờ để hiểu.

**Tốt hơn**: Hỏi thêm:
- "Giải thích tại sao dùng useCallback ở đây"
- "Trade-off của cách tiếp cận này là gì?"
- "Nó scale thế nào với 10,000 mục?"
- "Có cách triển khai thay thế không?"

## Tham chiếu nhanh: Checklist Prompt

Trước khi gửi prompt, kiểm tra:

- [ ] Tech stack cụ thể được đề cập?
- [ ] Mô tả task rõ ràng?
- [ ] Code/context liên quan được bao gồm?
- [ ] Edge case được chỉ định?
- [ ] Constraints được định nghĩa?
- [ ] Định dạng output mong đợi rõ ràng?

## Bài tập thực hành

Sửa các prompt tệ này:

1. **Tệ**: "Tạo tính năng tìm kiếm"
2. **Tệ**: "Code tôi không chạy"
3. **Tệ**: "Thêm style cho component này"
4. **Tệ**: "Tạo schema database"

Viết lại mỗi prompt dùng các pattern từ bài học.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ tổng hợp mọi thứ trong workshop prompt với 8 tình huống phát triển web thực tế.
