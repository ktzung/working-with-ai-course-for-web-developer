# Tech Stack Guide — Web Development

> Compare and choose the right technologies for your web project. Use this guide when asking AI for architecture recommendations.

## Frontend Frameworks

### React vs Vue vs Svelte

| Criteria | React | Vue | Svelte |
|----------|-------|-----|--------|
| **Learning Curve** | Medium | Easy | Easy |
| **Ecosystem** | Largest | Large | Growing |
| **Performance** | Good (virtual DOM) | Good (virtual DOM) | Excellent (compile-time) |
| **TypeScript** | Excellent | Good | Good |
| **Job Market** | Highest demand | Growing | Niche |
| **AI Support** | Best (Copilot, ChatGPT) | Good | Moderate |
| **Best For** | Large apps, enterprise | Small-medium apps | Performance-critical |

**Recommendation for this course:** React with Next.js — best AI tool support, largest ecosystem, strong TypeScript integration.

### Next.js vs Remix vs Vite

| Criteria | Next.js | Remix | Vite |
|----------|---------|-------|------|
| **SSR/SSG** | Both | SSR-focused | SSG/SPA |
| **API Routes** | Built-in | Built-in | External |
| **Deployment** | Vercel-optimized | Anywhere | Anywhere |
| **Data Fetching** | Server Components | Loaders | Client-side |
| **Maturity** | Most mature | Growing | Very mature |

**Recommendation:** Next.js 14 with App Router — best balance of features, deployment, and AI assistance.

## Backend

### Node.js vs Python vs Go

| Criteria | Node.js (Express/Fastify) | Python (FastAPI/Django) | Go |
|----------|--------------------------|------------------------|-----|
| **Performance** | Good | Moderate | Excellent |
| **TypeScript** | Native | Type hints | N/A |
| **Async** | Excellent | Good | Excellent |
| **AI Integration** | Best (same language) | Best (ML ecosystem) | Limited |
| **Learning Curve** | Easy (if JS known) | Easy | Steep |
| **Best For** | Full-stack JS | ML-heavy backends | High-performance APIs |

**Recommendation:** Node.js with Next.js API routes — unified language, excellent AI support, rapid development.

## Databases

### SQL vs NoSQL

| Criteria | PostgreSQL (SQL) | MongoDB (NoSQL) | Supabase |
|----------|-----------------|-----------------|----------|
| **Data Model** | Relational | Document | Relational + Realtime |
| **ACID** | Full | Limited | Full |
| **Joins** | Native | Manual | Native |
| **Scaling** | Vertical + Read replicas | Horizontal | Vertical |
| **TypeScript** | Prisma/Drizzle | Mongoose | Prisma |
| **Best For** | Structured data | Flexible schemas | Rapid prototyping |

**Recommendation:** PostgreSQL with Prisma ORM — strong typing, migrations, excellent AI code generation.

### ORM Comparison

| ORM | TypeScript | Migrations | Query Builder | AI Support |
|-----|-----------|------------|---------------|------------|
| **Prisma** | Excellent | Built-in | Type-safe | Best |
| **Drizzle** | Excellent | Built-in | SQL-like | Good |
| **TypeORM** | Good | Built-in | Active Record | Moderate |

**Recommendation:** Prisma — best TypeScript integration, most AI training data.

## Authentication

| Solution | Complexity | Features | Cost |
|----------|-----------|----------|------|
| **NextAuth.js** | Low | OAuth, email, JWT | Free |
| **Clerk** | Very low | Full UI, MFA | Free tier |
| **Supabase Auth** | Low | OAuth, magic link | Free tier |
| **Auth0** | Medium | Enterprise features | Free tier |

**Recommendation:** NextAuth.js v5 — free, flexible, well-integrated with Next.js.

## Styling

| Approach | Pros | Cons | AI Support |
|----------|------|------|------------|
| **Tailwind CSS** | Fast, consistent, small bundle | Class-heavy HTML | Excellent |
| **CSS Modules** | Scoped, standard CSS | More boilerplate | Good |
| **Styled Components** | Dynamic, CSS-in-JS | Runtime overhead | Good |
| **shadcn/ui** | Beautiful, accessible, copy-paste | Tailwind dependency | Excellent |

**Recommendation:** Tailwind CSS + shadcn/ui — fastest development, best AI generation, accessible components.

## Deployment

| Platform | Ease | Cost | Features |
|----------|------|------|----------|
| **Vercel** | Easiest | Free tier | Preview deploys, analytics |
| **Netlify** | Easy | Free tier | Forms, functions |
| **Railway** | Easy | $5/mo | Databases, cron |
| **AWS** | Complex | Pay-as-you-go | Full control |

**Recommendation:** Vercel — zero-config Next.js deployment, preview URLs, free tier.

## Decision Matrix for WebDevHub

| Choice | Selected | Why |
|--------|----------|-----|
| Frontend | Next.js 14 + React | Best AI support, SSR, ecosystem |
| Language | TypeScript | Type safety, AI code quality |
| Styling | Tailwind + shadcn/ui | Speed, consistency, accessibility |
| Database | PostgreSQL + Prisma | Relational data, type-safe queries |
| Auth | NextAuth.js v5 | Free, flexible, Next.js integration |
| Testing | Jest + RTL + Playwright | Full coverage stack |
| Deploy | Vercel | Zero-config, preview deploys |
