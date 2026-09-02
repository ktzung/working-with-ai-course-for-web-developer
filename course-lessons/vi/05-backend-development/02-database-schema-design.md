# Thiết kế Schema Database với AI

## Mục tiêu học tập
- Thiết kế schema database với sự hỗ trợ của AI
- Hiểu sự đánh đổi giữa SQL và NoSQL
- Tạo mối quan hệ, index và migration

## Chọn database

Quyết định đầu tiên: SQL hay NoSQL? AI có thể giúp bạn quyết định dựa trên mẫu dữ liệu.

**SQL (PostgreSQL, MySQL)** — Phù hợp nhất cho:
- Dữ liệu có cấu trúc với mối quan hệ rõ ràng
- Giao dịch phải tuân thủ ACID
- Truy vấn phức tạp với join
- Ví dụ: Thương mại điện tử, ngân hàng, hệ thống CRM

**NoSQL (MongoDB, Firebase)** — Phù hợp nhất cho:
- Schema linh hoạt, không ngừng phát triển
- Dữ liệu hướng tài liệu
- Tạo mẫu nhanh
- Ví dụ: Quản lý nội dung, ứng dụng thời gian thực, IoT

## Prompt AI để thiết kế Schema

```
Thiết kế schema database cho công cụ quản lý dự án với:
- Người dùng (tên, email, vai trò, ảnh đại diện)
- Dự án (tên, mô tả, chủ sở hữu, thành viên, trạng thái)
- Nhiệm vụ (tiêu đề, mô tả, người được giao, ưu tiên, ngày hết hạn, trạng thái)
- Bình luận (tác giả, nội dung, tệp đính kèm)
- Bảng thời gian (người dùng, nhiệm vụ, giờ, ngày)

Cho mỗi thực thể, chỉ định:
1. Trường với kiểu dữ liệu
2. Mối quan hệ (một-nhiều, nhiều-nhiều)
3. Index cho hiệu suất
4. Quy tắc xác thực

Sử dụng PostgreSQL với cú pháp Prisma ORM.
```

## Schema SQL với Prisma

```prisma
// schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  avatar    String?
  role      Role     @default(MEMBER)
  projects  ProjectMember[]
  tasks     Task[]    @relation("AssignedTasks")
  comments  Comment[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
}

enum Role {
  ADMIN
  MANAGER
  MEMBER
}

model Project {
  id          String   @id @default(cuid())
  name        String
  description String?
  status      ProjectStatus @default(ACTIVE)
  ownerId     String
  owner       User     @relation(fields: [ownerId], references: [id])
  members     ProjectMember[]
  tasks       Task[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([ownerId])
  @@index([status])
}

model Task {
  id          String   @id @default(cuid())
  title       String
  description String?
  priority    Priority @default(MEDIUM)
  status      TaskStatus @default(TODO)
  dueDate     DateTime?
  projectId   String
  project     Project  @relation(fields: [projectId], references: [id])
  assigneeId  String?
  assignee    User?    @relation("AssignedTasks", fields: [assigneeId], references: [id])
  comments    Comment[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([projectId])
  @@index([assigneeId])
  @@index([status])
}
```

## Schema MongoDB thay thế

Với Mongoose:

```javascript
// models/Project.js
const projectSchema = new mongoose.Schema({
  name: { type: String, required: true, trim: true },
  description: String,
  status: {
    type: String,
    enum: ['ACTIVE', 'ARCHIVED', 'COMPLETED'],
    default: 'ACTIVE'
  },
  owner: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  members: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    role: { type: String, enum: ['ADMIN', 'MEMBER', 'VIEWER'] },
    joinedAt: { type: Date, default: Date.now }
  }],
  tags: [String]
}, { timestamps: true });

projectSchema.index({ owner: 1, status: 1 });
projectSchema.index({ 'members.user': 1 });
```

## Pattern mối quan hệ

**Một-Nhiều**: Người dùng có nhiều nhiệm vụ
```prisma
model User {
  tasks Task[]
}
model Task {
  userId String
  user   User   @relation(fields: [userId], references: [id])
}
```

**Nhiều-Nhiều**: Người dùng thuộc nhiều dự án
```prisma
model ProjectMember {
  userId    String
  projectId String
  role      Role   @default(MEMBER)
  user      User    @relation(fields: [userId], references: [id])
  project   Project @relation(fields: [projectId], references: [id])
  @@id([userId, projectId])
}
```

## Chiến lược Index

AI có thể đề xuất index dựa trên mẫu truy vấn:

```
Cho các truy vấn phổ biến sau:
1. Tìm nhiệm vụ theo dự án và trạng thái
2. Tìm nhiệm vụ được giao cho người dùng
3. Tìm kiếm dự án theo tên
4. Lấy bình luận gần đây cho nhiệm vụ

Đề xuất index cho hiệu suất tối ưu.
```

AI sẽ đề xuất index tổng hợp như:
```prisma
@@index([projectId, status])  // Truy vấn 1
@@index([assigneeId, status]) // Truy vấn 2
```

## Migration

Khi schema phát triển, migration theo dõi thay đổi:

```bash
# Tạo migration
npx prisma migrate dev --name add-task-priority

# Áp dụng migration
npx prisma migrate deploy
```

## Bài tập thực hành

Thiết kế schema database cho nền tảng mạng xã hội:
- Người dùng với hồ sơ và mối quan hệ theo dõi
- Bài viết với hình ảnh, lượt thích và bình luận
- Tin nhắn trực tiếp giữa người dùng
- Hệ thống thông báo

Sử dụng AI để tạo cả phiên bản Prisma và Mongoose, sau đó so sánh các cách tiếp cận.

## Điểm chính

- SQL phù hợp với dữ liệu quan hệ có cấu trúc; NoSQL phù hợp với tài liệu linh hoạt
- AI tạo schema hoàn chỉnh từ yêu cầu ngôn ngữ tự nhiên
- Index rất quan trọng cho hiệu suất truy vấn — thiết kế chúng xoay quanh mẫu truy cập
- Migration cho phép bạn phát triển schema an toàn theo thời gian
