# Project Context là gì?

## Tại sao AI cần context dự án đầy đủ

Hãy tưởng tượng yêu cầu lập trình viên mới vào team bắt đầu code ngay — mà không cho xem codebase, kiến trúc hay coding standard. Họ sẽ tạo code không phù hợp. Điều tương tự xảy ra với AI khi thiếu context.

## Vấn đề Context

Công cụ AI tạo code dựa trên pattern đã học. Nếu thiếu context dự án, chúng sẽ:

- Dùng pattern chung thay vì quy ước team bạn
- Gợi ý công nghệ bạn không dùng
- Tạo code không tích hợp với kiến trúc hiện có
- Bỏ sót ràng buộc và yêu cầu quan trọng

## Project Context là gì?

Project context là thông tin AI cần để tạo code phù hợp, nhất quán cho dự án cụ thể. Bao gồm:

### 1. Context kỹ thuật
- **Tech stack**: Framework, ngôn ngữ, thư viện, phiên bản
- **Kiến trúc**: Cấu trúc app (monolith, microservices, etc.)
- **Database**: Schema, ORM, quan hệ
- **Thiết kế API**: Pattern REST, GraphQL, tRPC

### 2. Context codebase
- **Cấu trúc file**: Vị trí các loại file khác nhau
- **Quy ước đặt tên**: Cách đặt tên file, hàm và biến
- **Pattern code**: Pattern phổ biến trong codebase
- **Component hiện có**: Những gì đã xây dựng

### 3. Context team
- **Coding standard**: Quy tắc lint, sở thích format
- **Yêu cầu review**: Người review tìm kiếm điều gì
- **Tiêu chuẩn test**: Kỳ vọng coverage, pattern test
- **Tiêu chuẩn tài liệu**: Cách ghi tài liệu code

### 4. Context dự án
- **Mục tiêu hiện tại**: Đang xây gì bây giờ
- **Ràng buộc**: Hiệu suất, accessibility, hỗ trợ trình duyệt
- **Deadline**: Bao nhiêu độ phức tạp chấp nhận được
- **Yêu cầu người dùng**: Ai dùng app và dùng thế nào

## Tác động của Context tốt

Không có context:
```
Prompt: "Tạo component hồ sơ người dùng"
Kết quả: Component chung với trường cơ bản
```

Có context:
```
Prompt: "Tạo component hồ sơ người dùng"
Context: [project-context.md cho thấy Next.js 14, TypeScript, Tailwind,
          User type hiện có, design system cụ thể, yêu cầu accessibility]
Kết quả: Component khớp kiến trúc chính xác, dùng design token,
         tuân thủ quy ước đặt tên, tích hợp code hiện có
```

## Nguồn Context

### Context tự động
Công cụ AI có thể tự động thu thập:
- File đang mở trong editor
- Cấu trúc file dự án
- Dependencies trong package.json
- Kiểu và interface TypeScript

### Context thủ công
Bạn cần cung cấp:
- Quyết định kiến trúc
- Coding standard
- Mô tả task hiện tại
- Yêu cầu nghiệp vụ

### Context tài liệu
Tài liệu mô tả:
- Tổng quan và mục tiêu dự án
- Kiến trúc kỹ thuật
- Đặc tả API
- Design system

## Xây dựng lớp Context

Hãy coi context như các lớp:

```
Lớp 1: Nhận dạng dự án
├── Dự án này là gì?
├── Dùng tech stack gì?
└── Mục tiêu là gì?

Lớp 2: Kiến trúc
├── Code được tổ chức thế nào?
├── Dùng pattern gì?
└── Component tương tác thế nào?

Lớp 3: Tiêu chuẩn
├── Quy ước code
├── Yêu cầu test
└── Kỳ vọng tài liệu

Lớp 4: Task hiện tại
├── Đang xây gì bây giờ?
├── Yêu cầu cụ thể là gì?
└── Ràng buộc nào áp dụng?
```

## Context cho từng công cụ AI

### GitHub Copilot
- Tự động đọc file đang mở
- Dùng `.github/copilot-instructions.md` cho context cấp dự án
- Comment trong code cung cấp context cục bộ

### ChatGPT/Claude
- Bạn cung cấp context trong cuộc hội thoại
- Upload file context khi bắt đầu phiên
- Tham chiếu file cụ thể khi đặt câu hỏi

### Cursor
- Index toàn bộ codebase
- Dùng `.cursorrules` cho quy ước dự án
- Có thể tham chiếu bất kỳ file nào

## Tư duy Context Engineering

Context engineering là việc chủ động với thông tin cung cấp cho AI. Thay vì hy vọng AI tự hiểu dự án, bạn:

1. **Ghi chép** các quyết định quan trọng của dự án
2. **Cấu trúc** thông tin để AI có thể sử dụng
3. **Cập nhật** context khi dự án phát triển
4. **Chia sẻ** context trong team

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ tạo file context chính cho dự án — `project-context.md` — đóng vai trò nền tảng cho mọi tương tác AI.
