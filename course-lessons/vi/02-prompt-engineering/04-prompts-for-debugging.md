# Prompt cho Debugging

## Debug với AI

Bug là điều không thể tránh. Điều quan trọng là bạn tìm và sửa nhanh đến đâu. Công cụ AI rất giỏi trong việc debug — nếu bạn biết cách prompt hiệu quả.

## Công thức Prompt Debugging

Một prompt debug tốt bao gồm:

1. **Thông báo lỗi** — Copy chính xác lỗi
2. **Đoạn code** — Hiển thị code liên quan
3. **Context** — Bạn kỳ vọng gì vs. thực tế xảy ra gì
4. **Đã thử gì** — Các cách đã thử sửa

## Debug lỗi TypeScript

### Type không khớp

```
Tôi gặp lỗi TypeScript này:

Type 'string | undefined' is not assignable to type 'string'.
  Type 'undefined' is not assignable to type 'string'.

Trong đoạn code này:
const user: User = {
  name: data.name,
  email: data.email,
  avatar: data.avatar, // Lỗi ở đây
};

Dữ liệu đến từ response API mà avatar là tùy chọn.
Tôi nên xử lý thế nào cho đúng?
```

### Vấn đề Generic Type

```
Lỗi TypeScript trong component React:

No overload matches this call.
  Overload 1 of 2, '(props: ProjectCardProps): ProjectCard', gave the following error.

Đây là component:
[paste code component]

Và cách tôi dùng nó:
[paste code sử dụng]

Tôi nghĩ vấn đề ở kiểu props. Bạn giúp được không?
```

## Debug lỗi React

### Lỗi Hydration

```
Tôi gặp lỗi hydration mismatch trong Next.js:

Warning: Text content did not match. Server: "Loading..." Client: "5 projects"

Xảy ra trong component ProjectCount:
[paste code component]

Component fetch dữ liệu phía client, nhưng server render nội dung khác.
Sửa thế nào cho đúng trong Next.js 14 với App Router?
```

### Re-render vô hạn

```
Component React bị kẹt trong vòng lặp re-render vô hạn.

Console hiển thị: "Maximum update depth exceeded"

Đây là component:
[paste code component]

useEffect có vẻ liên tục kích hoạt. Tôi nghĩ vì:
- Tôi dùng object làm dependency
- Hàm fetch tạo reference mới mỗi render

Sửa đúng cách là gì? Nên dùng useCallback, useMemo, hay tái cấu trúc effect?
```

### Vấn đề cập nhật State

```
State React không cập nhật đúng.

Khi tôi nhấn "Thêm Task", task hiện thoáng rồi biến mất.
Tôi nghĩ là vấn đề stale closure nhưng không chắc.

Đây là code:
[paste code component]

Hàm addTask được gọi từ handler onClick.
State tasks có vẻ reset sau API call.
```

## Debug CSS/Styling

### Tailwind không hoạt động

```
Class Tailwind CSS không được áp dụng.

Tôi có nút với các class này:
className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"

Nhưng nút hiện không có style. tailwind.config.js:
[paste config]

Và file CSS:
[paste CSS]

Có thể vấn đề ở đâu?
```

### Vấn đề Layout

```
Layout flexbox bị lỗi trên mobile.

Tôi có layout sidebar + nội dung chính:
[paste HTML/JSX]

Trên desktop đúng, nhưng trên mobile:
- Sidebar chồng lên nội dung
- Nội dung không cuộn đúng

Tôi dùng Tailwind CSS. Sửa responsive thế nào?
```

## Debug API/Network

### Lỗi CORS

```
Tôi gặp lỗi CORS khi gọi API từ frontend:

Access to fetch at 'http://localhost:3001/api/projects' from origin 
'http://localhost:3000' has been blocked by CORS policy.

API là app Next.js chạy port 3001.
Frontend là app React riêng chạy port 3000.

Cấu hình CORS thế nào cho đúng?
```

### Lỗi Fetch

```
Request fetch bị lỗi 400, nhưng API hoạt động trên Postman.

Đây là code fetch:
[paste code]

Body request trên Postman:
[paste body]

Response lỗi:
[paste lỗi]

Tôi nghĩ vấn đề ở cách gửi body. Bạn giúp được không?
```

## Debug Database

### Lỗi Prisma

```
Prisma throw lỗi này:

Invalid `prisma.project.findMany()` invocation:
Unknown arg `include` in include.tasks for type Project

Schema Prisma:
[paste schema]

Truy vấn:
[paste query]

Tôi nghĩ đã thiết lập quan hệ đúng. Sai ở đâu?
```

### Vấn đề Migration

```
Tôi gặp lỗi khi chạy prisma migrate:

Error: P3006
Migration `20240101_add_tasks` failed to apply cleanly to the shadow database.

File migration:
[paste migration]

Tôi có dữ liệu hiện có trong database. Xử lý an toàn thế nào?
```

## Debug hiệu suất

### Render chậm

```
Component React render chậm. Profiler cho thấy nó re-render 
mỗi lần gõ trong input tìm kiếm.

Cấu trúc component:
[paste code]

SearchInput ở component cha, danh sách kết quả ở component con.
Tôi nghĩ component cha re-render quá nhiều.

Tối ưu thế nào? React.memo, useMemo, hay tái cấu trúc?
```

### Rò rỉ bộ nhớ

```
Bộ nhớ app tăng liên tục. Sau vài phút điều hướng giữa các trang, 
app trở nên rất chậm.

Tôi dùng useEffect để fetch dữ liệu:
[paste code]

Tôi nghĩ không cleanup đúng. Cần thêm gì?
```

## Best Practice Debugging

### Cung cấp đủ context
```
// ❌ Quá mơ hồ
"Giúp tôi, code không chạy!"

// ✅ Context tốt
"Tôi đang xây app Next.js 14 với TypeScript. Component ProjectCard 
throw lỗi 'Cannot read property map of undefined' khi dự án 
không có task. Đây là code liên quan..."
```

### Bao gồm stack trace lỗi
```
Error: Cannot read properties of undefined (reading 'map')
    at ProjectCard (src/components/ProjectCard.tsx:15:23)
    at renderWithHooks (react-dom.development.js:14985:18)
    at mountIndeterminateComponent (react-dom.development.js:17811:13)
```

### Hiển thị những gì đã thử
```
Tôi đã thử:
1. Thêm optional chaining (project.tasks?.map) — vẫn lỗi
2. Thêm giá trị mặc định (project.tasks || []) — được nhưng không hay
3. Thêm kiểm tra loading — không giúp, tasks là undefined không phải loading

Cách đúng để xử lý là gì?
```

## Bài tập thực hành

Debug các vấn đề WebDevHub:

1. **Lỗi Type**: Component mong đợi `Project[]` nhưng nhận `Project | undefined`
2. **Vòng lặp Re-render**: Bảng Kanban re-render khi kéo card
3. **Lỗi API**: Endpoint tạo snippet trả về 500

Viết prompt debug chi tiết cho mỗi vấn đề, sau đó sửa với công cụ AI.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ học cách prompt AI cho styling — tạo thiết kế đẹp, responsive với Tailwind CSS và kỹ thuật CSS hiện đại.
