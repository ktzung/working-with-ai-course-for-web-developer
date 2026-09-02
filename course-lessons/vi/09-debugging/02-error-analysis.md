# Phân tích lỗi với AI

## Đọc thông báo lỗi như chuyên gia

Thông báo lỗi không phải kẻ thù của bạn — chúng là đồng minh trong việc gỡ lỗi. Vấn đề là hầu hết lập trình viên hoảng sợ khi thấy chữ đỏ trong console. AI có thể biến những thông báo lỗi đáng sợ thành những hiểu biết rõ ràng, có thể hành động được.

## Cấu trúc của lỗi web

Mỗi lỗi đều kể một câu chuyện. Hãy phân tích những gì AI có thể giúp bạn hiểu:

### Stack Trace

Stack trace cho thấy chuỗi các lệnh gọi hàm dẫn đến lỗi. AI có thể đọc chúng và cho bạn biết chính xác cần xem ở đâu.

**Ví dụ prompt:** "Đây là stack trace của tôi: `TypeError: Cannot read properties of undefined (reading 'map') at UserList (UserList.jsx:15) at renderWithHooks...` Điều này có nghĩa là gì và cách sửa?"

AI sẽ giải thích rằng `users` là undefined khi component cố gắng map qua nó, và đề xuất thêm trạng thái loading hoặc giá trị mặc định.

### Lỗi mạng

HTTP errors có ý nghĩa cụ thể mà AI có thể giải mã ngay lập tức:

- **400 Bad Request:** "Request body bị sai định dạng. Kiểm tra cú pháp JSON và các trường bắt buộc."
- **401 Unauthorized:** "Token xác thực bị thiếu hoặc hết hạn. Kiểm tra header Authorization."
- **403 Forbidden:** "Bạn đã xác thực nhưng không có quyền. Kiểm tra vai trò người dùng."
- **404 Not Found:** "Endpoint không tồn tại. Xác minh URL và HTTP method."
- **500 Internal Server Error:** "Server bị crash. Kiểm tra backend logs."

**Mẫu prompt:** "Tôi nhận lỗi 422 Unprocessable Entity khi gửi form. Đây là payload request và middleware validation Express. Có gì sai?"

### Lỗi build

Webpack, Vite và các bundler khác tạo ra thông báo lỗi khó hiểu. AI có thể dịch chúng:

- "Module parse failed" → lỗi cú pháp trong file
- "Circular dependency detected" → hai file import lẫn nhau
- "Out of memory" → bundle quá lớn, cần code splitting

## Quy trình phân tích lỗi với AI

### Bước 1: Sao chép toàn bộ lỗi

Đừng diễn đạt lại. Sao chép toàn bộ thông báo lỗi, bao gồm đường dẫn file và số dòng.

### Bước 2: Thêm ngữ cảnh

"Tôi đang làm gì khi lỗi này xảy ra? Tôi đã thay đổi gì gần đây?"

### Bước 3: Yêu cầu tìm nguyên nhân gốc

"Đây là triệu chứng của vấn đề sâu hơn, hay là sửa lỗi đơn giản?"

### Bước 4: Yêu cầu cách phòng tránh

"Làm thế nào tôi có thể ngăn loại lỗi này trong tương lai?"

## Các mẫu lỗi phổ biến trong phát triển web

### Lỗi React

**"Maximum update depth exceeded"** — Vòng lặp render vô hạn. AI sẽ kiểm tra dependencies useEffect và cập nhật state.

**"Can't perform a React state update on an unmounted component"** — Rò rỉ bộ nhớ từ thao tác bất đồng bộ. AI sẽ đề xuất hàm cleanup.

**"Objects are not valid as a React child"** — Cố gắng render object thay vì string. AI sẽ giúp bạn truy cập đúng thuộc tính.

### Lỗi Node.js

**"EADDRINUSE"** — Port đang được sử dụng. AI sẽ chỉ cách tìm và tắt tiến trình.

**"Cannot find module"** — Thiếu dependency hoặc sai đường dẫn. AI sẽ kiểm tra package.json và câu lệnh import.

**"ERR_REQUIRE_ESM"** — Trộn lẫn CommonJS và ES Modules. AI sẽ giúp bạn chuyển sang một hệ thống.

### Lỗi Database

**"ER_DUP_ENTRY"** — Vi phạm ràng buộc duy nhất. AI sẽ đề xuất mẫu upsert hoặc kiểm tra trùng lặp.

**"Connection refused"** — Máy chủ database không chạy. AI sẽ giúp kiểm tra chuỗi kết nối và trạng thái server.

## Xây dựng cơ sở kiến thức lỗi

Nhờ AI giúp bạn tạo tài liệu tham khảo lỗi cá nhân:

"Giúp tôi tạo file markdown ghi lại 10 lỗi phổ biến nhất tôi gặp trong phát triển React, với nguyên nhân, cách sửa và cách phòng tránh cho mỗi lỗi."

Theo thời gian, đây sẽ trở thành bảng gian lận gỡ lỗi của bạn. Khi gặp mẫu lỗi quen thuộc, bạn sẽ biết chính xác cần xem ở đâu.

## Nâng cao: Phân tích mẫu lỗi

Nếu bạn thấy lỗi lặp lại, nhờ AI tìm mẫu:

"Tôi liên tục gặp lỗi undefined trong các component. Đây là 5 lần gần nhất. Có vấn đề có hệ thống trong cách tôi xử lý dữ liệu async không?"

AI có thể nhận ra rằng bạn không xử lý nhất quán trạng thái loading, và đề xuất custom hook bọc tất cả việc lấy dữ liệu.

## Bài tập thực hành

Tìm một lỗi trong dự án hiện tại. Thay vì ngay lập tức tìm Stack Overflow, hãy thử quy trình này:

1. Sao chép toàn bộ thông báo lỗi
2. Nhờ AI giải thích ý nghĩa bằng ngôn ngữ đơn giản
3. Nhờ AI đề xuất 3 nguyên nhân có thể xảy ra nhất
4. Làm việc qua từng nguyên nhân một cách có hệ thống
5. Nhờ AI cách phòng tránh loại lỗi này trong tương lai

## Điểm mấu chốt

Thông báo lỗi là cuộc hội thoại giữa code và runtime. AI là phiên dịch viên, biến thông báo kỹ thuật khó hiểu thành giải thích rõ ràng và cách sửa có thể hành động được. Càng phân tích nhiều lỗi với AI, bạn sẽ càng giỏi đọc chúng.
