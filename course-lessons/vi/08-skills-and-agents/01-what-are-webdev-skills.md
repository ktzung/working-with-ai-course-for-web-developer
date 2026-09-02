# Kỹ năng phát triển web là gì?

## Mục tiêu học tập
- Hiểu kỹ năng AI là gì trong bối cảnh phát triển web
- Tìm hiểu cách kỹ năng khác với prompt và agent
- Xác định các quy trình phát triển web phổ biến trở thành kỹ năng

## Kỹ năng = Quy trình có thể tái sử dụng

Trong phát triển được hỗ trợ bởi AI, **kỹ năng** là một quy trình có thể tái sử dụng mà bạn có thể gọi lặp đi lặp lại để thực hiện một tác vụ cụ thể. Hãy nghĩ nó như một công thức đã lưu — một khi bạn tìm ra cách hoàn hảo để tạo thành phần, dựng API hoặc viết kiểm thử, bạn lưu quy trình đó dưới dạng kỹ năng.

**Không có kỹ năng**: Bạn viết cùng một prompt mỗi khi cần thành phần
**Có kỹ năng**: Bạn gọi kỹ năng biết chính xác những gì bạn cần

## Kỹ năng vs Prompt vs Agent

| Khái niệm | Đó là gì | Ví dụ |
|-----------|----------|-------|
| **Prompt** | Hướng dẫn một lần | "Tạo biểu mẫu đăng nhập với email và mật khẩu" |
| **Kỹ năng** | Quy trình có thể tái sử dụng với hướng dẫn | Kỹ năng tạo thành phần luôn theo mẫu của bạn |
| **Agent** | Trợ lý tự động sử dụng kỹ năng | Trình quét bảo mật chạy nhiều kiểm tra |

**Prompt** giống như đặt câu hỏi. **Kỹ năng** giống như có công thức. **Agent** giống như thuê chuyên gia.

## Cấu trúc của kỹ năng

Kỹ năng thường bao gồm:

1. **Trigger**: Khi nào sử dụng kỹ năng này
2. **Hướng dẫn**: Quy trình từng bước
3. **Mẫu**: Mẫu mã để theo dõi
4. **Ràng buộc**: Quy tắc và giới hạn
5. **Định dạng đầu ra**: Kỹ năng tạo ra gì

```markdown
# Kỹ năng tạo thành phần

## Trigger
Khi người dùng yêu cầu tạo thành phần React mới

## Hướng dẫn
1. Hỏi tên thành phần và props
2. Kiểm tra thành phần hiện có để tìm mẫu
3. Tạo thành phần với TypeScript
4. Bao gồm kiểm thử và câu chuyện Storybook
5. Theo dõi风格指南 của dự án

## Mẫu
- Thành phần chức năng với hook
- Interface props
- Tệp kiểm thử với React Testing Library
- Câu chuyện Storybook

## Ràng buộc
- Sử dụng TypeScript
- Theo dõi quy ước đặt tên
- Bao gồm thuộc tính accessibility
- Tối đa 200 dòng mỗi thành phần
```

## Kỹ năng phát triển web phổ biến

### 1. Tạo thành phần
Tạo thành phần React/Vue/Angular theo quy ước dự án.

### 2. Dự dựng API
Tạo route Express/FastAPI với controller, model và xác thực.

### 3. Tạo kiểm thử
Viết kiểm thử đơn vị, tích hợp và E2E cho mã hiện có.

### 4. Tái cấu trúc mã
Cải thiện chất lượng mã trong khi duy trì chức năng.

### 5. Tạo tài liệu
Tạo tài liệu API, tệp README và bình luận nội tuyến.

### 6. Migration cơ sở dữ liệu
Tạo script migration cho thay đổi schema.

## Tại sao kỹ năng quan trọng

1. **Nhất quán**: Mọi thành phần theo cùng một mẫu
2. **Tốc độ**: Tạo mẫu trong vài giây, không phải vài phút
3. **Chất lượng**: Kỹ năng编码 thực hành tốt nhất
4. **Chia sẻ kiến thức**: Kỹ năng nhóm捕捉 kiến thức tổ chức

## Tạo kỹ năng đầu tiên của bạn

```markdown
# Trình tạo route Express

## Trigger
Khi người dùng cần endpoint API mới

## Quy trình
1. Hỏi tên tài nguyên (ví dụ: "products")
2. Hỏi thao tác cần (CRUD hoặc tập con)
3. Tạo tệp route với:
   - Định nghĩa route
   - Nhập controller
   - Middleware (xác thực, xác nhận)
   - Xử lý lỗi
4. Tạo tệp controller với:
   - Trình xử lý bất đồng bộ
   - Thao tác cơ sở dữ liệu
   - Định dạng phản hồi
5. Tạo schema xác thực với Zod

## Đầu ra
- routes/[resource].js
- controllers/[resource]Controller.js
- validators/[resource]Validator.js
```

## Prompt AI cho tạo kỹ năng

```
Tạo kỹ năng có thể tái sử dụng để tạo thành phần React:
1. Theo dõi mẫu thành phần hiện có của dự án
2. Bao gồm interface props TypeScript
3. Tạo tệp kiểm thử với React Testing Library
4. Tạo câu chuyện Storybook
5. Thêm thuộc tính accessibility đúng cách
6. Theo dõi quy ước đặt tên

Kỹ năng phải được tài liệu hóa dưới dạng tệp markdown với trigger, hướng dẫn và mẫu rõ ràng.
```

## Bài tập thực hành

Tạo ba kỹ năng cho quy trình phát triển của bạn:
1. **Trình tạo thành phần**: Tạo thành phần React với kiểm thử
2. **Trình tạo endpoint API**: Tạo route Express với controller
3. **Trình tạo trang**: Tạo trang Next.js với bố cục

Tài liệu hóa mỗi kỹ năng với trigger, hướng dẫn và mẫu.

## Điểm chính

- Kỹ năng là quy trình có thể tái sử dụng编码 mẫu phát triển
- Chúng khác với prompt một lần và agent tự động
- Kỹ năng cải thiện tính nhất quán, tốc độ và chất lượng
- Bắt đầu bằng cách xác định tác vụ lặp đi lặp lại trong quy trình của bạn
