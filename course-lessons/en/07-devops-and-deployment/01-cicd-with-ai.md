# CI/CD with AI

## Learning Objectives
- Set up GitHub Actions for automated workflows
- Create CI/CD pipelines for testing and deployment
- Use AI to generate workflow configurations

## Why CI/CD Matters

Continuous Integration (CI) automatically tests your code when you push changes. Continuous Deployment (CD) automatically deploys when tests pass. Together, they catch bugs early and ship features faster.

**Without CI/CD**: Manual testing → Manual deployment → Human errors → Slow releases
**With CI/CD**: Automated tests → Automated deployment → Consistent → Fast releases

## GitHub Actions Basics

GitHub Actions runs workflows triggered by events (push, pull request, schedule):

```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

## Complete CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy Application

on:
  push:
    branches: [main]

env:
  NODE_ENV: production

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test
      - run: npm run build

  deploy-staging:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: staging
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to Staging
        run: |
          # Deploy to staging environment
          echo "Deploying to staging..."
      - name: Run E2E tests
        run: npm run test:e2e

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to Production
        run: |
          # Deploy to production environment
          echo "Deploying to production..."
```

## Testing in CI

```yaml
# Run tests with coverage
- name: Run tests with coverage
  run: npm run test:coverage

- name: Upload coverage to Codecov
  uses: codecov/codecov-action@v3
  with:
    token: ${{ secrets.CODECOV_TOKEN }}

# Run tests in parallel
- name: Run tests
  run: npm test -- --shard=${{ matrix.shard }}/4
  strategy:
    matrix:
      shard: [1, 2, 3, 4]
```

## Environment Secrets

```yaml
# Store secrets in GitHub Settings > Secrets
steps:
  - name: Deploy
    env:
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
      API_KEY: ${{ secrets.API_KEY }}
      AWS_ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY }}
    run: npm run deploy
```

## Caching Dependencies

```yaml
# Cache npm dependencies
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-

# Or use setup-node cache
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'
```

## AI Prompt for CI/CD

```
Create a GitHub Actions CI/CD pipeline for a Node.js application with:
1. Linting and testing on every push
2. Build and test on pull requests
3. Deploy to staging on main branch push
4. Deploy to production after staging tests pass
5. Cache dependencies for faster builds
6. Environment-specific secrets
7. Slack notifications on deployment
8. Automatic rollback on failure

Include both frontend and backend deployment steps.
```

## Matrix Testing

Test across multiple Node.js versions:

```yaml
test:
  runs-on: ubuntu-latest
  strategy:
    matrix:
      node-version: [18, 20, 22]

  steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm test
```

## Practice Exercise

Set up CI/CD for your Task Management application:
- GitHub Actions workflow for testing
- Automated deployment to staging
- Environment variables and secrets
- Coverage reporting
- Slack notifications

## Key Takeaways

- CI/CD automates testing and deployment
- GitHub Actions is the most popular CI/CD platform
- Caching speeds up workflow execution
- Secrets keep sensitive data secure
