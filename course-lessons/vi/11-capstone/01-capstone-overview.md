# Tổng quan Capstone: Xây ứng dụng web hoàn chỉnh

## Dự án cuối cùng

Đây là đỉnh cao — kết tinh mọi thứ bạn đã học. Trong capstone này, bạn sẽ xây ứng dụng web hoàn chỉnh, sẵn sàng production từ đầu với AI là đối tác phát triển. Đây không phải dự án đồ chơi; nó là ứng dụng thực chứng minh kỹ năng với nhà tuyển dụng và khách hàng.

## Dự án: TaskFlow — Công cụ quản lý dự án nhóm

Bạn sẽ xây **TaskFlow**, ứng dụng quản lý dự án tương tự Trello hoặc Linear. Bao gồm:

- **Xác thực người dùng** (đăng ký, đăng nhập, đặt lại mật khẩu)
- **Không gian làm việc nhóm** (tạo nhóm, mời thành viên)
- **Bảng Kanban** (quản lý task kéo thả)
- **Cập nhật thời gian thực** (cộng tác qua WebSocket)
- **Tệp đính kèm** (tải lên và xem trước tệp)
- **Phân tích dashboard** (biểu đồ tiến độ dự án)
- **Thiết kế responsive** (hoạt động trên desktop và mobile)

## Tại sao dự án này?

Dự án này bao gồm mọi khái niệm phát triển web chính:

| Khái niệm | Xuất hiện ở đâu |
|-----------|-----------------|
| Xác thực | Đăng ký, đăng nhập, JWT tokens |
| Quản lý state | State bảng, tùy chọn người dùng |
| Thiết kế API | Endpoints RESTful, xử lý lỗi |
| Database | Dữ liệu người dùng, dự án, task |
| Thời gian thực | WebSocket cho cộng tác trực tiếp |
| Xử lý tệp | Tải lên, lưu trữ, xem trước |
| Hiệu suất | Lazy loading, tối ưu |
| Testing | Unit, integration, E2E tests |
| Triển khai | CI/CD, hosting, giám sát |

## Công nghệ

- **Frontend:** React 18 + TypeScript + Tailwind CSS
- **Backend:** Node.js + Express + TypeScript
- **Database:** PostgreSQL với Prisma ORM
- **Xác thực:** JWT + bcrypt
- **Thời gian thực:** Socket.io
- **Lưu trữ tệp:** AWS S3 hoặc lưu trữ cục bộ
- **Testing:** Jest + React Testing Library + Playwright
- **Triển khai:** Docker + Vercel/Railway

## AI hỗ trợ ở mỗi giai đoạn

### Giai đoạn lập kế hoạch
AI giúp xác định yêu cầu, tạo user stories, thiết kế schema database và lập kế hoạch kiến trúc.

### Giai đoạn frontend
AI tạo components, viết styles, triển khai quản lý state và xử lý tương tác phức tạp như kéo thả.

### Giai đoạn backend
AI thiết kế endpoints API, viết truy vấn database, triển khai xác thực và thiết lập kết nối WebSocket.

### Giai đoạn triển khai
AI tạo cấu hình Docker, thiết lập CI/CD pipeline và giúp giám sát, logging.

## Thời gian dự án

| Giai đoạn | Thời gian | Trọng tâm |
|-----------|----------|-----------|
| Giai đoạn 1: Lập kế hoạch | 2-3 ngày | Yêu cầu, kiến trúc, thiết lập |
| Giai đoạn 2: Frontend | 5-7 ngày | UI components, trang, state |
| Giai đoạn 3: Backend | 5-7 ngày | API, database, auth, thời gian thực |
| Giai đoạn 4: Triển khai | 2-3 ngày | Testing, triển khai, giám sát |

**Tổng: 2-3 tuần** (điều chỉnh theo tốc độ)

## Bạn sẽ học được gì

Hoàn thành capstone, bạn sẽ:

1. **Trải nghiệm toàn bộ vòng đời phát triển** — từ lập kế hoạch đến triển khai
2. **Thực hành phát triển với AI** — sử dụng AI hiệu quả ở mọi giai đoạn
3. **Xây dự án xứng đáng portfolio** — thứ bạn có thể tự hào khoe nhà tuyển dụng
4. **Phát triển kỹ năng chuyên nghiệp** — review code, testing, tài liệu hóa
5. **Hiểu vấn đề production** — bảo mật, hiệu suất, giám sát

## Bắt đầu

Trong bài tiếp theo, chúng ta sẽ đi sâu vào Giai đoạn 1: Lập kế hoạch. Bạn sẽ xác định yêu cầu, thiết kế kiến trúc và thiết lập môi trường phát triển. Đến cuối capstone, bạn sẽ có ứng dụng web hoàn chỉnh và sự tự tin để xây bất cứ thứ gì.

## Điểm mấu chốt

Capstone này là cơ hội kết hợp mọi thứ. AI không làm việc thay bạn — nó là đối tác pair programming, giúp bạn ra quyết định tốt hơn, viết code sạch hơn và ship nhanh hơn. Hãy xây điều tuyệt vời.
