# Tạo agent: Quét bảo mật

## Mục tiêu học tập
- Xây dựng agent quét lỗ hổng bảo mật
- Kiểm tra các vấn đề bảo mật web phổ biến
- Tạo báo cáo bảo mật có thể hành động

## Tại sao quét bảo mật?

Lỗ hổng bảo mật có thể dẫn đến vi phạm dữ liệu, tổn thất tài chính và danh tiếng bị tổn hại. Trình quét bảo mật tự động phát hiện vấn đề trước khi chúng到达 production.

## Vấn đề bảo mật web phổ biến

1. **Injection SQL**: Đầu vào người dùng chưa được làm sạch trong truy vấn cơ sở dữ liệu
2. **XSS (Cross-Site Scripting)**: Nội dung người dùng chưa được thoát trong HTML
3. **CSRF (Cross-Site Request Forgery)**: Thiếu token CSRF
4. **Xác thực không an toàn**: Mật khẩu yếu, thiếu giới hạn tốc độ
5. **Lộ dữ liệu nhạy cảm**: Ghi log mật khẩu, thiếu mã hóa
6. **Kiểm soát truy cập hỏng**: Thiếu kiểm tra授权
7. **Phụ thuộc có lỗ hổng**: Gói cũ với CVE已知

## Định nghĩa agent

Tạo `.github/copilot/agents/security-scanner.md`:

```markdown
# Agent quét bảo mật

## Mô tả
Quét codebase để tìm lỗ hổng bảo mật và tạo báo cáo có thể hành động.

## Trigger
Khi người dùng yêu cầu kiểm tra bảo mật, quét lỗ hổng hoặc kiểm toán codebase.

## Quy trình

### Giai đoạn 1: Phân tích tĩnh
Quét tất cả tệp nguồn để tìm mẫu nguy hiểm:

#### Rủi ro Injection SQL
- Tìm kiếm nối chuỗi trong truy vấn
- Kiểm tra SQL thô không có tham số hóa
- Cờ: `query("SELECT * FROM users WHERE id = " + userId)`

#### Rủi ro XSS
- Tìm kiếm `dangerouslySetInnerHTML`
- Kiểm tra đầu vào người dùng chưa được thoát trong JSX
- Cờ: `<div dangerouslySetInnerHTML={{__html: userContent}} />`

#### Lộ dữ liệu nhạy cảm
- Tìm kiếm bí mật硬编码 (khóa API, mật khẩu)
- Kiểm tra mật khẩu trong log
- Cờ: `console.log('Password:', password)`

#### Phụ thuộc không an toàn
- Chạy `npm audit` và phân tích kết quả
- Kiểm tra gói cũ
- Cờ gói với CVE已知

### Giai đoạn 2: Đánh giá xác thực
Kiểm tra triển khai xác thực:

1. **Băm mật khẩu**: Xác minh bcrypt hoặc tương tự được sử dụng
2. **Bảo mật JWT**: Kiểm tra xác thực token đúng cách
3. **Giới hạn tốc độ**: Xác minh endpoint đăng nhập có giới hạn tốc độ
4. **Quản lý phiên**: Kiểm tra cài đặt cookie bảo mật

### Giai đoạn 3: Đánh giá授权
Kiểm tra kiểm soát truy cập:

1. **Bảo vệ route**: Xác minh tất cả route nhạy cảm có middleware xác thực
2. **Truy cập dựa trên vai trò**: Kiểm tra xác thực vai trò đúng cách
3. **Sở hữu tài nguyên**: Xác minh người dùng chỉ có thể truy cập dữ liệu của họ

### Giai đoạn 4: Xác thực đầu vào
Kiểm tra xử lý đầu vào:

1. **Xác thực yêu cầu**: Xác minh tất cả đầu vào được xác thực
2. **Bảo mật tải tệp lên**: Kiểm tra xác thực loại và kích thước tệp
3. **Tham số hóa SQL**: Xác minh truy vấn sử dụng tham số

### Giai đoạn 5: Tạo báo cáo
Tạo báo cáo bảo mật với:
- Tóm tắt发现
- Danh sách chi tiết lỗ hổng
- Mức độ nghiêm trọng (Nghiêm trọng, Cao, Trung bình, Thấp)
- Đề xuất sửa cho mỗi vấn đề
- Tham khảo hướng dẫn OWASP

## Định dạng đầu ra

```markdown
# Báo cáo quét bảo mật

## Tóm tắt
- Nghiêm trọng: X vấn đề
- Cao: X vấn đề
- Trung bình: X vấn đề
- Thấp: X vấn đề

