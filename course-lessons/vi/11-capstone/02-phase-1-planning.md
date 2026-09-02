# Giai đoạn 1: Lập kế hoạch — Yêu cầu, kiến trúc, thiết lập

## Nền tảng của dự án tuyệt vời

Mọi dự án thành công đều bắt đầu từ kế hoạch vững chắc. Trong bài học này, bạn sẽ dùng AI để xác định yêu cầu, thiết kế kiến trúc và thiết lập môi trường phát triển cho TaskFlow.

## Bước 1: Xác định yêu cầu với AI

Bắt đầu bằng mô tả tầm nhìn cho AI:

"Tôi muốn xây công cụ quản lý dự án tên TaskFlow. Nó cần hỗ trợ xác thực người dùng, không gian làm việc nhóm, bảng kanban với kéo thả, cộng tác thời gian thực, tệp đính kèm và dashboard. Giúp tôi tạo user stories chi tiết cho mỗi tính năng."

AI sẽ tạo user stories như:

**Xác thực:**
- Người dùng có thể đăng ký bằng email và mật khẩu
- Người dùng có thể đăng nhập và duy trì xác thực qua các phiên
- Người dùng có thể đặt lại mật khẩu qua email

**Không gian làm việc:**
- Người dùng có thể tạo không gian làm việc nhóm
- Người dùng có thể mời thành viên qua email
- Người dùng có thể quản lý vai trò thành viên (admin, thành viên, người xem)

**Bảng:**
- Người dùng có thể tạo nhiều bảng cho mỗi không gian
- Người dùng có thể tạo, chỉnh sửa và xóa cột
- Người dùng có thể kéo thả task giữa các cột

## Bước 2: Thiết kế schema database

Nhờ AI giúp thiết kế mô hình dữ liệu:

"Thiết kế schema PostgreSQL cho TaskFlow. Bao gồm bảng cho người dùng, không gian làm việc, bảng, cột, task, bình luận và tệp đính kèm. Cho thấy mối quan hệ và indexes."

AI sẽ tạo schema đầy đủ với:
- Foreign keys và mối quan hệ đúng
- Indexes cho truy vấn phổ biến
- Timestamps và soft deletes
- Enum types cho trạng thái và vai trò

## Bước 3: Lập kế hoạch API

"Thiết kế endpoints RESTful API cho TaskFlow. Bao gồm xác thực, thao tác CRUD cho tất cả tài nguyên và mẫu xử lý lỗi."

AI sẽ tạo đặc tả API bao gồm:
- Endpoints auth (đăng ký, đăng nhập, làm mới, đăng xuất)
- CRUD không gian làm việc
- Quản lý bảng và cột
- Thao tác task với lọc và sắp xếp
- Endpoints tải tệp

## Bước 4: Quyết định kiến trúc

Thảo luận kiến trúc với AI:

"Mẫu kiến trúc nào nên dùng cho ứng dụng React + Express với tính năng thời gian thực? Cân nhắc monorepo vs repo riêng, cách tiếp cận quản lý state và tích hợp WebSocket."

AI sẽ giúp bạn quyết định:
- **Monorepo** với types chia sẻ giữa frontend và backend
- **Zustand** cho state client (đơn giản hơn Redux cho quy mô này)
- **Socket.io** rooms cho cập nhật thời gian thực cấp không gian làm việc
- **Prisma** cho truy cập database type-safe

## Bước 5: Thiết lập dự án

"Giúp tôi thiết lập monorepo với frontend React và backend Express dùng TypeScript. Bao gồm ESLint, Prettier và cấu hình chia sẻ."

AI sẽ hướng dẫn:
1. Tạo cấu trúc dự án
2. Cấu hình TypeScript cho cả frontend và backend
3. Thiết lập ESLint và Prettier
4. Tạo định nghĩa type chia sẻ
5. Thiết lập scripts phát triển

## Bước 6: Tạo bảng dự án

"Tôi cần tổ chức task phát triển cho TaskFlow. Giúp tôi tạo phân tích task với ưu tiên và phụ thuộc."

AI sẽ giúp bạn tạo lộ trình phát triển với:
- Task được tổ chức theo tính năng
- Mức ưu tiên (P0, P1, P2)
- Phụ thuộc giữa các task
- Ước tính nỗ lực cho mỗi task

## Sản phẩm Giai đoạn 1

Đến cuối giai đoạn này, bạn nên có:

- [ ] Tài liệu user stories
- [ ] Schema database (SQL hoặc Prisma schema)
- [ ] Đặc tả API
- [ ] Tài liệu quyết định kiến trúc
- [ ] Thiết lập dự án với tất cả công cụ
- [ ] Bảng task phát triển

## Điểm mấu chốt

Lập kế hoạch với AI biến ý tưởng mơ hồ thành bản thiết kế cụ thể. Bạn đã xác định xây gì, cấu trúc thế nào và tổ chức công việc. Giờ bạn sẵn sàng bắt đầu xây.
