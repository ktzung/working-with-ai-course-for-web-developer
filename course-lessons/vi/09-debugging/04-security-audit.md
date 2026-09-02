# Kiểm tra bảo mật với AI

## Bảo mật không phải tùy chọn

Mỗi 39 giây, một cuộc tấn công mạng xảy ra somewhere trên web. Là lập trình viên web, bạn đang ở tuyến đầu phòng thủ. Nhưng bảo mật rất rộng — SQL injection, XSS, CSRF, lỗi xác thực, cấu hình sai header... Bắt đầu từ đâu?

AI có thể là chuyên gia tư vấn bảo mật, giúp bạn xác định lỗ hổng, hiểu vectơ tấn công và triển khai phòng thủ phù hợp.

## OWASP Top 10 với AI

OWASP Top 10 liệt kê các rủi ro bảo mật ứng dụng web quan trọng nhất. Hãy xem AI giúp thế nào:

### 1. Kiểm soát truy cập bị hỏng
**Prompt:** "Kiểm tra middleware Express của tôi về lỗ hổng phân quyền. Đây là mã bảo vệ route."

AI sẽ kiểm tra thiếu kiểm tra phân quyền, tham chiếu đối tượng trực tiếp không an toàn và đường dẫn leo thang đặc quyền.

### 2. Lỗi mã hóa
**Prompt:** "Tôi đang lưu mật khẩu người dùng bằng MD5. Làm thế nào để chuyển sang bcrypt?"

AI sẽ cung cấp chiến lược di cư, giải thích tại sao MD5 bị phá vỡ và hiển thị triển khai băm mật khẩu đúng cách.

### 3. Injection
**Prompt:** "Đây là mã truy vấn database. Tôi có dễ bị SQL injection không?"

AI sẽ xác định nối chuỗi trong truy vấn và chỉ bạn sử dụng parameterized queries hoặc ORM.

### 4. Thiết kế không an toàn
**Prompt:** "Kiểm tra luồng xác thực của tôi về các vấn đề bảo mật cấp thiết kế."

AI sẽ phân tích kiến trúc về thiếu giới hạn tốc độ, thiếu xác thực đa yếu tố và luồng đặt lại mật khẩu không an toàn.

### 5. Cấu hình bảo mật sai
**Prompt:** "Kiểm tra cấu hình Express/Helmet về thiếu header bảo mật."

AI sẽ kiểm tra chính sách CORS, Content Security Policy và các header bảo mật HTTP khác.

## Quy trình kiểm tra bảo mật với AI

### Bước 1: Quét tự động

Nhờ AI kiểm tra codebase về các lỗ hổng phổ biến:

"Quét dự án React và Node.js của tôi về vấn đề bảo mật. Tập trung vào xác thực, xử lý dữ liệu và API endpoints."

### Bước 2: Kiểm tra dependency

"package.json của tôi có 47 dependency. Có package nào có lỗ hổng bảo mật đã biết không?"

AI sẽ phân tích dependencies và đề xuất cập nhật hoặc thay thế cho các package có CVE đã biết.

### Bước 3: Kiểm tra cấu hình

"Kiểm tra xử lý .env, cấu hình CORS và cài đặt cookie của tôi về thực hành bảo mật tốt nhất."

AI sẽ kiểm tra bí mật bị lộ, CORS quá thoáng và cấu hình cookie không an toàn.

### Bước 4: Phân tích cấp code

"Đây là endpoint đăng ký người dùng. Kiểm tra nó về vấn đề bảo mật."

AI sẽ kiểm tra xác thực đầu vào, yêu cầu độ mạnh mật khẩu, xác minh email và giới hạn tốc độ.

## Các lỗ hổng phổ biến AI phát hiện

### Cross-Site Scripting (XSS)
"Tôi render bình luận người dùng với `dangerouslySetInnerHTML`. Điều này an toàn không?"

AI sẽ giải thích rủi ro XSS và đề xuất khử trùng với DOMPurify hoặc sử dụng phương thức render an toàn.

### Cross-Site Request Forgery (CSRF)
"Form của tôi gửi đến `/api/transfer`. Làm thế nào ngăn tấn công CSRF?"

AI sẽ triển khai CSRF tokens và giải thích thuộc tính cookie SameSite.

### Tham chiếu đối tượng trực tiếp không an toàn (IDOR)
"Người dùng có thể truy cập `/api/orders/123`. Làm thế nào đảm bảo họ chỉ xem đơn hàng của mình?"

AI sẽ thêm xác minh quyền sở hữu vào logic truy vấn.

### Lộ dữ liệu nhạy cảm
"API trả về toàn bộ đối tượng người dùng bao gồm băm mật khẩu. Sửa thế nào?"

AI sẽ chỉ bạn cách chọn trường cụ thể và không bao giờ lộ dữ liệu nhạy cảm.

## Xây dựng danh mục kiểm tra bảo mật

Nhờ AI tạo danh mục kiểm tra bảo mật cụ thể cho dự án:

"Tạo danh mục kiểm tra bảo mật cho ứng dụng MERN stack thương mại điện tử. Bao gồm các mục cho xác thực, xử lý dữ liệu, bảo mật API và triển khai."

Đây sẽ trở thành cổng bảo mật trước triển khai. Kiểm tra qua nó trước mỗi lần phát hành.

## Header bảo mật dễ dàng

AI có thể tạo cấu hình header bảo mật hoàn hảo:

"Tạo cấu hình Helmet.js với tất cả header bảo mật được khuyến nghị cho ứng dụng Express production."

Bạn sẽ nhận thiết lập đầy đủ bao gồm CSP, HSTS, X-Frame-Options và nhiều hơn.

## Bài tập thực hành

Chọn một phần của dự án hiện tại và thực hiện kiểm tra bảo mật:

1. Nhờ AI kiểm tra code về lỗ hổng OWASP Top 10
2. Với mỗi vấn đề tìm thấy, nhờ AI giải thích kịch bản tấn công
3. Triển khai các sửa lỗi AI đề xuất
4. Nhờ AI xác minh các sửa lỗi đúng
5. Ghi lại các mẫu bảo mật bạn học được

## Điểm mấu chốt

Kiểm tra bảo mật với AI biến nhiệm vụ đáng sợ thành quy trình có thể quản lý. AI có thể phát hiện lỗ hổng phổ biến, giải thích vectơ tấn công và cung cấp sửa lỗi sẵn sàng triển khai. Nhưng nhớ — AI là công cụ, không thay thế chuyên môn bảo mật. Sử dụng nó để bắt các vấn đề rõ ràng, sau đó cân nhắc kiểm tra thâm nhập chuyên nghiệp cho ứng dụng quan trọng.
