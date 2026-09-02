# Đạo đức và Chất lượng Code với AI

## Câu hỏi về trách nhiệm

Khi AI viết code, ai chịu trách nhiệm? Câu trả lời đơn giản: chính bạn. Mỗi dòng code do AI tạo ra đưa vào production đều là trách nhiệm của bạn. Bài học này đề cập đến các cân nhắc đạo đức và thực tế khi làm việc với code do AI tạo.

## Code Review là bắt buộc

Code do AI tạo cần được xem xét kỹ như code của lập trình viên junior — có thể còn kỹ hơn. Lý do:

**AI không hiểu logic nghiệp vụ.** Nó có thể tạo code hoàn hảo về mặt cú pháp nhưng hoàn toàn miss yêu cầu. Hàm sắp xếp người dùng theo "mức độ phổ biến" có thể dùng sai metric nếu bạn không giải thích "phổ biến" nghĩa là gì.

**AI có thể tạo bug tinh vi.** Race condition, lỗi off-by-one, kiểm tra null sai —这些都是 code do AI tạo hay gặp vì AI đang pattern-match, không suy luận về luồng dữ liệu cụ thể.

**AI có thể dùng pattern cũ.** Dữ liệu huấn luyện có ngày cắt. AI có thể gợi ý API deprecated, pattern React cũ (như class component khi bạn muốn hook), hoặc practices bảo mật không còn được khuyến nghị.

### Checklist review

Với mỗi đoạn code do AI tạo, kiểm tra:

1. **Nó có làm đúng yêu cầu không?** — Đọc code, đừng chỉ chạy
2. **Có vấn đề bảo mật không?** — SQL injection, XSS, secret cứng
3. **Có xử lý edge case không?** — Mảng rỗng, giá trị null, lỗi mạng
4. **Có nhất quán với codebase không?** — Quy ước đặt tên, cấu trúc file, pattern
5. **Có vấn đề hiệu suất không?** — Render lại không cần thiết, N+1 query, rò rỉ bộ nhớ

## Cân nhắc bảo mật

Công cụ AI được huấn luyện trên code công khai — bao gồm cả code có lỗ hổng bảo mật. Các vấn đề phổ biến:

### Credential cứng
AI có thể tạo code với API key hoặc chuỗi database placeholder. Luôn thay bằng biến môi trường.

```typescript
// ❌ AI có thể tạo thế này
const apiKey = "sk-1234567890abcdef";

// ✅ Luôn dùng biến môi trường
const apiKey = process.env.API_KEY;
```

### SQL Injection
Truy vấn database do AI tạo có thể không dùng parameterized query:

```typescript
// ❌ Dễ bị injection
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE id = $1';
const result = await db.query(query, [userId]);
```

### Lỗ hổng XSS
Khi tạo HTML hoặc component React, AI có thể không sanitize đúng input người dùng:

```typescript
// ❌ Nguy hiểm
<div dangerouslySetInnerHTML={{ __html: userContent }} />

// ✅ Đã sanitize
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userContent) }} />
```

## Sở hữu code và giấy phép

Công cụ AI được huấn luyện trên code mã nguồn mở. Điều này đặt ra câu hỏi:

- **Có thể dùng code do AI tạo trong dự án thương mại không?** Thường được, nhưng hãy kiểm tra điều khoản dịch vụ của công cụ AI.
- **Code do AI tạo có bản quyền không?** Chưa có câu trả lời pháp lý rõ ràng. Hầu hết chuyên gia đồng ý code hoàn toàn do AI tạo không được bảo hộ, nhưng code bạn sửa đổi đáng kể có thể được.
- **Có nên ghi nhận sự hỗ trợ của AI không?** Đây là quyết định của team. Một số team ghi nhận trong commit message hoặc code comment.

## Duy trì chất lượng code

### Thiết lập tiêu chuẩn team

Nếu team dùng công cụ AI, hãy thống nhất:
- **Yêu cầu review** cho code do AI tạo
- **Tiêu chuẩn test** (code AI cũng cần test)
- **Kỳ vọng tài liệu** (giải thích code làm gì, không chỉ ghi AI viết)
- **Quét bảo mật** như một phần CI/CD

### Đừng chấp nhận mù quáng

Rủi ro lớn nhất không phải AI viết code dở — mà là lập trình viên chấp nhận mà không hiểu. Nếu bạn không giải thích được code làm gì, đừng merge.

### Tiếp tục học hỏi

AI là công cụ, không thay thế sự hiểu biết. Dùng AI để viết code nhanh hơn, nhưng đảm bảo bạn hiểu:
- Tại sao code hoạt động
- Nó dùng pattern gì
- Nó có thể fail thế nào
- Cách cải thiện nó

## Yếu tố con người

AI có thể viết code, nhưng không thể:
- Hiểu nhu cầu người dùng
- Ra quyết định sản phẩm
- Điều hướng động lực team
- Chịu trách nhiệm cho bug trên production

Giá trị của bạn không nằm ở việc gõ code — mà ở giải quyết vấn đề, ra quyết định và xây dựng thứ có ý nghĩa. AI chỉ giúp bạn làm nhanh hơn.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ thiết lập workspace phát triển với đúng công cụ AI và extension để tối đa hóa năng suất.
