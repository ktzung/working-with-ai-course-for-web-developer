# Authentication with AI

## Learning Objectives
- Implement JWT-based authentication
- Understand OAuth 2.0 flows
- Manage sessions securely with AI assistance

## Why Authentication Matters

Authentication answers one question: "Who are you?" It's the gatekeeper of your application. Get it wrong, and you expose user data. Get it right, and users trust your platform.

## JWT (JSON Web Tokens)

JWT is the most popular authentication method for modern APIs. It's stateless — the server doesn't need to store session data.

**How JWT Works**:
1. User logs in with credentials
2. Server validates and creates a JWT
3. Client stores JWT (usually in httpOnly cookie)
4. Client sends JWT with each request
5. Server verifies JWT and grants access

**JWT Structure**: `header.payload.signature`

```javascript
// JWT payload example
{
  "userId": "abc123",
  "email": "user@example.com",
  "role": "admin",
  "iat": 1694000000,  // issued at
  "exp": 1694086400   // expires
}
```

## Implementing JWT with Express

```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');

const authenticate = (req, res, next) => {
  const token = req.cookies.token || req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Authentication required' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid or expired token' });
  }
};

// Generate token
const generateToken = (user) => {
  return jwt.sign(
    { userId: user.id, email: user.email, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
};
```

## OAuth 2.0

OAuth lets users log in with Google, GitHub, or other providers. No password storage needed.

**OAuth Flow**:
1. User clicks "Login with Google"
2. Redirect to Google's authorization page
3. User grants permission
4. Google redirects back with authorization code
5. Server exchanges code for access token
6. Server fetches user profile from Google

```javascript
// Using Passport.js for OAuth
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

## AI Prompt for Authentication

```
Implement a complete authentication system for Express.js with:
1. Local registration with email/password (bcrypt hashing)
2. Login with JWT tokens (access + refresh tokens)
3. Google OAuth 2.0 integration
4. Password reset via email
5. Role-based access control (admin, user)
6. Rate limiting on login attempts

Include middleware for protecting routes and proper error handling.
```

## Session Management

**Access Tokens**: Short-lived (15 min), used for API requests
**Refresh Tokens**: Long-lived (7 days), used to get new access tokens

```javascript
// Token refresh endpoint
router.post('/refresh', async (req, res) => {
  const { refreshToken } = req.cookies;

  if (!refreshToken) {
    return res.status(401).json({ error: 'Refresh token required' });
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
    res.status(401).json({ error: 'Invalid refresh token' });
  }
});
```

## Security Best Practices

1. **Hash passwords** with bcrypt (salt rounds ≥ 12)
2. **Use httpOnly cookies** for tokens (prevents XSS)
3. **Enable HTTPS** in production
4. **Implement CSRF protection** for cookie-based auth
5. **Rate limit** login attempts (5 per 15 minutes)
6. **Validate input** to prevent injection attacks

## Practice Exercise

Build a complete auth system for a blog platform:
- Email/password registration and login
- Google OAuth integration
- Protected routes for creating/editing posts
- Admin-only routes for user management
- Password reset flow

## Key Takeaways

- JWT is stateless and scalable for API authentication
- OAuth simplifies login by leveraging existing providers
- Access/refresh token pattern balances security and UX
- AI can generate complete auth systems from requirements