## Phát hiện

### [NGHIÊM TRỌNG] Injection SQL trong routes/users.js:45
**Mô tả**: Đầu vào người dùng nối trực tiếp trong truy vấn SQL
**Mã**: `query("SELECT * FROM users WHERE id = " + req.params.id)`
**Sửa**: Sử dụng truy vấn có tham số
**Tham khảo**: https://owasp.org/www-community/attacks/SQL_Injection

### [CAO] Thiếu giới hạn tốc độ trên /api/auth/login
**Mô tả**: Endpoint đăng nhập không có giới hạn tốc độ
**Sửa**: Thêm middleware express-rate-limit
```

## Triển khai

```javascript
// security-scanner.js
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

class SecurityScanner {
  constructor(projectPath) {
    this.projectPath = projectPath;
    this.findings = [];
  }

  async scan() {
    await this.scanForSQLInjection();
    await this.scanForXSS();
    await this.scanForHardcodedSecrets();
    await this.checkDependencies();
    await this.reviewAuthentication();
    return this.generateReport();
  }

  async scanForSQLInjection() {
    const files = this.getJavaScriptFiles();
    for (const file of files) {
      const content = fs.readFileSync(file, 'utf-8');
      const lines = content.split('\n');

      lines.forEach((line, index) => {
        // Kiểm tra nối chuỗi trong truy vấn
        if (line.match(/query\s*\(\s*['"`].*\+/)) {
          this.findings.push({
            severity: 'CRITICAL',
            type: 'Injection SQL',
            file: file,
            line: index + 1,
            code: line.trim(),
            fix: 'Sử dụng truy vấn có tham số'
          });
        }
      });
    }
  }

  async scanForXSS() {
    const files = this.getJavaScriptFiles();
    for (const file of files) {
      const content = fs.readFileSync(file, 'utf-8');

      if (content.includes('dangerouslySetInnerHTML')) {
        this.findings.push({
          severity: 'HIGH',
          type: 'Rủi ro XSS',
          file: file,
          fix: 'Làm sạch nội dung HTML trước khi render'
        });
      }
    }
  }

  async checkDependencies() {
    try {
      const audit = execSync('npm audit --json', { cwd: this.projectPath });
      const results = JSON.parse(audit);

      if (results.vulnerabilities) {
        Object.entries(results.vulnerabilities).forEach(([pkg, vuln]) => {
          this.findings.push({
            severity: vuln.severity.toUpperCase(),
            type: 'Phụ thuộc có lỗ hổng',
            file: `package.json (${pkg})`,
            fix: `Cập nhật lên phiên bản ${vuln.fixAvailable || 'latest'}`
          });
        });
      }
    } catch (error) {
      // npm audit trả về mã thoát khác không khi发现 lỗ hổng
    }
  }

  generateReport() {
    const critical = this.findings.filter(f => f.severity === 'CRITICAL').length;
    const high = this.findings.filter(f => f.severity === 'HIGH').length;
    const medium = this.findings.filter(f => f.severity === 'MEDIUM').length;
    const low = this.findings.filter(f => f.severity === 'LOW').length;

    return {
      summary: { critical, high, medium, low, total: this.findings.length },
      findings: this.findings
    };
  }
}

module.exports = SecurityScanner;
```

## Prompt AI cho agent bảo mật

```
Tạo agent quét bảo mật:
1. Quét mã Express.js để tìm rủi ro injection SQL
2. Kiểm tra thành phần React để tìm lỗ hổng XSS
3. Đánh giá triển khai xác thực
4. Kiểm tra bí mật硬编码
5. Kiểm toán phụ thuộc npm
6. Tạo báo cáo bảo mật với mức độ nghiêm trọng

Bao gồm định nghĩa agent và mã triển khai.
```

## Bài tập thực hành

Xây dựng và chạy trình quét bảo mật trên dự án của bạn:
1. Tạo tệp định nghĩa agent
2. Triển khai逻辑 quét
3. Chạy trên API quản lý nhiệm vụ
4. Đánh giá các phát hiện
5. Sửa các vấn đề nghiêm trọng và cao

## Điểm chính

- Trình quét bảo mật tự động phát hiện lỗ hổng
- Kiểm tra injection SQL, XSS, vấn đề xác thực và phụ thuộc có lỗ hổng
- Tạo báo cáo với mức độ nghiêm trọng và đề xuất sửa
- Chạy thường xuyên để phát hiện lỗ hổng mới
