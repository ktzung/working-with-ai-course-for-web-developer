# Phòng thí nghiệm gỡ lỗi: Sửa 10 dự án có lỗi

## Chào mừng đến phòng thí nghiệm gỡ lỗi

Lý thuyết hay, nhưng gỡ lỗi là kỹ năng thực hành. Trong bài học này, bạn sẽ làm việc qua 10 dự án web có lỗi cố ý. Mỗi dự án có một lỗi cụ thể bạn cần tìm và sửa bằng các kỹ thuật gỡ lỗi với AI đã học.

## Cách thức hoạt động

Với mỗi dự án:
1. Đọc mô tả lỗi
2. Kiểm tra code
3. Dùng AI để chẩn đoán vấn đề
4. Triển khai sửa lỗi
5. Xác minh giải pháp hoạt động

Tự đo thời gian cho mỗi lỗi. Mục tiêu là giải quyết mỗi lỗi trong dưới 15 phút với sự hỗ trợ của AI.

## Lỗi #1: Vòng lặp vô hạn

**Dự án:** Ứng dụng todo React
**Triệuứng:** App đóng băng khi thêm todo mới
**Gợi ý:** Kiểm tra dependencies useEffect

**Prompt AI để bắt đầu:** "Ứng dụng todo React này đóng băng khi tôi thêm task. Đây là component TodoList. Điều gì gây ra vòng lặp vô hạn?"

Lỗi là useEffect cập nhật state khiến trigger cùng một useEffect. AI sẽ phát hiện vấn đề mảng dependency ngay lập tức.

## Lỗi #2: Bóng ma CORS

**Dự án:** Blog Express + React
**Triệuứng:** API hoạt động trên Postman nhưng lỗi trên trình duyệt
**Gợi ý:** Xem cấu hình server

**Prompt AI để bắt đầu:** "API trả về dữ liệu trên Postman nhưng trình duyệt hiển thị lỗi CORS. Đây là thiết lập server Express."

Lỗi là thiếu CORS middleware hoặc cấu hình origin sai. AI sẽ giải thích preflight requests và sửa thiết lập.

## Lỗi #3: Lỗi im lặng

**Dự án:** Hệ thống xác thực người dùng
**Triệuứng:** Đăng nhập có vẻ hoạt động nhưng dữ liệu người dùng undefined
**Gợiý:** Kiểm tra sử dụng async/await

**Prompt AI để bắt đầu:** "Đăng nhập trả về thành công nhưng dữ liệu người dùng undefined. Đây là auth context và hàm đăng nhập."

Lỗi là thiếu `await` trên hàm async, khiến promise được trả về thay vì giá trị đã giải quyết.

## Lỗi #4: Bộ nhớ khổng lồ

**Dự án:** Ứng dụng thư viện hình ảnh
**Triệuứng:** Tab trình duyệt dùng 2GB bộ nhớ sau khi duyệt
**Gợiý:** Xem event listeners và xử lý hình ảnh

**Prompt AI để bắt đầu:** "Thư viện hình ảnh tiêu thụ bộ nhớ khổng lồ. Đây là component gallery và code tải hình ảnh."

Lỗi là event listeners được thêm trong vòng lặp mà không cleanup, cộng thêm hình ảnh tải không giới hạn kích thước.

## Lỗi #5: Điều kiện đua

**Dự án:** Ứng dụng chat thời gian thực
**Triệuứng:** Tin nhắn xuất hiện không theo thứ tự hoặc trùng lặp
**Gợiý:** Kiểm tra xử lý tin nhắn WebSocket

**Prompt AI để bắt đầu:** "Tin nhắn chat đến không theo thứ tự và đôi khi trùng lặp. Đây là WebSocket handler và quản lý state tin nhắn."

Lỗi là cập nhật state từ tin nhắn WebSocket nhanh ghi đè lẫn nhau.

## Lỗi #6: Timer zombie

**Dự án:** Widget đếm ngược
**Triệuứng:** Timer tiếp tục chạy sau khi điều hướng đi và quay lại
**Gợiý:** Kiểm tra cleanup useEffect

**Prompt AI để bắt đầu:** "Timer đếm ngược tiếp tục chạy ngay cả sau khi tôi điều hướng đi và quay lại. Đây là component Timer."

Lỗi là setInterval không bao giờ được xóa khi unmount.

## Lỗi #7: Bóng ma form

**Dự án:** Wizard form nhiều bước
**Triệuứng:** Dữ liệu form biến mất khi quay lại bước trước
**Gợiý:** Kiểm tra quản lý state giữa các bước

**Prompt AI để bắt đầu:** "Dữ liệu form bị mất khi điều hướng giữa các bước. Đây là component wizard form."

Lỗi là state bị reset khi component unmount giữa các bước.

## Lỗi #8: Kẻ mạo danh API

**Dự án:** Dashboard thời tiết
**Triệuứng:** Hiển thị thời tiết hôm qua dù đã làm mới
**Gợiý:** Kiểm tra caching và gọi API

**Prompt AI để bắt đầu:** "Dashboard thời tiết hiển thị dữ liệu cũ. Đây là lệnh gọi API và logic lấy dữ liệu."

Lỗi là caching trình duyệt quá aggressive với phản hồi API mà không có cache-busting đúng cách.

## Lỗi #9: Phá vỡ style

**Dự án:** Menu điều hướng responsive
**Triệuứng:** Menu hoạt động trên desktop nhưng đè lên nội dung trên mobile
**Gợiý:** Kiểm tra media queries CSS và z-index

**Prompt AI để bắt đầu:** "Menu nav đè lên nội dung trên mobile. Đây là CSS và cấu trúc component."

Lỗi là thiếu hoặc sai media queries kết hợp với vấn đề z-index.

## Lỗi #10: Vượt qua xác thực

**Dự án:** Dashboard quản trị
**Triệuứng:** Người dùng thường có thể truy cập route admin
**Gợiý:** Kiểm tra middleware bảo vệ route

**Prompt AI để bắt đầu:** "Người dùng thường có thể truy cập trang admin. Đây là bảo vệ route và auth middleware."

Lỗi là bảo vệ route chỉ phía client mà không có xác minh phía server.

## Sau phòng thí nghiệm

Khi đã sửa cả 10 lỗi, hãy phản ánh về các mẫu:

1. Kỹ thuật gỡ lỗi nào hoạt động tốt nhất cho mỗi loại lỗi?
2. AI đã giúp bạn thu hẹp vấn đề nhanh hơn như thế nào?
3. Lỗi phổ biến nào bạn sẽ chú ý trong code của mình?

## Xây dựng bộ công cụ gỡ lỗi

Nhờ AI giúp bạn tạo danh mục gỡ lỗi cá nhân:

"Dựa trên 10 lỗi vừa sửa, tạo danh mục gỡ lỗi tôi có thể dùng cho dự án tương lai. Phân loại theo: React, API, CSS, Bảo mật."

## Điểm mấu chốt

Gỡ lỗi là kỹ năng cải thiện qua thực hành. Bằng cách làm việc qua 10 lỗi này với sự hỗ trợ của AI, bạn đã xây dựng trí nhớ cơ bắp cho các vấn đề phát triển web phổ biến. Lần sau gặp vấn đề tương tự, bạn sẽ biết chính xác cần xem ở đâu và hỏi AI câu gì.
