# Tạo agent: Phân tích hiệu suất

## Mục tiêu học tập
- Xây dựng agent phân tích hiệu suất ứng dụng
- Xác định nút thắt cổ chai trong frontend và backend
- Tạo khuyến nghị tối ưu hóa

## Tại sao phân tích hiệu suất?

Ứng dụng chậm mất người dùng. Độ trễ 1 giây trong tải trang có thể giảm chuyển đổi 7%. Phân tích hiệu suất xác định nút thắt cổ chai trước khi người dùng注意到.

## Số liệu hiệu suất

### Số liệu frontend
- **First Contentful Paint (FCP)**: Thời gian đến nội dung hiển thị đầu tiên
- **Largest Contentful Paint (LCP)**: Thời gian đến phần tử nội dung lớn nhất
- **Time to Interactive (TTI)**: Thời gian直到 trang完全 tương tác
- **Cumulative Layout Shift (CLS)**: Ổn định视觉
- **Kích thước bundle**: Tổng JavaScript được tải xuống

### Số liệu backend
- **Thời gian phản hồi**: Thời gian xử lý yêu cầu
- **Thông lượng**: Yêu cầu mỗi giây
- **Thời gian truy vấn DB**: Thời gian cho thao tác cơ sở dữ liệu
- **Sử dụng bộ nhớ**: Tiêu thụ RAM
- **Sử dụng CPU**: Sử dụng bộ xử lý

## Định nghĩa agent

Tạo `.github/copilot/agents/performance-analyzer.md`:

```markdown
# Agent phân tích hiệu suất

## Mô tả
Phân tích hiệu suất ứng dụng và tạo khuyến nghị tối ưu hóa.

## Trigger
Khi người dùng yêu cầu kiểm tra hiệu suất, tối ưu hóa tốc độ hoặc phân tích nút thắt cổ chai.

## Quy trình

### Giai đoạn 1: Phân tích bundle
Phân tích bundle frontend:
1. Chạy trình phân tích bundle
2. Xác định phụ thuộc lớn
3. Kiểm tra chia nhỏ mã
4. Đánh giá triển khai tải懒

### Giai đoạn 2: Hiệu suất API
Phân tích endpoint backend:
1. Kiểm tra thời gian phản hồi cho tất cả endpoint
2. Xác định truy vấn cơ sở dữ liệu chậm
3. Đánh giá vấn đề truy vấn N+1
4. Kiểm tra triển khai bộ đệm

### Giai đoạn 3: Tối ưu hóa hình ảnh
Đánh giá xử lý hình ảnh:
1. Kiểm tra định dạng hình ảnh (WebP, AVIF)
2. Xác minh nén hình ảnh
3. Kiểm tra tải懒
4. Đánh giá hình ảnhResponsive

### Giai đoạn 4: Chiến lược bộ đệm
Đánh giá triển khai bộ đệm:
1. Kiểm tra tiêu đề bộ đệm trình duyệt
2. Đánh giá cấu hình CDN
3. Phân tích bộ đệm Redis/bộ nhớ
4. Kiểm tra cài đặt bộ đệm React Query

### Giai đoạn 5: Tạo báo cáo
Tạo báo cáo hiệu suất với:
- Điểm hiệu suất tổng thể
- Danh sách vấn đề theo tác động
- Khuyến nghị tối ưu hóa
- Cải tiến dự kiến cho mỗi sửa

## Định dạng đầu ra

```markdown
# Báo cáo phân tích hiệu suất

## Điểm tổng thể: 72/100

## Vấn đề frontend

### [CAO] Kích thước bundle lớn (2.5MB)
**Tác động**: Tải ban đầu chậm trên mạng di động
**Nguyên nhân**: Moment.js được bao gồm (290KB gzipped)
**Sửa**: Thay thế bằng date-fns (20KB gzipped)
**Cải tiến dự kiến**: -270KB kích thước bundle

### [TRUNG BÌNH] Không chia nhỏ mã
**Tác động**: Người dùng tải tất cả mã upfront
**Sửa**: Triển khai React.lazy() cho chia nhỏ dựa trên route
**Cải tiến dự kiến**: -40% bundle ban đầu

## Vấn đề backend

### [CAO] Truy vấn N+1 trong GET /api/tasks
**Tác động**: Thời gian phản hồi 500ms với 100 nhiệm vụ
**Nguyên nhân**: Lấy người được giao cho mỗi nhiệm vụ riêng biệt
**Sửa**: Sử dụng .populate('assignee') trong truy vấn đơn
**Cải tiến dự kiến**: -450ms thời gian phản hồi

### [TRUNG BÌNH] Không có bộ đệm trên GET /api/products
**Tác động**: Chạm cơ sở dữ liệu trên mỗi yêu cầu
**Sửa**: Thêm bộ đệm Redis với TTL 5 phút
**Cải tiến dự kiến**: -90% thời gian phản hồi
```

## Triển khai

```javascript
// performance-analyzer.js
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

class PerformanceAnalyzer {
  constructor(projectPath) {
    this.projectPath = projectPath;
    this.issues = [];
  }

  async analyze() {
    await this.analyzeBundleSize();
    await this.analyzeAPIPerformance();
    await this.analyzeImageOptimization();
    await this.analyzeCaching();
    return this.generateReport();
  }

