# Deploy Frontend with AI

## Learning Objectives
- Deploy React/Next.js to Vercel
- Deploy static sites to Netlify
- Use AI to configure deployments

## Why Deployment Matters

Building an app is only half the battle. Getting it live where users can access it is the other half. Modern platforms make frontend deployment incredibly easy.

## Vercel (Recommended for Next.js)

Vercel is made by the creators of Next.js. It's the best platform for Next.js apps.

### Deploy with Git

1. Push your code to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Click "New Project"
4. Import your repository
5. Vercel auto-detects Next.js and configures everything
6. Click "Deploy"

### Vercel Configuration

```json
// vercel.json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "env": {
    "NEXT_PUBLIC_API_URL": "@api-url"
  },
  "rewrites": [
    { "source": "/api/:path*", "destination": "https://api.example.com/:path*" }
  ]
}
```

### Environment Variables

```bash
# Set via Vercel CLI
vercel env add NEXT_PUBLIC_API_URL

# Or in Vercel Dashboard:
# Settings > Environment Variables
```

### Custom Domain

1. Go to Project Settings > Domains
2. Add your domain
3. Configure DNS records as shown
4. SSL is automatic

## Netlify (Great for Static Sites)

Netlify excels at static site hosting with great DX.

### Deploy with Git

1. Push code to GitHub
2. Go to [netlify.com](https://netlify.com)
3. Click "New site from Git"
4. Select repository
5. Configure build settings:
   - Build command: `npm run build`
   - Publish directory: `dist` or `build`
6. Click "Deploy site"

### Netlify Configuration

```toml
# netlify.toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"
```

### Netlify Functions

Serverless functions for API endpoints:

```javascript
// netlify/functions/hello.js
exports.handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: 'Hello from Netlify Functions!' })
  };
};
```

## AI Prompt for Frontend Deployment

```
Configure deployment for a React/Next.js application with:
1. Vercel configuration with environment variables
2. Custom domain setup with SSL
3. Preview deployments for pull requests
4. Build optimization and caching
5. Redirect rules for SPA routing
6. Environment-specific configurations
7. Performance monitoring setup

Include vercel.json and netlify.toml configurations.
```

## Build Optimization

```javascript
// next.config.js
module.exports = {
  // Enable static exports where possible
  output: 'standalone',

  // Optimize images
  images: {
    domains: ['cdn.example.com'],
    formats: ['image/avif', 'image/webp']
  },

  // Enable compression
  compress: true,

  // Bundle analyzer
  webpack: (config) => {
    if (process.env.ANALYZE === 'true') {
      const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
      config.plugins.push(new BundleAnalyzerPlugin());
    }
    return config;
  }
};
```

## Preview Deployments

Every pull request gets a unique URL:

```yaml
# GitHub Actions for preview
name: Preview Deployment
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

## Practice Exercise

Deploy your Task Management frontend:
- Set up Vercel or Netlify account
- Configure environment variables
- Set up custom domain
- Enable preview deployments
- Test the deployed application

## Key Takeaways

- Vercel is ideal for Next.js; Netlify excels at static sites
- Git-based deployment is automatic and reliable
- Environment variables keep secrets secure
- Preview deployments enable testing before production
