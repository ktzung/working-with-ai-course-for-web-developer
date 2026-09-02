# Thực hành: Xây dựng REST API hoàn chỉnh

## Mục tiêu học tập
- Áp dụng tất cả khái niệm backend trong dự án thực tế
- Xây dựng API sẵn sàng cho production từ đầu
- Thực hành quy trình phát triển với sự hỗ trợ của AI

## Dự án: API quản lý nhiệm vụ

Xây dựng REST API hoàn chỉnh cho ứng dụng quản lý nhiệm vụ. Dự án này kết hợp mọi thứ từ Phần 5.

## Yêu cầu

**Người dùng**:
- Đăng ký với email/mật khẩu
- Đăng nhập với JWT
- Quản lý hồ sơ (cập nhật, tải lên ảnh đại diện)
- Kiểm soát truy cập dựa trên vai trò (quản trị, quản lý, thành viên)

**Dự án**:
- Tạo, đọc, cập nhật, xóa dự án
- Thêm/xóa thành viên
- Cài đặt và trạng thái dự án

**Nhiệm vụ**:
- Thao tác CRUD với các trường phong phú
- Giao cho thành viên nhóm
- Mức độ ưu tiên, ngày hết hạn, theo dõi trạng thái
- Bình luận và tệp đính kèm

**Tính năng nâng cao**:
- Tìm kiếm và lọc
- Phân trang
- Tải tệp lên
- Thông báo qua email

## Bước 1: Thiết lập dự án

```bash
mkdir task-api && cd task-api
npm init -y
npm install express mongoose dotenv bcryptjs jsonwebtoken
npm install multer cloudinary zod cors helmet morgan
npm install -D nodemon jest supertest
```

## Bước 2: Sử dụng AI để tạo nền tảng

```
Tạo cấu trúc dự án Express.js hoàn chỉnh cho API quản lý nhiệm vụ với:
1. Kiến trúc MVC (models, controllers, routes, middleware)
2. Kết nối MongoDB với Mongoose
3. Cấu hình môi trường với dotenv
4. Middleware xử lý lỗi
5. Middleware xác thực với JWT
6. Xác thực đầu vào với Zod
7. CORS và tiêu đề bảo mật

Tạo tất cả các tệp mẫu bao gồm script package.json.
```

## Bước 3: Triển khai Models

```javascript
// models/Task.js
const taskSchema = new mongoose.Schema({
  title: { type: String, required: true, trim: true },
  description: { type: String },
  status: {
    type: String,
    enum: ['TODO', 'IN_PROGRESS', 'REVIEW', 'DONE'],
    default: 'TODO'
  },
  priority: {
    type: String,
    enum: ['LOW', 'MEDIUM', 'HIGH', 'URGENT'],
    default: 'MEDIUM'
  },
  dueDate: { type: Date },
  project: { type: mongoose.Schema.Types.ObjectId, ref: 'Project', required: true },
  assignee: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  creator: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  tags: [String],
  attachments: [{
    url: String,
    name: String,
    size: Number,
    uploadedAt: { type: Date, default: Date.now }
  }],
  comments: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    content: { type: String, required: true },
    createdAt: { type: Date, default: Date.now }
  }]
}, { timestamps: true });
```

## Bước 4: Xây dựng Controllers

Sử dụng AI để tạo mỗi controller:

```
Tạo controller nhiệm vụ với Express.js và Mongoose xử lý:
1. createTask - xác thực đầu vào, kiểm tra tư cách thành viên dự án, tạo nhiệm vụ
2. getTasks - lọc theo dự án, trạng thái, người được giao; sắp xếp và phân trang
3. getTask - tìm theo ID với tham chiếu đã populate
4. updateTask - cập nhật một phần với xác thực
5. deleteTask - xóa mềm với kiểm tra quyền
6. addComment - thêm bình luận vào nhiệm vụ
7. updateStatus - thay đổi trạng thái nhiệm vụ với xác thực

Bao gồm xử lý lỗi đúng cách và kiểm tra授权.
```

## Bước 5: Thêm tải tệp lên

```javascript
// routes/tasks.js
const upload = require('../middleware/upload');

router.post('/:id/attachments',
  auth,
  upload.array('files', 5),
  taskController.addAttachments
);

router.delete('/:id/attachments/:attachmentId',
  auth,
  taskController.removeAttachment
);
```

## Bước 6: Kiểm thử

```javascript
// tests/tasks.test.js
const request = require('supertest');
const app = require('../app');

describe('Tasks API', () => {
  let token, projectId;

  beforeAll(async () => {
    // Thiết lập người dùng và dự án kiểm thử
    const res = await request(app)
      .post('/api/auth/register')
      .send({ email: 'test@test.com', password: 'password123', name: 'Test' });
    token = res.body.data.token;
  });

  test('POST /api/tasks - tạo nhiệm vụ', async () => {
    const res = await request(app)
      .post('/api/tasks')
      .set('Authorization', `Bearer ${token}`)
      .send({
        title: 'Nhiệm vụ kiểm thử',
        project: projectId,
        priority: 'HIGH'
      });

    expect(res.status).toBe(201);
    expect(res.body.data.title).toBe('Nhiệm vụ kiểm thử');
  });
});
```

## Bước 7: Tài liệu API

Sử dụng AI để tạo tài liệu Swagger/OpenAPI:

```
Tạo tài liệu OpenAPI 3.0 cho API quản lý nhiệm vụ này bao gồm:
- Tất cả endpoint với schema request/response
- Yêu cầu xác thực
- Schema phản hồi lỗi
- Ví dụ request và response
```

## Sản phẩm bàn giao

Kết thúc thực hành này, bạn sẽ có:
1. ✅ API Express.js hoàn chỉnh với kiến trúc MVC
2. ✅ Model MongoDB với mối quan hệ
3. ✅ Xác thực JWT với kiểm soát truy cập dựa trên vai trò
4. ✅ Thao tác CRUD cho tất cả tài nguyên
5. ✅ Tải tệp lên với lưu trữ đám mây
6. ✅ Xác thực đầu vào và xử lý lỗi
7. ✅ Tài liệu API
8. ✅ Bộ kiểm thử cơ bản

## Điểm chính

- Xây dựng API hoàn chỉnh đòi hỏi kết hợp nhiều khái niệm
- AI tăng tốc phát triển bằng cách tạo mã mẫu
- Kiểm thử đảm bảo API hoạt động đúng
- Tài liệu làm cho API có thể sử dụng được bởi người khác
