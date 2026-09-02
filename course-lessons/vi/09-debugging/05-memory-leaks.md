# Phát hiện và sửa rò rỉ bộ nhớ với AI

## Rò rỉ bộ nhớ là gì?

Rò rỉ bộ nhớ xảy ra khi ứng dụng cấp phát bộ nhớ nhưng không bao giờ giải phóng nó. Theo thời gian, bộ nhớ bị tiêu hao tích lũy, khiến ứng dụng chậm lại, đóng băng hoặc crash. Trong ứng dụng web, rò rỉ bộ nhớ đặc biệt tinh vi — chúng có thể không xuất hiện trong quá trình phát triển nhưng trở nên nghiêm trọng trong production khi người dùng giữ tab mở hàng giờ.

## Tại sao ứng dụng web bị rò rỉ bộ nhớ

Nguyên nhân phổ biến của rò rỉ bộ nhớ trong phát triển web:

- **Event listeners** được thêm nhưng không bao giờ xóa
- **Timers** (setInterval, setTimeout) không được xóa
- **Closures** giữ tham chiếu đến đối tượng lớn
- **DOM nodes bị tách** vẫn được tham chiếu trong JavaScript
- **Kết nối WebSocket** không được đóng đúng cách
- **Biến toàn cục** tích lũy dữ liệu theo thời gian

## Phát hiện rò rỉ bộ nhớ với AI

### Bước 1: Xác định triệu chứng

Mô tả những gì bạn quan sát:

"Ứng dụng React của tôi chạy mượt lúc đầu nhưng chậm lại sau 30 phút sử dụng. Tab trình duyệt dùng 800MB bộ nhớ. Làm mới tạm thời sửa được."

AI sẽ ngay lập tức nghi ngờ rò rỉ bộ nhớ và hướng dẫn bạn điều tra.

### Bước 2: Kiểm tra code với AI

Chia sẻ các component và nhờ AI kiểm tra rò rỉ:

"Kiểm tra các component React này về rò rỉ bộ nhớ. Tập trung vào cleanup useEffect, event listeners và subscriptions."

AI sẽ quét:
- useEffect không có hàm cleanup
- addEventListener không có removeEventListener
- setInterval không có clearInterval
- Subscriptions không có unsubscribe
- Thao tác async tiếp tục sau khi unmount

### Bước 3: Phân tích DevTools

Nhờ AI hướng dẫn bạn qua Chrome DevTools memory profiling:

"Hướng dẫn tôi sử dụng tab Memory trong Chrome DevTools để tìm rò rỉ bộ nhớ trong ứng dụng React. Tôi nên tìm gì trong heap snapshots?"

AI sẽ giải thích:
- Chụp heap snapshots trước và sau hành động
- So sánh snapshots để tìm số lượng đối tượng tăng
- Sử dụng Allocation timeline để phát hiện xu hướng
- Xác định retained objects và retainers

## Các mẫu rò rỉ bộ nhớ phổ biến trong React

### Thiếu cleanup useEffect

```javascript
// Code bị rò rỉ
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Thiếu cleanup!
}, []);

// Code đã sửa
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

**Prompt AI:** "Kiểm tra tất cả useEffect hooks của tôi về thiếu hàm cleanup."

### Rò rỉ Interval

```javascript
// Code bị rò rỉ
useEffect(() => {
  setInterval(() => fetchUpdates(), 5000);
}, []);

// Code đã sửa
useEffect(() => {
  const interval = setInterval(() => fetchUpdates(), 5000);
  return () => clearInterval(interval);
}, []);
```

### Bẫy bộ nhớ Closure

```javascript
// Rò rỉ: closure capture largeData
useEffect(() => {
  const handler = () => console.log(largeData);
  element.addEventListener('click', handler);
}, [largeData]);
```

AI sẽ đề xuất sử dụng refs hoặc tái cấu trúc để tránh capture không cần thiết.

### Cập nhật state sau unmount

```javascript
// Code bị rò rỉ
useEffect(() => {
  fetchData().then(data => setState(data));
  // Component có thể unmount trước khi fetch hoàn thành
}, []);

// Code đã sửa
useEffect(() => {
  let cancelled = false;
  fetchData().then(data => {
    if (!cancelled) setState(data);
  });
  return () => { cancelled = true; };
}, []);
```

## Rò rỉ bộ nhớ trong Node.js

Rò rỉ bộ nhớ phía server còn nghiêm trọng hơn:

**Prompt:** "Bộ nhớ server Node.js tăng từ 50MB lên 2GB trong 24 giờ. Đây là mã Express app."

AI sẽ kiểm tra:
- Kết nối database không đóng
- Event emitter listeners tích lũy
- Streams không được hủy đúng cách
- Global cache không có giới hạn kích thước
- Dữ liệu phạm vi request được lưu toàn cục

## Thiết lập giám sát bộ nhớ

Nhờ AI giúp triển khai giám sát bộ nhớ:

"Thêm giám sát sử dụng bộ nhớ vào Express app ghi cảnh báo khi sử dụng heap vượt 80%."

AI sẽ cung cấp code sử dụng `process.memoryUsage()` và đề xuất tích hợp với công cụ giám sát như Prometheus hoặc New Relic.

## Nâng cao: WeakRef và FinalizationRegistry

Để quản lý bộ nhớ nâng cao, AI có thể giải thích các tính năng JavaScript hiện đại:

"Khi nào tôi nên dùng WeakRef thay vì tham chiếu thông thường? Cho tôi ví dụ thực tế trong ứng dụng web."

AI sẽ trình diễn cách WeakRef cho phép garbage collection đối tượng được tham chiếu và khi nào mẫu này phù hợp.

## Bài tập thực hành

1. Mở dự án hiện tại trong Chrome DevTools
2. Chụp heap snapshot
3. Thực hiện hành động người dùng phổ biến (điều hướng, mở modal, gửi form)
4. Chụp heap snapshot khác
5. So sánh snapshots và chia sẻ phát hiện với AI
6. Nhờ AI giúp sửa bất kỳ rò rỉ nào tìm thấy

## Điểm mấu chốt

Rò rỉ bộ nhớ là kẻ giết người thầm lặng của hiệu suất ứng dụng web. AI xuất sắc trong việc kiểm tra code tìm mẫu rò rỉ, giải thích DevTools memory profiling và đề xuất sửa lỗi. Chìa khóa là học cách nhận ra triệu chứng — sử dụng bộ nhớ tăng, ngày càng chậm — và biết cách điều tra với hướng dẫn của AI.
