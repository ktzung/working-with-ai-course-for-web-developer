# Gỡ lỗi có hệ thống với AI

## Tư duy gỡ lỗi

Mọi lập trình viên web đều biết cảm giác này: thứ gì đó bị lỗi, và bạn dành hàng giờ nhìn chằm chằm vào code, thêm `console.log` một cách ngẫu nhiên, hy vọng tìm ra câu trả lời. Nếu bạn có một người bạn đồng hành có hệ thống, giúp bạn thu hẹp vấn đề trong vài phút thay vì vài giờ thì sao?

Gỡ lỗi với AI không phải là thay thế kỹ năng giải quyết vấn đề của bạn. Mà là cung cấp cho bạn một khuôn khổ có cấu trúc để điều tra vấn đề nhanh hơn và kỹ lưỡng hơn.

## Quy trình gỡ lỗi với AI

Đây là năm bước đã được kiểm chứng:

### Bước 1: Mô tả vấn đề rõ ràng

Trước khi nhờ AI giúp, hãy thu thập thông tin. Chính xác điều gì đang xảy ra? Bạn mong đợi điều gì? Khi nào nó bắt đầu?

**Prompt kém:** "Ứng dụng của tôi bị lỗi, giúp tôi!"

**Prompt tốt:** "Sau khi nhấn nút đăng nhập, trang hiển thị màn hình trắng thay vì chuyển hướng đến trang tổng quan. Việc này bắt đầu sau khi tôi cập nhật React Router từ v5 lên v6. Console không hiển thị lỗi, nhưng tab network cho thấy API trả về 200 với dữ liệu người dùng hợp lệ."

Càng nhiều ngữ cảnh bạn cung cấp, AI càng giúp bạn tốt hơn.

### Bước 2: Cô lập vấn đề

Nhờ AI giúp bạn tạo cây giả thuyết. Ví dụ:

"Tôi bị màn hình trắng sau đăng nhập. Hãy giúp tôi tạo cây quyết định để xác định vấn đề nằm ở cấu hình routing, quản lý trạng thái xác thực, hay render component."

AI sẽ gợi ý các kiểm tra cụ thể tại mỗi tầng, giúp bạn thu hẹp nguyên nhân một cách có hệ thống.

### Bước 3: Kiểm tra bằng chứng

Chia sẻ các đoạn code liên quan, thông báo lỗi và file cấu hình với AI. Hỏi nó phân tích các mẫu:

"Đây là cấu hình router, auth context và login handler của tôi. Bạn có thể xác định bất kỳ sự không tương thích nào giữa mẫu React Router v5 và cú pháp v6 không?"

### Bước 4: Triển khai và kiểm tra bản sửa

Khi AI đề xuất sửa lỗi, đừng chỉ copy-paste. Hỏi nó giải thích tại sao thay đổi này sẽ hoạt động, và cần kiểm tra gì sau đó:

"Tại sao việc thay đổi `<Switch>` thành `<Routes>` lại sửa được lỗi này? Tôi cần cập nhật những mẫu v5 nào khác trong file này?"

### Bước 5: Tài liệu hóa giải pháp

Nhờ AI giúp bạn viết commit message hoặc thêm comment giải thích bản sửa. Điều này xây dựng kiến thức gỡ lỗi cho nhóm của bạn.

## Các tình huống gỡ lỗi phổ biến

**Lỗi quản lý trạng thái:** "Component của tôi render vô hạn. Đây là useEffect và state mà nó phụ thuộc." AI có thể phát hiện vấn đề mảng dependency, thiếu hàm cleanup, hoặc vòng lặp cập nhật state.

**Vấn đề tích hợp API:** "Lệnh fetch của tôi hoạt động trên Postman nhưng lỗi trên trình duyệt với lỗi CORS." AI có thể giải thích CORS preflight requests và gợi ý headers hoặc cấu hình proxy phù hợp.

**Vấn đề CSS/Layout:** "Container flex này hoạt động trên desktop nhưng tràn trên mobile." AI có thể phân tích CSS và gợi ý điều chỉnh responsive.

**Lỗi build:** "npm run build thất bại với 'Module not found' nhưng file tồn tại." AI có thể kiểm tra đường dẫn import, phân biệt chữ hoa/thường, và cấu hình webpack/vite.

## Mẹo hay khi gỡ lỗi với AI

1. **Chia sẻ thông báo lỗi đầy đủ**, không chỉ tóm tắt. AI có thể phân tích stack trace để xác định chính xác file và dòng lỗi.

2. **Bao gồm thông tin môi trường.** Phiên bản Node, trình duyệt, hệ điều hành — chúng quan trọng hơn bạn nghĩ.

3. **Mô tả những gì đã thay đổi gần đây.** "Nó hoạt động hôm qua" là manh mối mạnh mẽ.

4. **Hỏi AI giải thích, không chỉ sửa.** Hiểu tại sao thứ gì đó bị lỗi sẽ ngăn nó xảy ra lại.

5. **Dùng AI tạo các bước tái hiện.** Một ví dụ có thể tái hiện tối thiểu thường揭示 nguyên nhân gốc.

## Bài tập thực hành

Lấy một lỗi bạn đang gặp phải. Viết mô tả vấn đề chi tiết theo mẫu trên. Nhờ AI giúp bạn tạo cây giả thuyết, sau đó làm việc qua từng nhánh một cách có hệ thống. Đo thời gian — bạn sẽ ngạc nhiên vì giải quyết nhanh hơn bao nhiêu.

## Điểm mấu chốt

AI không thay thế trực giác gỡ lỗi — nó khuếch đại trực giác đó. Bằng cách cung cấp phân tích có cấu trúc và gợi ý các hướng điều tra, AI biến việc khắc phục sự cố hỗn loạn thành quy trình có phương pháp. Chìa khóa là học cách giao tiếp vấn đề rõ ràng và đặt câu hỏi đúng tại mỗi bước.
