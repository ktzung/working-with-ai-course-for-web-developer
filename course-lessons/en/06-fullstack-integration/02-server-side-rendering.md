# Server-Side Rendering with AI

## Learning Objectives
- Understand SSR vs CSR trade-offs
- Implement SSR with Next.js and Nuxt.js
- Use AI to generate SSR components

## Why SSR Matters

Client-Side Rendering (CSR) sends an empty HTML file and JavaScript to render the page. Server-Side Rendering (SSR) sends fully rendered HTML from the server.

**Benefits of SSR**:
- **SEO**: Search engines can crawl your content
- **Performance**: Users see content faster (First Contentful Paint)
- **Social sharing**: Preview images and descriptions work correctly

**When to use SSR**:
- Content-heavy sites (blogs, news, e-commerce)
- Pages that need SEO
- Applications where initial load time matters

**When CSR is fine**:
- Dashboards and admin panels
- Apps behind authentication
- Highly interactive applications

## Next.js SSR

Next.js is the most popular React SSR framework:

```javascript
// pages/products/[id].js
export async function getServerSideProps(context) {
  const { id } = context.params;

  try {
    const product = await fetch(`${process.env.API_URL}/products/${id}`);
    const data = await product.json();

    if (!data.success) {
      return { notFound: true };
    }

    return {
      props: {
        product: data.data
      }
    };
  } catch (error) {
    return { notFound: true };
  }
}

export default function ProductPage({ product }) {
  return (
    <div>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <span>${product.price}</span>
    </div>
  );
}
```

## Next.js App Router (React Server Components)

The newer App Router uses React Server Components:

```javascript
// app/products/[id]/page.js
async function getProduct(id) {
  const res = await fetch(`${process.env.API_URL}/products/${id}`, {
    cache: 'no-store' // or 'force-cache' for static
  });
  return res.json();
}

export default async function ProductPage({ params }) {
  const { data: product } = await getProduct(params.id);

  return (
    <div>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <AddToCartButton productId={product.id} />
    </div>
  );
}
```

## Nuxt.js SSR (Vue)

Nuxt.js provides SSR for Vue applications:

```vue
<!-- pages/products/[id].vue -->
<script setup>
const route = useRoute();
const { data: product } = await useFetch(`/api/products/${route.params.id}`);

if (!product.value) {
  throw createError({
    statusCode: 404,
    message: 'Product not found'
  });
}
</script>

<template>
  <div>
    <h1>{{ product.name }}</h1>
    <p>{{ product.description }}</p>
    <span>${{ product.price }}</span>
  </div>
</template>
```

## Static Site Generation (SSG)

For content that doesn't change often, SSG pre-renders at build time:

```javascript
// Next.js SSG
export async function getStaticProps() {
  const products = await fetch(`${process.env.API_URL}/products`);
  const data = await products.json();

  return {
    props: { products: data.data },
    revalidate: 60 // ISR: regenerate every 60 seconds
  };
}

export async function getStaticPaths() {
  const products = await fetch(`${process.env.API_URL}/products`);
  const data = await products.json();

  return {
    paths: data.data.map(p => ({ params: { id: p.id } })),
    fallback: 'blocking'
  };
}
```

## AI Prompt for SSR

```
Create a Next.js product page with:
1. Server-side rendering for SEO
2. Dynamic route for product details
3. Loading states and error boundaries
4. Image optimization with next/image
5. Meta tags for social sharing
6. Incremental static regeneration for performance

Include both App Router and Pages Router examples.
```

## Data Fetching Patterns

```javascript
// Server Component (App Router)
async function ProductList() {
  const products = await fetch('https://api.example.com/products', {
    next: { revalidate: 3600 } // Cache for 1 hour
  }).then(r => r.json());

  return (
    <div>
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

// Client Component for interactivity
'use client';
function AddToCartButton({ productId }) {
  const [loading, setLoading] = useState(false);

  const handleAdd = async () => {
    setLoading(true);
    await api.post('/cart', { productId });
    setLoading(false);
  };

  return (
    <button onClick={handleAdd} disabled={loading}>
      {loading ? 'Adding...' : 'Add to Cart'}
    </button>
  );
}
```

## Practice Exercise

Convert a CSR React app to SSR with Next.js:
- Product listing page with SSR
- Product detail page with dynamic routes
- Blog posts with SSG and ISR
- Proper meta tags and SEO optimization
- Loading and error states

## Key Takeaways

- SSR improves SEO and initial load performance
- Next.js and Nuxt.js are the leading SSR frameworks
- SSG is ideal for content that rarely changes
- React Server Components simplify data fetching
