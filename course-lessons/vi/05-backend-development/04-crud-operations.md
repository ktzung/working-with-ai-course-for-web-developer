# Thao tác CRUD với AI

## Mục tiêu học tập
- Triển khai thao tác CRUD với Express.js và FastAPI
- Sử dụng AI để tạo mã CRUD mẫu
- Xử lý xác thực, phân trang và phản hồi lỗi

## CRUD là gì?

CRUD là viết tắt của bốn thao tác cơ sở dữ liệu cơ bản:
- **Create** — Thêm bản ghi mới (POST)
- **Read** — Lấy bản ghi (GET)
- **Update** — Chỉnh sửa bản ghi hiện có (PUT/PATCH)
- **Delete** — Xóa bản ghi (DELETE)

Mọi ứng dụng web đều围绕 bốn thao tác này. Nắm vững chúng, bạn có thể xây dựng bất cứ thứ gì.

## CRUD Express.js với MongoDB

```javascript
// controllers/productController.js
const Product = require('../models/Product');

// CREATE
exports.createProduct = async (req, res) => {
  try {
    const product = await Product.create(req.body);
    res.status(201).json({ success: true, data: product });
  } catch (error) {
    if (error.name === 'ValidationError') {
      const messages = Object.values(error.errors).map(e => e.message);
      return res.status(400).json({ success: false, errors: messages });
    }
    res.status(500).json({ success: false, error: 'Lỗi server' });
  }
};

// READ ALL (với phân trang, lọc, sắp xếp)
exports.getProducts = async (req, res) => {
  try {
    const { page = 1, limit = 10, sort = '-createdAt', category, minPrice, maxPrice } = req.query;

    const filter = {};
    if (category) filter.category = category;
    if (minPrice || maxPrice) {
      filter.price = {};
      if (minPrice) filter.price.$gte = Number(minPrice);
      if (maxPrice) filter.price.$lte = Number(maxPrice);
    }

    const products = await Product.find(filter)
      .sort(sort)
      .limit(Number(limit))
      .skip((Number(page) - 1) * Number(limit));

    const total = await Product.countDocuments(filter);

    res.json({
      success: true,
      data: products,
      pagination: {
        page: Number(page),
        limit: Number(limit),
        total,
        pages: Math.ceil(total / Number(limit))
      }
    });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Lỗi server' });
  }
};

// READ ONE
exports.getProduct = async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    if (!product) {
      return res.status(404).json({ success: false, error: 'Không tìm thấy sản phẩm' });
    }
    res.json({ success: true, data: product });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Lỗi server' });
  }
};

// UPDATE
exports.updateProduct = async (req, res) => {
  try {
    const product = await Product.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true, runValidators: true }
    );
    if (!product) {
      return res.status(404).json({ success: false, error: 'Không tìm thấy sản phẩm' });
    }
    res.json({ success: true, data: product });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Lỗi server' });
  }
};

// DELETE
exports.deleteProduct = async (req, res) => {
  try {
    const product = await Product.findByIdAndDelete(req.params.id);
    if (!product) {
      return res.status(404).json({ success: false, error: 'Không tìm thấy sản phẩm' });
    }
    res.json({ success: true, data: {} });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Lỗi server' });
  }
};
```

## CRUD FastAPI với Python

```python
# routers/products.py
from fastapi import APIRouter, HTTPException, Query
from models import Product, ProductCreate, ProductUpdate
from database import products_collection
from bson import ObjectId

router = APIRouter(prefix="/api/products", tags=["products"])

@router.post("/", status_code=201)
async def create_product(product: ProductCreate):
    result = await products_collection.insert_one(product.dict())
    created = await products_collection.find_one({"_id": result.inserted_id})
    return {"success": True, "data": serialize_product(created)}

@router.get("/")
async def get_products(
    page: int = Query(1, ge=1),
    limit: int = Query(10, ge=1, le=100),
    category: str = None,
    sort: str = "-created_at"
):
    filter_query = {}
    if category:
        filter_query["category"] = category

    skip = (page - 1) * limit
    cursor = products_collection.find(filter_query).skip(skip).limit(limit)
    products = [serialize_product(p) async for p in cursor]
    total = await products_collection.count_documents(filter_query)

    return {
        "success": True,
        "data": products,
        "pagination": {"page": page, "limit": limit, "total": total}
    }
```

## Prompt AI để tạo CRUD

```
Tạo API CRUD hoàn chỉnh cho [tài nguyên] với Express.js và MongoDB:
- Xác thực đầu vào bằng Joi
- Phân trang với tham số page, limit, sort
- Lọc theo [các trường]
- Populate tài liệu liên quan
- Xử lý lỗi đúng cách với mã trạng thái
- Định dạng phản hồi nhất quán: { success, data, error, pagination }

Bao gồm model, controller, routes và middleware xác thực.
```

## Xác thực với Joi

```javascript
// validators/productValidator.js
const Joi = require('joi');

const productSchema = Joi.object({
  name: Joi.string().min(2).max(100).required(),
  description: Joi.string().max(1000),
  price: Joi.number().positive().required(),
  category: Joi.string().valid('electronics', 'clothing', 'books').required(),
  stock: Joi.number().integer().min(0).default(0),
  tags: Joi.array().items(Joi.string())
});

const validateProduct = (req, res, next) => {
  const { error } = productSchema.validate(req.body, { abortEarly: false });
  if (error) {
    const messages = error.details.map(d => d.message);
    return res.status(400).json({ success: false, errors: messages });
  }
  next();
};
```

## Bài tập thực hành

Xây dựng API CRUD hoàn chỉnh cho nền tảng chia sẻ công thức nấu ăn:
- Công thức với nguyên liệu, các bước và hình ảnh
- Người dùng có thể đánh giá và nhận xét công thức
- Tìm kiếm theo nguyên liệu hoặc loại ẩm thực
- Phân trang và lọc

Triển khai với cả Express.js và FastAPI để so sánh cách tiếp cận.

## Điểm chính

- CRUD là nền tảng của mọi ứng dụng web
- Định dạng phản hồi nhất quán giúp tích hợp frontend dễ dàng hơn
- Xác thực ngăn dữ liệu xấu进入 cơ sở dữ liệu
- AI tạo mẫu CRUD hoàn chỉnh từ mô tả tài nguyên
