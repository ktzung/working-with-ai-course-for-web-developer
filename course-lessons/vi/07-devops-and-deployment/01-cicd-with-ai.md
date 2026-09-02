# CI/CD với AI

## Mục tiêu học tập
- Thiết lập GitHub Actions cho quy trình tự động
- Tạo pipeline CI/CD cho kiểm thử và triển khai
- Sử dụng AI để tạo cấu hình workflow

## Tại sao CI/CD quan trọng?

Tích hợp liên tục (CI) tự động kiểm thử mã khi bạn đẩy thay đổi. Triển khai liên tục (CD) tự động triển khai khi kiểm thử通过. Cùng nhau, chúng phát hiện lỗi sớm và ship tính năng nhanh hơn.

**Không có CI/CD**: Kiểm thử thủ công → Triển khai thủ công → Lỗi con người → Phát hành chậm
**Có CI/CD**: Kiểm thử tự động → Triển khai tự động → Nhất quán → Phát hành nhanh

## GitHub Actions cơ bản

GitHub Actions chạy workflow được触发 bởi sự kiện (push, pull request, lịch trình):

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

      - name: Thiết lập Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Cài đặt phụ thuộc
        run: npm ci

      - name: Chạy linter
        run: npm run lint

      - name: Chạy kiểm thử
        run: npm test

      - name: Build
        run: npm run build
```

## Pipeline CI/CD hoàn chỉnh

```yaml
# .github/workflows/deploy.yml
name: Triển khai ứng dụng

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
      - name: Triển khai lên Staging
        run: |
          # Triển khai lên môi trường staging
          echo "Đang triển khai lên staging..."
      - name: Chạy kiểm thử E2E
        run: npm run test:e2e

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      - name: Triển khai lên Production
        run: |
          # Triển khai lên môi trường production
          echo "Đang triển khai lên production..."
```

## Kiểm thử trong CI

```yaml
# Chạy kiểm thử với覆盖率
- name: Chạy kiểm thử với覆盖率
  run: npm run test:coverage

- name: Tải覆盖率 lên Codecov
  uses: codecov/codecov-action@v3
  with:
    token: ${{ secrets.CODECOV_TOKEN }}

# Chạy kiểm thử song song
- name: Chạy kiểm thử
  run: npm test -- --shard=${{ matrix.shard }}/4
  strategy:
    matrix:
      shard: [1, 2, 3, 4]
```

## Bí mật môi trường

```yaml
# Lưu trữ bí mật trong GitHub Settings > Secrets
steps:
  - name: Triển khai
    env:
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
      API_KEY: ${{ secrets.API_KEY }}
      AWS_ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY }}
    run: npm run deploy
```

## Bộ đệm phụ thuộc

```yaml
# Bộ đệm phụ thuộc npm
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-

# Hoặc sử dụng bộ đệm setup-node
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'
```

## Prompt AI cho CI/CD

```
Tạo pipeline CI/CD GitHub Actions cho ứng dụng Node.js với:
1. Lint và kiểm thử trên mỗi push
2. Build và kiểm thử trên pull request
3. Triển khai lên staging khi push nhánh main
4. Triển khai lên production sau khi kiểm thử staging通过
5. Bộ đệm phụ thuộc cho build nhanh hơn
6. Bí mật cụ thể cho môi trường
7. Thông báo Slack khi triển khai
8. Tự động rollback khi thất bại

Bao gồm cả bước triển khai frontend và backend.
```

## Kiểm thử ma trận

Kiểm thử trên nhiều phiên bản Node.js:

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

## Bài tập thực hành

Thiết lập CI/CD cho ứng dụng quản lý nhiệm vụ:
- Workflow GitHub Actions cho kiểm thử
- Triển khai tự động lên staging
- Biến môi trường và bí mật
- Báo cáo覆盖率
- Thông báo Slack

## Điểm chính

- CI/CD tự động hóa kiểm thử và triển khai
- GitHub Actions là nền tảng CI/CD phổ biến nhất
- Bộ đệm tăng tốc thực thi workflow
- Bí mật giữ dữ liệu nhạy cảm an toàn
