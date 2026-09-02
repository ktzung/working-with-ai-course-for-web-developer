# Phân tích hiệu suất với AI

## Tại sao hiệu suất quan trọng

Một giây trễ trong tải trang có thể giảm tỷ lệ chuyển đổi 7%. Người dùng kỳ vọng website tải trong dưới 3 giây. Google sử dụng Core Web Vitals làm yếu tố xếp hạng. Hiệu suất không phải tùy chọn — nó là thiết yếu.

Nhưng phân tích hiệu suất có thể choáng ngợp. Bắt đầu từ đâu? Chỉ số nào quan trọng? Làm sao diễn giải dữ liệu? AI có thể hướng dẫn bạn qua từng bước.

## Hiểu Core Web Vitals

Core Web Vitals của Google đo trải nghiệm người dùng thực:

### Largest Contentful Paint (LCP)
**Đo gì:** Thời gian cho đến khi phần tử lớn nhất hiển thị.
**Tốt:** Dưới 2.5 giây.
**Prompt AI:** "LCP của tôi là 4.2 giây. Đây là cấu trúc HTML và chiến lược tải hình ảnh. Làm thế nào để cải thiện?"

AI sẽ đề xuất preload hình ảnh hero, tối ưu định dạng hình ảnh, loại bỏ tài nguyên chặn render, và triển khai lazy loading.

### First Input Delay (FID) / Interaction to Next Paint (INP)
**Đo gì:** Mức độ phản hồi của trang với tương tác người dùng.
**Tốt:** Dưới 100ms (INP dưới 200ms).
**Prompt AI:** "INP của tôi là 350ms. Khi người dùng nhấn nút tìm kiếm, có độ trễ đáng kể. Đây là mã xử lý sự kiện."

AI sẽ xác định các tác vụ JavaScript chạy dài và đề xuất chia nhỏ chúng với `requestIdleCallback` hoặc web workers.

### Cumulative Layout Shift (CLS)
**Đo gì:** Mức độ nhảy layout trong quá trình tải.
**Tốt:** Dưới 0.1.
**Prompt AI:** "Điểm CLS của tôi là 0.25. Hình ảnh và quảng cáo đẩy nội dung xuống khi tải."

AI sẽ đề xuất đặt chiều rộng/chiều cao rõ ràng cho hình ảnh, giữ chỗ cho nội dung động, và sử dụng CSS `aspect-ratio`.

## Sử dụng Lighthouse với AI

Lighthouse là công cụ kiểm tra hiệu suất tích hợp trong Chrome. Chạy nó, sau đó chia sẻ kết quả với AI:

**Mẫu prompt:** "Đây là báo cáo Lighthouse. Điểm hiệu suất là 45. Đây là các cơ hội và chẩn đoán nó đánh dấu. Giúp tôi ưu tiên sửa lỗi nào sẽ có tác động lớn nhất."

AI sẽ xếp hạng các vấn đề theo tác động và nỗ lực, giúp bạn tập trung vào tối ưu hóa giá trị cao trước.

## Các vấn đề hiệu suất phổ biến AI có thể giúp sửa

### Kích thước bundle
"Bundle JavaScript của tôi là 2.5MB. Đây là cấu hình webpack và câu lệnh import."

AI sẽ đề xuất:
- Tree shaking code không dùng
- Dynamic imports cho code splitting theo route
- Thay thế thư viện nặng bằng lựa chọn nhẹ hơn
- Phân tích bundle với webpack-bundle-analyzer

### Tối ưu hình ảnh
"Trang tải 15MB hình ảnh. Làm thế nào tối ưu mà không mất chất lượng?"

AI sẽ đề xuất:
- Chuyển sang định dạng WebP/AVIF
- Triển khai hình ảnh responsive với `srcset`
- Lazy loading hình ảnh bên dưới màn hình
- Sử dụng CDN để phân phối hình ảnh

### Chiến lược caching
"Người dùng quay lại vẫn trải nghiệm tải chậm. Nên dùng chiến lược caching nào?"

AI sẽ giúp cấu hình:
- Headers caching trình duyệt (Cache-Control, ETag)
- Chiến lược caching service worker
- Cấu hình CDN
- Caching phản hồi API

### Script bên thứ ba
"Google Analytics, chat widget và nút mạng xã hội làm chậm trang."

AI sẽ đề xuất:
- Tải script bên thứ ba với `defer` hoặc `async`
- Sử dụng mẫu facade cho embeds
- Tự lưu trữ script quan trọng
- Triển khai tải dựa trên sự đồng ý

## Thiết lập giám sát hiệu suất

Nhờ AI giúp thiết lập giám sát hiệu suất liên tục:

"Giúp tôi thiết lập giám sát Web Vitals báo cáo đến bảng phân tích. Tôi muốn theo dõi LCP, FID, CLS và TTFB cho người dùng thực."

AI sẽ cung cấp code sử dụng thư viện `web-vitals` và chỉ cách gửi metrics đến nền tảng phân tích bạn chọn.

## Nâng cao: Hiệu suất runtime

Ngoài tốc độ tải, AI có thể giúp với hiệu suất runtime:

**Re-render React:** "Component bảng của tôi render lại mỗi khi bất kỳ state nào thay đổi. Đây là cây component." AI sẽ đề xuất memoization, virtualization và colocating state.

**Sử dụng bộ nhớ:** "Bộ nhớ ứng dụng tăng theo thời gian. Sau 10 phút sử dụng, nó dùng 500MB." AI sẽ giúp xác định rò rỉ bộ nhớ và đề xuất mẫu cleanup.

**Animation giật:** "Animation cuộn bị giật trên mobile." AI sẽ đề xuất sử dụng `transform` và `opacity` cho animation tăng tốc GPU.

## Bài tập thực hành

Chạy Lighthouse trên dự án hiện tại. Chia sẻ toàn bộ báo cáo với AI và nhờ nó:

1. Giải thích từng vấn đề bằng ngôn ngữ đơn giản
2. Xếp hạng vấn đề theo tác động người dùng
3. Cung cấp code sửa lỗi cụ thể cho 3 vấn đề hàng đầu
4. Thiết lập ngân sách hiệu suất cho dự án

## Điểm mấu chốt

Phân tích hiệu suất không cần phải đáng sợ. Với AI là hướng dẫn viên, bạn có thể hiểu metrics phức tạp, ưu tiên sửa lỗi hiệu quả và xây dựng ứng dụng web nhanh, phản hồi. Chìa khóa là chia sẻ dữ liệu cụ thể — điểm Lighthouse, kích thước bundle, metrics thời gian — và để AI dịch chúng thành cải tiến có thể hành động được.