  async analyzeBundleSize() {
    // Kiểm tra phụ thuộc lớn
    const packageJson = JSON.parse(
      fs.readFileSync(path.join(this.projectPath, 'package.json'), 'utf-8')
    );

    const largeDependencies = {
      'moment': { size: '290KB', alternative: 'date-fns', altSize: '20KB' },
      'lodash': { size: '72KB', alternative: 'lodash-es', altSize: '25KB' },
      'jquery': { size: '87KB', alternative: 'vanilla JS', altSize: '0KB' }
    };

    const deps = { ...packageJson.dependencies, ...packageJson.devDependencies };

    Object.keys(deps).forEach(dep => {
      if (largeDependencies[dep]) {
        this.issues.push({
          severity: 'HIGH',
          category: 'Kích thước bundle',
          title: `Phụ thuộc lớn: ${dep}`,
          impact: `Thêm ${largeDependencies[dep].size} vào bundle`,
          fix: `Thay thế bằng ${largeDependencies[dep].alternative} (${largeDependencies[dep].altSize})`
        });
      }
    });

    // Kiểm tra chia nhỏ mã
    const srcFiles = this.getFiles('src', '.tsx');
    const hasLazyLoading = srcFiles.some(file => {
      const content = fs.readFileSync(file, 'utf-8');
      return content.includes('React.lazy') || content.includes('dynamic(');
    });

    if (!hasLazyLoading && srcFiles.length > 10) {
      this.issues.push({
        severity: 'MEDIUM',
        category: 'Kích thước bundle',
        title: 'Không phát hiện chia nhỏ mã',
        impact: 'Người dùng tải tất cả mã upfront',
        fix: 'Triển khai chia nhỏ mã dựa trên route với React.lazy()'
      });
    }
  }

  async analyzeAPIPerformance() {
    // Kiểm tra truy vấn N+1
    const controllerFiles = this.getFiles('controllers', '.js');

    controllerFiles.forEach(file => {
      const content = fs.readFileSync(file, 'utf-8');

      // Kiểm tra vòng lặp với lệnh gọi cơ sở dữ liệu
      const loopPattern = /for\s*\(.*\)\s*\{[\s\S]*?(?:find|findOne|findById)/g;
      if (loopPattern.test(content)) {
        this.issues.push({
          severity: 'HIGH',
          category: 'Hiệu suất API',
          title: `Truy vấn N+1 tiềm năng trong ${path.basename(file)}`,
          impact: 'Thời gian phản hồi chậm với nhiều bản ghi',
          fix: 'Sử dụng populate() hoặc truy vấn hàng loạt'
        });
      }
    });

    // Kiểm tra thiếu index
    const modelFiles = this.getFiles('models', '.js');
    modelFiles.forEach(file => {
      const content = fs.readFileSync(file, 'utf-8');

      if (!content.includes('.index(')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'Hiệu suất API',
          title: `Không có index được định nghĩa trong ${path.basename(file)}`,
          impact: 'Truy vấn chậm trên bộ sưu tập lớn',
          fix: 'Thêm index cho trường truy vấn thường xuyên'
        });
      }
    });
  }

  async analyzeImageOptimization() {
    // Kiểm tra tối ưu hóa hình ảnh
    const nextConfig = path.join(this.projectPath, 'next.config.js');
    if (fs.existsSync(nextConfig)) {
      const content = fs.readFileSync(nextConfig, 'utf-8');
      if (!content.includes('images')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'Tối ưu hóa hình ảnh',
          title: 'Tối ưu hóa hình ảnh Next.js chưa được cấu hình',
          impact: 'Kích thước hình ảnh lớn hơn, tải chậm hơn',
          fix: 'Cấu hình next/image với miền và định dạng đúng'
        });
      }
    }
  }

  async analyzeCaching() {
    // Kiểm tra tiêu đề bộ đệm
    const appFile = path.join(this.projectPath, 'app.js');
    if (fs.existsSync(appFile)) {
      const content = fs.readFileSync(appFile, 'utf-8');
      if (!content.includes('Cache-Control')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'Bộ đệm',
          title: 'Không có tiêu đề bộ đệm được cấu hình',
          impact: 'Tải lại tài nguyên tĩnh lặp đi lặp lại',
          fix: 'Thêm tiêu đề Cache-Control cho tệp tĩnh'
        });
      }
    }
  }

  generateReport() {
    const high = this.issues.filter(i => i.severity === 'HIGH').length;
    const medium = this.issues.filter(i => i.severity === 'MEDIUM').length;
    const low = this.issues.filter(i => i.severity === 'LOW').length;

    const score = Math.max(0, 100 - (high * 20) - (medium * 10) - (low * 5));

    return {
      score,
      summary: { high, medium, low },
      issues: this.issues
    };
  }
}

module.exports = PerformanceAnalyzer;
```

## Prompt AI cho agent hiệu suất

```
Tạo agent phân tích hiệu suất:
1. Phân tích kích thước bundle frontend
2. Xác định endpoint API chậm
3. Kiểm tra vấn đề truy vấn N+1
4. Đánh giá triển khai bộ đệm
5. Tạo khuyến nghị tối ưu hóa

Bao gồm định nghĩa agent và mã triển khai.
```

## Bài tập thực hành

Xây dựng và chạy trình phân tích hiệu suất:
1. Tạo tệp định nghĩa agent
2. Triển khai逻辑 phân tích
3. Chạy trên ứng dụng quản lý nhiệm vụ
4. Đánh giá các phát hiện
5. Triển khai 3 tối ưu hóa hàng đầu

## Điểm chính

- Agent hiệu suất tự động xác định nút thắt cổ chai
- Kiểm tra kích thước bundle, hiệu suất API và bộ đệm
- Tạo khuyến nghị tối ưu hóa có thể hành động
- Chạy thường xuyên để duy trì tiêu chuẩn hiệu suất
