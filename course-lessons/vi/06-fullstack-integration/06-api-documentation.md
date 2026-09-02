# Tài liệu API với AI

## Mục tiêu học tập
- Tạo tài liệu OpenAPI/Swagger
- Tạo tài liệu API tương tác
- Sử dụng AI để tài liệu hóa API hiện có

## Tại sao tài liệu quan trọng?

API không được tài liệu hóa là API không thể sử dụng. Tài liệu tốt:
- Giảm thời gian onboard cho nhà phát triển mới
- Ngăn lỗi tích hợp
- Cho phép tạo SDK client tự động
- Phục vụ như hợp đồng giữa frontend và backend

## Đặc tả OpenAPI

OpenAPI (trước đây là Swagger) là tiêu chuẩn cho tài liệu API:

```yaml
# openapi.yaml
openapi: 3.0.0
info:
  title: API quản lý nhiệm vụ
  version: 1.0.0
  description: API để quản lý nhiệm vụ, dự án và nhóm

servers:
  - url: http://localhost:5000/api
    description: Phát triển
  - url: https://api.example.com
    description: Production

paths:
  /tasks:
    get:
      summary: Lấy tất cả nhiệm vụ
      tags: [Nhiệm vụ]
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: status
          in: query
          schema:
            type: string
            enum: [TODO, IN_PROGRESS, REVIEW, DONE]
      responses:
        200:
          description: Danh sách nhiệm vụ
          content:
            application/json:
              schema:
                type: object
                properties:
                  success:
                    type: boolean
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Task'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

    post:
      summary: Tạo nhiệm vụ
      tags: [Nhiệm vụ]
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskInput'
      responses:
        201:
          description: Nhiệm vụ đã tạo
        400:
          description: Lỗi xác thực
        401:
          description: Chưa xác thực

components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
        title:
          type: string
        status:
          type: string
          enum: [TODO, IN_PROGRESS, REVIEW, DONE]
        priority:
          type: string
          enum: [LOW, MEDIUM, HIGH, URGENT]

    TaskInput:
      type: object
      required: [title, project]
      properties:
        title:
          type: string
          minLength: 2
          maxLength: 100
        description:
          type: string
        priority:
          type: string
          default: MEDIUM

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

## Swagger UI với Express

```javascript
// Cài đặt: npm install swagger-ui-express yamljs

const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const swaggerDocument = YAML.load('./openapi.yaml');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument, {
  customCss: '.swagger-ui .topbar { display: none }',
  customSiteTitle: 'Tài liệu Task API'
}));
```

## Tự động tạo từ JSDoc

```javascript
// Cài đặt: npm install swagger-jsdoc

const swaggerJsdoc = require('swagger-jsdoc');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'API quản lý nhiệm vụ',
      version: '1.0.0'
    }
  },
  apis: ['./routes/*.js'] // Quét tệp route cho bình luận JSDoc
};

const swaggerSpec = swaggerJsdoc(options);

/**
 * @swagger
 * /api/tasks:
 *   get:
 *     summary: Lấy tất cả nhiệm vụ
 *     tags: [Nhiệm vụ]
 *     parameters:
 *       - in: query
 *         name: status
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: Thành công
 */
router.get('/tasks', taskController.getTasks);
```

## Prompt AI cho tài liệu

```
Tạo tài liệu OpenAPI 3.0 cho API Express.js này:
1. Quét tất cả tệp route và trích xuất endpoint
2. Tạo schema request/response từ model Mongoose
3. Tài liệu hóa yêu cầu xác thực
4. Thêm ví dụ request và response
5. Bao gồm schema phản hồi lỗi
6. Tạo bộ sưu tập Postman như giải pháp thay thế

Xuất cả định dạng YAML và JSON.
```

## Bộ sưu tập Postman

```javascript
// Tạo bộ sưu tập Postman từ OpenAPI
const postman = require('openapi-to-postmanv2');

postman.convert({ type: 'string', data: yamlContent }, {}, (err, result) => {
  if (!err) {
    fs.writeFileSync('collection.json', JSON.stringify(result.output[0].data));
  }
});
```

## Thực hành tốt nhất về tài liệu

1. **Giữ cập nhật**: Tài liệu cũ tệ hơn không có tài liệu
2. **Bao gồm ví dụ**: Show ví dụ request/response thực tế
3. **Tài liệu lỗi**: Liệt kê tất cả mã lỗi có thể
4. **Phiên bản tài liệu**: Khớp phiên bản API
5. **Làm cho tương tác**: Swagger UI cho phép người dùng kiểm tra endpoint

## Bài tập thực hành

Tài liệu hóa API quản lý nhiệm vụ:
- Tạo đặc tả OpenAPI cho tất cả endpoint
- Thiết lập Swagger UI tại `/api-docs`
- Thêm bình luận JSDoc vào tất cả route
- Tạo bộ sưu tập Postman
- Bao gồm tài liệu xác thực

## Điểm chính

- OpenAPI là tiêu chuẩn cho tài liệu API
- Swagger UI cung cấp kiểm thử API tương tác
- AI có thể tạo tài liệu từ mã hiện có
- Tài liệu tốt rất cần thiết cho việc采纳 API
