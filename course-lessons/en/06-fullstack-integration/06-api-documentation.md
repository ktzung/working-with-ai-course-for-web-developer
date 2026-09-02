# API Documentation with AI

## Learning Objectives
- Generate OpenAPI/Swagger documentation
- Create interactive API docs
- Use AI to document existing APIs

## Why Documentation Matters

An undocumented API is an unusable API. Good documentation:
- Reduces onboarding time for new developers
- Prevents integration errors
- Enables automatic client SDK generation
- Serves as a contract between frontend and backend

## OpenAPI Specification

OpenAPI (formerly Swagger) is the standard for API documentation:

```yaml
# openapi.yaml
openapi: 3.0.0
info:
  title: Task Management API
  version: 1.0.0
  description: API for managing tasks, projects, and teams

servers:
  - url: http://localhost:5000/api
    description: Development
  - url: https://api.example.com
    description: Production

paths:
  /tasks:
    get:
      summary: Get all tasks
      tags: [Tasks]
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
          description: List of tasks
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
      summary: Create a task
      tags: [Tasks]
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
          description: Task created
        400:
          description: Validation error
        401:
          description: Unauthorized

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

## Swagger UI with Express

```javascript
// Install: npm install swagger-ui-express yamljs

const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const swaggerDocument = YAML.load('./openapi.yaml');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument, {
  customCss: '.swagger-ui .topbar { display: none }',
  customSiteTitle: 'Task API Documentation'
}));
```

## Auto-Generate from JSDoc

```javascript
// Install: npm install swagger-jsdoc

const swaggerJsdoc = require('swagger-jsdoc');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Task Management API',
      version: '1.0.0'
    }
  },
  apis: ['./routes/*.js'] // Scan route files for JSDoc comments
};

const swaggerSpec = swaggerJsdoc(options);

/**
 * @swagger
 * /api/tasks:
 *   get:
 *     summary: Get all tasks
 *     tags: [Tasks]
 *     parameters:
 *       - in: query
 *         name: status
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: Success
 */
router.get('/tasks', taskController.getTasks);
```

## AI Prompt for Documentation

```
Generate OpenAPI 3.0 documentation for this Express.js API:
1. Scan all route files and extract endpoints
2. Generate request/response schemas from Mongoose models
3. Document authentication requirements
4. Add example requests and responses
5. Include error response schemas
6. Generate Postman collection as an alternative

Output both YAML and JSON formats.
```

## Postman Collections

```javascript
// Generate Postman collection from OpenAPI
const postman = require('openapi-to-postmanv2');

postman.convert({ type: 'string', data: yamlContent }, {}, (err, result) => {
  if (!err) {
    fs.writeFileSync('collection.json', JSON.stringify(result.output[0].data));
  }
});
```

## Documentation Best Practices

1. **Keep it updated**: Outdated docs are worse than no docs
2. **Include examples**: Show real request/response examples
3. **Document errors**: List all possible error codes
4. **Version your docs**: Match API version
5. **Make it interactive**: Swagger UI lets users test endpoints

## Practice Exercise

Document your Task Management API:
- Create OpenAPI specification for all endpoints
- Set up Swagger UI at `/api-docs`
- Add JSDoc comments to all routes
- Generate Postman collection
- Include authentication documentation

## Key Takeaways

- OpenAPI is the standard for API documentation
- Swagger UI provides interactive API testing
- AI can generate documentation from existing code
- Good documentation is essential for API adoption
