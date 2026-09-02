# Tạo kỹ năng: Dự dựng API

## Mục tiêu học tập
- Xây dựng kỹ năng tạo endpoint API Express.js
- Bao gồm route, controller, model và xác thực
- Tùy chỉnh cho mẫu API dự án của bạn

## Tại sao dự dựng API?

Mọi tài nguyên mới trong API của bạn cần:
- Định nghĩa route
- Controller với thao tác CRUD
- Định nghĩa Model/Schema
- Middleware xác thực
- Xử lý lỗi
- Kiểm thử

Kỹ năng dự dựng API tạo tất cả những thứ này từ mô tả đơn giản.

## Định nghĩa kỹ năng

Tạo tệp tại `.github/copilot/skills/api-scaffolding.md`:

```markdown
# Kỹ năng dự dựng API

## Mô tả
Tạo endpoint API Express.js hoàn chỉnh với route, controller, model và xác thực.

## Trigger
Khi người dùng yêu cầu tạo endpoint API mới, thêm tài nguyên hoặc dự dựng API.

## Hướng dẫn

### Bước 1: Thu thập yêu cầu
Hỏi người dùng:
- Tên tài nguyên (số ít, chữ thường): ví dụ "product"
- Trường với kiểu: ví dụ "name: string, price: number, description: string"
- Mối quan hệ: ví dụ "thuộc về User, có nhiều Review"
- Yêu cầu xác thực: công khai, đã xác thực, chỉ quản trị
- Thao tác đặc biệt ngoài CRUD: tìm kiếm, lọc, xuất

### Bước 2: Tạo Model
Tạo `models/ResourceName.js`:

```javascript
const mongoose = require('mongoose');

const resourceSchema = new mongoose.Schema({
  // Trường từ yêu cầu
  name: { type: String, required: true, trim: true },
  // ... các trường khác

  // Thời gian
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
});

// Index
resourceSchema.index({ name: 1 });

// Hook trước khi lưu
resourceSchema.pre('save', function(next) {
  this.updatedAt = Date.now();
  next();
});

module.exports = mongoose.model('ResourceName', resourceSchema);
```

### Bước 3: Tạo Controller
Tạo `controllers/resourceController.js` với:
- `getAll` - Danh sách với phân trang, lọc, sắp xếp
- `getById` - Tài nguyên đơn với quan hệ đã populate
- `create` - Tạo với xác thực
- `update` - Cập nhật với kiểm tra授权
- `delete` - Xóa mềm với kiểm tra授权

### Bước 4: Tạo Routes
Tạo `routes/resource.js`:

```javascript
const express = require('express');
const router = express.Router();
const controller = require('../controllers/resourceController');
const auth = require('../middleware/auth');
const validate = require('../middleware/validate');

router.get('/', controller.getAll);
router.get('/:id', controller.getById);
router.post('/', auth, validate('create'), controller.create);
router.put('/:id', auth, validate('update'), controller.update);
router.delete('/:id', auth, controller.delete);

module.exports = router;
```

### Bước 5: Tạo Validation
Tạo `validators/resourceValidator.js` với schema Zod cho create và update.

### Bước 6: Tạo Tests
Tạo `tests/resource.test.js` với kiểm thử cho tất cả thao tác CRUD.

## Tệp đầu ra
- `models/ResourceName.js`
- `controllers/resourceController.js`
- `routes/resource.js`
- `validators/resourceValidator.js`
- `tests/resource.test.js`

## Ràng buộc
- Sử dụng async/await cho tất cả thao tác cơ sở dữ liệu
- Bao gồm xử lý lỗi đúng cách với lớp lỗi tùy chỉnh
- Theo dõi quy ước đặt tên RESTful
- Thêm phân trang cho endpoint danh sách (mặc định: page=1, limit=10)
- Bao gồm populate tài liệu liên quan
```

## Sử dụng kỹ năng

Khi bạn nói "Tạo API sản phẩm với name, price, description và category", kỹ năng tạo:

1. **Model Product** với schema Mongoose
2. **Controller Product** với tất cả thao tác CRUD
3. **Routes Product** với middleware
4. **Validator Product** với schema Zod
5. **Tests Product** với Supertest

## Tùy chỉnh cho dự án của bạn

### Thêm mẫu xác thực

```markdown
## Mẫu xác thực
- Route công khai: GET (danh sách, getById)
- Route đã xác thực: POST, PUT (tài nguyên của mình)
- Route quản trị: DELETE, PUT (bất kỳ tài nguyên nào)
```

### Thêm định dạng phản hồi

```markdown
## Định dạng phản hồi
Tất cả phản hồi theo cấu trúc này:
{
  "success": boolean,
  "data": T | T[],
  "error": { "code": string, "message": string },
  "pagination": { "page": number, "limit": number, "total": number }
}
```

### Thêm mẫu cơ sở dữ liệu

```markdown
## Mẫu cơ sở dữ liệu
- Sử dụng lean() cho thao tác đọc
- Populate tài liệu liên quan theo mặc định
- Xóa mềm (đặt deletedAt) thay vì xóa cứng
- Thêm thời gian tạo/cập nhật tự động
```

## Nâng cao: Tài nguyên lồng nhau

```markdown
## Tài nguyên lồng nhau
Với tài nguyên có quan hệ cha-con:

### Routes
GET /api/projects/:projectId/tasks
POST /api/projects/:projectId/tasks
GET /api/projects/:projectId/tasks/:taskId

### Controller
- Lọc theo ID cha
- Xác thực cha tồn tại
- Kiểm tra quyền sở hữu cha
```

## Prompt AI cho kỹ năng API

```
Tạo kỹ năng dự dựng API cho Express.js:
1. Tạo model, controller, routes, validator và kiểm thử
2. Theo dõi quy ước RESTful
3. Bao gồm xác thực và授权
4. Hỗ trợ phân trang, lọc và sắp xếp
5. Sử dụng Zod cho xác thực
6. Theo dõi định dạng phản hồi dự án

Xuất kỹ năng dưới dạng tệp markdown sẵn sàng sử dụng.
```

## Bài tập thực hành

Tạo kỹ năng dự dựng API cho dự án của bạn:
1. Định nghĩa kỹ năng với hướng dẫn rõ ràng
2. Bao gồm mẫu cho tất cả tệp được tạo
3. Thêm mẫu xác thực và授权
4. Kiểm thử bằng cách dự dựng tài nguyên "comments"
5. Tinh chỉnh dựa trên chất lượng mã được tạo

## Điểm chính

- Kỹ năng dự dựng API tạo endpoint CRUD hoàn chỉnh
- Bao gồm model, controller, routes, validator và kiểm thử
- Tùy chỉnh cho mẫu và quy ước dự án của bạn
- Tiết kiệm hàng giờ mã mẫu cho mỗi tài nguyên mới
