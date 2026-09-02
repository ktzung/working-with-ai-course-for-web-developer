# CRUD Operations with AI

## Learning Objectives
- Implement CRUD operations with Express.js and FastAPI
- Use AI to generate boilerplate CRUD code
- Handle validation, pagination, and error responses

## What is CRUD?

CRUD stands for the four basic database operations:
- **Create** — Add new records (POST)
- **Read** — Retrieve records (GET)
- **Update** — Modify existing records (PUT/PATCH)
- **Delete** — Remove records (DELETE)

Every web application revolves around these operations. Master them, and you can build anything.

## Express.js CRUD with MongoDB

Here's a complete CRUD controller for a products resource:

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
    res.status(500).json({ success: false, error: 'Server error' });
  }
};

// READ ALL (with pagination, filtering, sorting)
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
    res.status(500).json({ success: false, error: 'Server error' });
  }
};

// READ ONE
exports.getProduct = async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    if (!product) {
      return res.status(404).json({ success: false, error: 'Product not found' });
    }
    res.json({ success: true, data: product });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Server error' });
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
      return res.status(404).json({ success: false, error: 'Product not found' });
    }
    res.json({ success: true, data: product });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Server error' });
  }
};

// DELETE
exports.deleteProduct = async (req, res) => {
  try {
    const product = await Product.findByIdAndDelete(req.params.id);
    if (!product) {
      return res.status(404).json({ success: false, error: 'Product not found' });
    }
    res.json({ success: true, data: {} });
  } catch (error) {
    res.status(500).json({ success: false, error: 'Server error' });
  }
};
```

## FastAPI CRUD with Python

The same CRUD pattern in FastAPI:

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

## AI Prompt for CRUD Generation

```
Generate a complete CRUD API for a [resource] with Express.js and MongoDB:
- Input validation using Joi
- Pagination with page, limit, sort parameters
- Filtering by [fields]
- Population of related documents
- Proper error handling with status codes
- Consistent response format: { success, data, error, pagination }

Include the model, controller, routes, and validation middleware.
```

## Validation with Joi

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

## Practice Exercise

Build a complete CRUD API for a recipe sharing platform:
- Recipes with ingredients, steps, and images
- Users can rate and review recipes
- Search by ingredients or cuisine type
- Pagination and filtering

Implement with both Express.js and FastAPI to compare approaches.

## Key Takeaways

- CRUD is the foundation of every web application
- Consistent response format improves frontend integration
- Validation prevents bad data from entering your database
- AI generates complete CRUD boilerplate from resource descriptions
