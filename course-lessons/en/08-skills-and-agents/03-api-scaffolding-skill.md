# Create Skill: API Scaffolding

## Learning Objectives
- Build a skill that generates Express.js API endpoints
- Include routes, controllers, models, and validation
- Customize for your project's API patterns

## Why API Scaffolding?

Every new resource in your API needs:
- Route definitions
- Controller with CRUD operations
- Model/Schema definition
- Validation middleware
- Error handling
- Tests

An API scaffolding skill generates all of this from a simple description.

## The Skill Definition

Create a file at `.github/copilot/skills/api-scaffolding.md`:

```markdown
# API Scaffolding Skill

## Description
Generate complete Express.js API endpoints with routes, controllers, models, and validation.

## Trigger
When user asks to create a new API endpoint, add a resource, or scaffold an API.

## Instructions

### Step 1: Gather Requirements
Ask the user for:
- Resource name (singular, lowercase): e.g., "product"
- Fields with types: e.g., "name: string, price: number, description: string"
- Relationships: e.g., "belongs to User, has many Reviews"
- Authentication requirements: public, authenticated, admin-only
- Special operations beyond CRUD: search, filter, export

### Step 2: Generate Model
Create `models/ResourceName.js`:

```javascript
const mongoose = require('mongoose');

const resourceSchema = new mongoose.Schema({
  // Fields from requirements
  name: { type: String, required: true, trim: true },
  // ... other fields

  // Timestamps
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
});

// Indexes
resourceSchema.index({ name: 1 });

// Pre-save hook
resourceSchema.pre('save', function(next) {
  this.updatedAt = Date.now();
  next();
});

module.exports = mongoose.model('ResourceName', resourceSchema);
```

### Step 3: Generate Controller
Create `controllers/resourceController.js` with:
- `getAll` - List with pagination, filtering, sorting
- `getById` - Single resource with populated relations
- `create` - Create with validation
- `update` - Update with authorization check
- `delete` - Soft delete with authorization check

### Step 4: Generate Routes
Create `routes/resource.js`:

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

### Step 5: Generate Validation
Create `validators/resourceValidator.js` with Zod schemas for create and update.

### Step 6: Generate Tests
Create `tests/resource.test.js` with tests for all CRUD operations.

## Output Files
- `models/ResourceName.js`
- `controllers/resourceController.js`
- `routes/resource.js`
- `validators/resourceValidator.js`
- `tests/resource.test.js`

## Constraints
- Use async/await for all database operations
- Include proper error handling with custom error classes
- Follow RESTful naming conventions
- Add pagination for list endpoints (default: page=1, limit=10)
- Include population of related documents
```

## Using the Skill

When you say "Create a products API with name, price, description, and category", the skill generates:

1. **Product Model** with Mongoose schema
2. **Product Controller** with all CRUD operations
3. **Product Routes** with middleware
4. **Product Validator** with Zod schemas
5. **Product Tests** with Supertest

## Customizing for Your Project

### Add Authentication Patterns

```markdown
## Authentication Patterns
- Public routes: GET (list, getById)
- Authenticated routes: POST, PUT (own resources)
- Admin routes: DELETE, PUT (any resource)
```

### Add Response Format

```markdown
## Response Format
All responses follow this structure:
{
  "success": boolean,
  "data": T | T[],
  "error": { "code": string, "message": string },
  "pagination": { "page": number, "limit": number, "total": number }
}
```

### Add Database Patterns

```markdown
## Database Patterns
- Use lean() for read operations
- Populate related documents by default
- Soft delete (set deletedAt) instead of hard delete
- Add created/updated timestamps automatically
```

## Advanced: Nested Resources

```markdown
## Nested Resources
For resources with parent-child relationships:

### Routes
GET /api/projects/:projectId/tasks
POST /api/projects/:projectId/tasks
GET /api/projects/:projectId/tasks/:taskId

### Controller
- Filter by parent ID
- Validate parent exists
- Check parent ownership
```

## AI Prompt for API Skill

```
Create an API scaffolding skill for Express.js that:
1. Generates model, controller, routes, validator, and tests
2. Follows RESTful conventions
3. Includes authentication and authorization
4. Supports pagination, filtering, and sorting
5. Uses Zod for validation
6. Follows the project's response format

Output the skill as a markdown file ready to use.
```

## Practice Exercise

Create an API scaffolding skill for your project:
1. Define the skill with clear instructions
2. Include templates for all generated files
3. Add authentication and authorization patterns
4. Test by scaffolding a "comments" resource
5. Refine based on the generated code quality

## Key Takeaways

- API scaffolding skills generate complete CRUD endpoints
- Include model, controller, routes, validator, and tests
- Customize for your project's patterns and conventions
- Saves hours of boilerplate code for each new resource
