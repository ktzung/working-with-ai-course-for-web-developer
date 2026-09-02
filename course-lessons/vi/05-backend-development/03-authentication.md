# Xác thực với AI

## Mục tiêu học tập
- Triển khai xác thực dựa trên JWT
- Hiểu luồng OAuth 2.0
- Quản lý phiên làm việc an toàn với sự hỗ trợ của AI

## Tại sao xác thực quan trọng?

Xác thực trả lời một câu hỏi: "Bạn là ai?" Nó là người gác cổng của ứng dụng. Nếu làm sai, bạn暴露 lộ dữ liệu người dùng. Nếu làm đúng, người dùng tin tưởng nền tảng của bạn.

## JWT (JSON Web Tokens)

JWT là phương thức xác thực phổ biến nhất cho API hiện đại. Nó không trạng thái — server không cần lưu trữ dữ liệu phiên.

**Cách JWT hoạt động**:
1. Người dùng đăng nhập với thông tin xác thực
2. Server xác thực và tạo JWT
3. Client lưu trữ JWT (thường trong httpOnly cookie)
4. Client gửi JWT với mỗi yêu cầu
5. Server xác minh JWT và cấp quyền truy cập

**Cấu trúc JWT**: `header.payload.signature`

```javascript
// Ví dụ JWT payload
{
  "userId": "abc123",
  "email": "user@example.com",
  "role": "admin",
  "iat": 1694000000,  // thời điểm phát hành
  "exp": 1694086400   // thời điểm hết hạn
}
```

## Triển khai JWT với Express

```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');

const authenticate = (req, res, next) => {
  const token = req.cookies.token || req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Yêu cầu xác thực' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Token không hợp lệ hoặc đã hết hạn' });
  }
};

// Tạo token
const generateToken = (user) => {
  return jwt.sign(
    { userId: user.id, email: user.email, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
};
```

## OAuth 2.0

OAuth cho phép người dùng đăng nhập bằng Google, GitHub hoặc nhà cung cấp khác. Không cần lưu trữ mật khẩu.

**Luồng OAuth**:
1. Người dùng nhấp "Đăng nhập bằng Google"
2. Chuyển hướng đến trang授权 của Google
3. Người dùng cấp quyền
4. Google chuyển hướng lại với mã授权
5. Server đổi mã lấy token truy cập
6. Server lấy hồ sơ người dùng từ Google

```javascript
// Sử dụng Passport.js cho OAuth
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  async (accessToken, refreshToken, profile, done) => {
    let user = await User.findOne({ googleId: profile.id });
    if (!user) {
      user = await User.create({
        googleId: profile.id,
        email: profile.emails[0].value,
        name: profile.displayName,
        avatar: profile.photos[0].value
      });
    }
    done(null, user);
  }
));
```

## Prompt AI cho hệ thống xác thực

```
Triển khai hệ thống xác thực hoàn chỉnh cho Express.js với:
1. Đăng ký cục bộ với email/mật khẩu (băm bcrypt)
2. Đăng nhập với token JWT (token truy cập + token làm mới)
3. Tích hợp Google OAuth 2.0
4. Đặt lại mật khẩu qua email
5. Kiểm soát truy cập dựa trên vai trò (quản trị, người dùng)
6. Giới hạn tốc độ trên các lần thử đăng nhập

Bao gồm middleware để bảo vệ route và xử lý lỗi đúng cách.
```

## Quản lý phiên

**Token truy cập**: Thời gian sống ngắn (15 phút), dùng cho yêu cầu API
**Token làm mới**: Thời gian sống dài (7 ngày), dùng để lấy token truy cập mới

```javascript
// Endpoint làm mới token
router.post('/refresh', async (req, res) => {
  const { refreshToken } = req.cookies;

  if (!refreshToken) {
    return res.status(401).json({ error: 'Yêu cầu token làm mới' });
  }

  try {
    const decoded = jwt.verify(refreshToken, process.env.REFRESH_SECRET);
    const user = await User.findById(decoded.userId);

    const newAccessToken = generateAccessToken(user);
    const newRefreshToken = generateRefreshToken(user);

    res.cookie('refreshToken', newRefreshToken, {
      httpOnly: true,
      secure: true,
      sameSite: 'strict',
      maxAge: 7 * 24 * 60 * 60 * 1000
    });

    res.json({ accessToken: newAccessToken });
  } catch (error) {
    res.status(401).json({ error: 'Token làm mới không hợp lệ' });
  }
});
```

## Thực hành tốt nhất về bảo mật

1. **Băm mật khẩu** với bcrypt (vòng salt ≥ 12)
2. **Dùng httpOnly cookie** cho token (ngăn XSS)
3. **Bật HTTPS** trong môi trường production
4. **Triển khai bảo vệ CSRF** cho xác thực dựa trên cookie
5. **Giới hạn tốc độ** thử đăng nhập (5 lần mỗi 15 phút)
6. **Xác thực đầu vào** để ngăn tấn công注入

## Bài tập thực hành

Xây dựng hệ thống xác thực hoàn chỉnh cho nền tảng blog:
- Đăng ký và đăng nhập email/mật khẩu
- Tích hợp Google OAuth
- Route được bảo vệ để tạo/sửa bài viết
- Route chỉ dành cho quản trị để quản lý người dùng
- Luồng đặt lại mật khẩu

## Điểm chính

- JWT không trạng thái và có thể mở rộng cho xác thực API
- OAuth đơn giản hóa đăng nhập bằng cách利用 nhà cung cấp hiện có
- Mẫu token truy cập/làm mới cân bằng bảo mật và trải nghiệm người dùng
- AI có thể tạo hệ thống xác thực hoàn chỉnh từ yêu cầu
