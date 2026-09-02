# File Uploads with AI

## Learning Objectives
- Implement file uploads with Multer and Express
- Integrate with cloud storage (S3, Cloudinary)
- Handle image processing and validation

## Why File Uploads Matter

Almost every modern app needs file uploads — profile pictures, document attachments, product images. Getting uploads right means handling security, storage, and performance.

## Local Uploads with Multer

Multer is the standard middleware for handling multipart/form-data:

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
    cb(new AppError('Invalid file type', 400, 'INVALID_FILE_TYPE'), false);
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

## Route with File Upload

```javascript
// routes/users.js
const upload = require('../middleware/upload');

// Single file upload
router.post('/avatar', auth, upload.single('avatar'), async (req, res) => {
  const user = await User.findByIdAndUpdate(
    req.user.id,
    { avatar: req.file.path },
    { new: true }
  );
  res.json({ success: true, data: { avatar: user.avatar } });
});

// Multiple files upload
router.post('/gallery', auth, upload.array('images', 10), async (req, res) => {
  const images = req.files.map(file => ({
    url: file.path,
    name: file.originalname,
    size: file.size
  }));
  res.json({ success: true, data: images });
});
```

## AWS S3 Integration

For production, store files in S3:

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
  // Resize image
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

## Cloudinary Integration

Cloudinary handles uploads, transformations, and CDN delivery:

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

## AI Prompt for File Upload System

```
Create a file upload system for Express.js with:
1. Multer middleware for handling multipart uploads
2. File type validation (images, PDFs, documents)
3. File size limits (5MB for images, 10MB for documents)
4. Image resizing and compression with Sharp
5. AWS S3 upload with signed URLs for direct client uploads
6. Thumbnail generation for images
7. Progress tracking for large uploads

Include error handling for all failure cases.
```

## Signed URLs for Direct Upload

Let clients upload directly to S3:

```javascript
// Generate signed URL
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

## Security Considerations

1. **Validate file types** — Don't trust the client's MIME type
2. **Limit file sizes** — Prevent denial-of-service attacks
3. **Scan for malware** — Use ClamAV or similar for production
4. **Use signed URLs** — Never expose AWS credentials to clients
5. **Rename files** — Don't use original filenames (prevent path traversal)

## Practice Exercise

Build a file upload feature for a portfolio site:
- Profile picture upload with cropping
- Project image gallery (multiple files)
- Resume/document upload
- Image optimization and thumbnail generation
- S3 storage with signed URLs

## Key Takeaways

- Multer handles multipart uploads in Express.js
- Cloud storage (S3, Cloudinary) is essential for production
- Image processing (resize, compress) improves performance
- Signed URLs enable secure direct client uploads
