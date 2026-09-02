# Tải tệp lên với AI

## Mục tiêu học tập
- Triển khai tải tệp lên với Multer và Express
- Tích hợp với lưu trữ đám mây (S3, Cloudinary)
- Xử lý xử lý hình ảnh và xác thực

## Tại sao tải tệp lên quan trọng?

Gần như mọi ứng dụng hiện đại đều cần tải tệp lên — ảnh hồ sơ, tệp đính kèm tài liệu, hình ảnh sản phẩm. Làm đúng tải tệp lên nghĩa là xử lý bảo mật, lưu trữ và hiệu suất.

## Tải lên cục bộ với Multer

Multer là middleware tiêu chuẩn để xử lý multipart/form-data:

```javascript
// middleware/upload.js
const multer = require('multer');
const path = require('path');

const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
    cb(null, uniqueSuffix + path.extname(file.originalname));
  }
});

const fileFilter = (req, file, cb) => {
  const allowedTypes = ['image/jpeg', 'image/png', 'image/webp', 'application/pdf'];
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new AppError('Loại tệp không hợp lệ', 400, 'INVALID_FILE_TYPE'), false);
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: {
    fileSize: 5 * 1024 * 1024 // 5MB
  }
});

module.exports = upload;
```

## Route với tải tệp lên

```javascript
// routes/users.js
const upload = require('../middleware/upload');

// Tải lên một tệp
router.post('/avatar', auth, upload.single('avatar'), async (req, res) => {
  const user = await User.findByIdAndUpdate(
    req.user.id,
    { avatar: req.file.path },
    { new: true }
  );
  res.json({ success: true, data: { avatar: user.avatar } });
});

// Tải lên nhiều tệp
router.post('/gallery', auth, upload.array('images', 10), async (req, res) => {
  const images = req.files.map(file => ({
    url: file.path,
    name: file.originalname,
    size: file.size
  }));
  res.json({ success: true, data: images });
});
```

## Tích hợp AWS S3

Cho môi trường production, lưu tệp trên S3:

```javascript
// services/s3Service.js
const { S3Client, PutObjectCommand, DeleteObjectCommand } = require('@aws-sdk/client-s3');
const sharp = require('sharp');

const s3 = new S3Client({
  region: process.env.AWS_REGION,
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY,
    secretAccessKey: process.env.AWS_SECRET_KEY
  }
});

const uploadToS3 = async (file, folder = 'uploads') => {
  // Thay đổi kích thước hình ảnh
  const buffer = await sharp(file.buffer)
    .resize(800, 800, { fit: 'inside', withoutEnlargement: true })
    .jpeg({ quality: 80 })
    .toBuffer();

  const key = `${folder}/${Date.now()}-${file.originalname}`;

  await s3.send(new PutObjectCommand({
    Bucket: process.env.S3_BUCKET,
    Key: key,
    Body: buffer,
    ContentType: file.mimetype
  }));

  return `https://${process.env.S3_BUCKET}.s3.${process.env.AWS_REGION}.amazonaws.com/${key}`;
};

const deleteFromS3 = async (key) => {
  await s3.send(new DeleteObjectCommand({
    Bucket: process.env.S3_BUCKET,
    Key: key
  }));
};
```

## Tích hợp Cloudinary

Cloudinary xử lý tải lên, chuyển đổi và phân phối CDN:

```javascript
// services/cloudinaryService.js
const cloudinary = require('cloudinary').v2;

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD,
  api_key: process.env.CLOUDINARY_KEY,
  api_secret: process.env.CLOUDINARY_SECRET
});

const uploadToCloudinary = async (file, folder = 'uploads') => {
  const result = await cloudinary.uploader.upload(file.path, {
    folder,
    transformation: [
      { width: 800, height: 800, crop: 'limit' },
      { quality: 'auto' },
      { format: 'webp' }
    ]
  });

  return {
    url: result.secure_url,
    publicId: result.public_id,
    width: result.width,
    height: result.height
  };
};

const deleteFromCloudinary = async (publicId) => {
  await cloudinary.uploader.destroy(publicId);
};
```

## Prompt AI cho hệ thống tải tệp lên

```
Tạo hệ thống tải tệp lên cho Express.js với:
1. Middleware Multer để xử lý tải lên multipart
2. Xác thực loại tệp (hình ảnh, PDF, tài liệu)
3. Giới hạn kích thước tệp (5MB cho hình ảnh, 10MB cho tài liệu)
4. Thay đổi kích thước và nén hình ảnh với Sharp
5. Tải lên AWS S3 với URL ký cho tải lên trực tiếp từ client
6. Tạo hình thu nhỏ cho hình ảnh
7. Theo dõi tiến trình cho tải lên lớn

Bao gồm xử lý lỗi cho tất cả trường hợp thất bại.
```

## URL ký cho tải lên trực tiếp

Cho phép client tải lên trực tiếp lên S3:

```javascript
// Tạo URL ký
const { getSignedUrl } = require('@aws-sdk/s3-request-presigner');
const { PutObjectCommand } = require('@aws-sdk/client-s3');

router.get('/upload-url', auth, async (req, res) => {
  const key = `uploads/${req.user.id}/${Date.now()}-${req.query.filename}`;

  const command = new PutObjectCommand({
    Bucket: process.env.S3_BUCKET,
    Key: key,
    ContentType: req.query.contentType
  });

  const url = await getSignedUrl(s3, command, { expiresIn: 300 });

  res.json({
    success: true,
    data: { uploadUrl: url, key }
  });
});
```

## Cân nhắc bảo mật

1. **Xác thực loại tệp** — Không tin tưởng MIME type từ client
2. **Giới hạn kích thước tệp** — Ngăn tấn công từ chối dịch vụ
3. **Quét phần mềm độc hại** — Sử dụng ClamAV hoặc tương tự cho production
4. **Dùng URL ký** — Không bao giờ暴露 lộ thông tin xác thực AWS cho client
5. **Đổi tên tệp** — Không sử dụng tên tệp gốc (ngăn tấn công path traversal)

## Bài tập thực hành

Xây dựng tính năng tải tệp lên cho trang portfolio:
- Tải lên ảnh hồ sơ với cắt xén
- Thư viện hình ảnh dự án (nhiều tệp)
- Tải lên sơ yếu lý lịch/tài liệu
- Tối ưu hóa hình ảnh và tạo hình thu nhỏ
- Lưu trữ S3 với URL ký

## Điểm chính

- Multer xử lý tải lên multipart trong Express.js
- Lưu trữ đám mây (S3, Cloudinary) rất cần thiết cho production
- Xử lý hình ảnh (thay đổi kích thước, nén) cải thiện hiệu suất
- URL ký cho phép tải lên trực tiếp an toàn từ client
