# Agent phát triển web là gì?

## Mục tiêu học tập
- Hiểu agent AI là gì trong phát triển web
- Tìm hiểu cách agent sử dụng kỹ năng để thực hiện tác vụ
- Xác định các agent phát triển web phổ biến

## Agent = Trợ lý chuyên biệt

**Agent** là trợ lý AI tự động có thể sử dụng nhiều kỹ năng để hoàn thành tác vụ phức tạp. Trong khi kỹ năng là một quy trình đơn lẻ, agent điều phối nhiều quy trình để đạt mục tiêu.

**Kỹ năng**: Tạo thành phần
**Agent**: Kiểm tra toàn bộ codebase để tìm lỗ hổng bảo mật, sửa chúng và tạo báo cáo

## Cách agent hoạt động

```
Yêu cầu người dùng
     │
     ▼
┌─────────────┐
│    Agent    │
│  (Điều phối) │
└──────┬──────┘
       │
   ┌───┴───┐
   ▼       ▼
┌─────┐ ┌─────┐ ┌─────┐
│Kỹ   │ │Kỹ   │ │Kỹ   │
│năng │ │năng │ │năng │
│  1  │ │  2  │ │  3  │
└─────┘ └─────┘ └─────┘
```

Agent:
1. **Nhận** yêu cầu cấp cao
2. **Lập kế hoạch** các bước cần thiết
3. **Chọn** kỹ năng phù hợp
4. **Thực thi** kỹ năng theo thứ tự
5. **Kết hợp** kết quả
6. **Báo cáo**发现

## Agent phát triển web phổ biến

### 1. Agent quét bảo mật
Quét codebase để tìm lỗ hổng:
- Kiểm tra rủi ro注入 SQL
- Xác định lỗ hổng XSS
- Đánh giá triển khai xác thực
- Kiểm tra lỗ hổng phụ thuộc
- Tạo báo cáo bảo mật

### 2. Agent phân tích hiệu suất
Phân tích hiệu suất ứng dụng:
- Đo kích thước bundle
- Xác định endpoint API chậm
- Đánh giá hiệu quả truy vấn cơ sở dữ liệu
- Kiểm tra tối ưu hóa hình ảnh
- Đề xuất cải tiến

### 3. Agent đánh giá mã
Đánh giá chất lượng mã:
- Kiểm tra mùi mã
- Xác minh quy ước đặt tên
- Đánh giá xử lý lỗi
- Kiểm tra覆盖率 kiểm thử
- Đề xuất tái cấu trúc

### 4. Agent tài liệu
Tạo và duy trì tài liệu:
- Tạo tài liệu API
- Tạo tệp README
- Cập nhật bình luận nội tuyến
- Tạo sơ đồ kiến trúc
- Duy trì nhật ký thay đổi

### 5. Agent kiểm thử
Tạo và chạy kiểm thử:
- Tạo kiểm thử đơn vị cho mã mới
- Tạo kiểm thử tích hợp
- Chạy bộ kiểm thử E2E
- Báo cáo số liệu覆盖率
- Xác định mã chưa được kiểm thử

## So sánh Agent vs Kỹ năng

| Khía cạnh | Kỹ năng | Agent |
|-----------|---------|-------|
| Phạm vi | Tác vụ đơn | Quy trình phức tạp |
| Tự động | Theo hướng dẫn | Ra quyết định |
| Kỹ năng sử dụng | Một | Nhiều |
| Đầu ra | Mã/tệp | Báo cáo + hành động |
| Ví dụ | Tạo thành phần | Kiểm toán bảo mật |

## Cấu trúc của agent

```markdown
# Agent quét bảo mật

## Mô tả
Quét codebase để tìm lỗ hổng bảo mật và tạo báo cáo.

## Khả năng
- Phân tích mã tĩnh
- Quét lỗ hổng phụ thuộc
- Đánh giá xác thực
- Kiểm tra xác thực đầu vào

## Quy trình
1. Quét tất cả tệp nguồn để tìm mẫu
2. Kiểm tra phụ thuộc để tìm lỗ hổng已知
3. Đánh giá xác thực và授权
4. Phân tích xác thực đầu vào
5. Tạo báo cáo bảo mật với发现
6. Đề xuất sửa cho mỗi vấn đề

## Kỹ năng sử dụng
- Khớp mẫu mã
- Phân tích phụ thuộc
- Đánh giá xác thực
- Tạo báo cáo

## Đầu ra
- Báo cáo bảo mật (markdown)
- Danh sách lỗ hổng với mức độ nghiêm trọng
- Đề xuất sửa cho mỗi vấn đề
```

## Xây dựng agent đầu tiên của bạn

```markdown
# Agent chất lượng mã

## Trigger
Khi người dùng yêu cầu đánh giá chất lượng mã hoặc kiểm tra vấn đề.

## Quy trình
1. **Kiểm tra Lint**: Chạy ESLint và báo lỗi
2. **Kiểm tra Type**: Chạy trình biên dịch TypeScript và báo lỗi
3. **Coverage kiểm thử**: Chạy kiểm thử và báo coverage
4. **Mùi mã**: Xác định mùi mã phổ biến
5. **Độ phức tạp**: Kiểm tra độ phức tạp cyclomatic
6. **Báo cáo**: Tạo báo cáo chất lượng

## Kỹ năng
- Kỹ năng linting
- Kiểm tra type
- Kỹ năng kiểm thử
- Kỹ năng phân tích mã

## Đầu ra
Báo cáo chất lượng với:
- Điểm tổng thể (0-100)
- Danh sách vấn đề theo loại
- Đề xuất cải tiến
```

## Prompt AI cho tạo agent

```
Tạo agent phát triển web:
1. Quét codebase React/Express
2. Xác định lỗ hổng bảo mật
3. Kiểm tra vấn đề hiệu suất
4. Đánh giá chất lượng mã
5. Tạo báo cáo toàn diện
6. Đề xuất sửa cho mỗi vấn đề

Agent nên sử dụng nhiều kỹ năng và đưa ra quyết định tự động về những gì cần kiểm tra.
```

## Bài tập thực hành

Tạo hai agent cho quy trình phát triển của bạn:
1. **Quét bảo mật**: Quét lỗ hổng trong API của bạn
2. **Phân tích hiệu suất**: Phân tích hiệu suất frontend của bạn

Tài liệu hóa mỗi agent với khả năng, quy trình và định dạng đầu ra.

## Điểm chính

- Agent là trợ lý tự động sử dụng nhiều kỹ năng
- Chúng xử lý tác vụ phức tạp đòi hỏi nhiều bước
- Agent đưa ra quyết định về những gì cần kiểm tra và cách kiểm tra
- Agent phổ biến bao gồm quét bảo mật và phân tích hiệu suất
