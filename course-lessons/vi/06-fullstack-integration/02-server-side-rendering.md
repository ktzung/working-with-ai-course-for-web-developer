# Render phía Server với AI

## Mục tiêu học tập
- Hiểu sự đánh đổi giữa SSR và CSR
- Triển khai SSR với Next.js và Nuxt.js
- Sử dụng AI để tạo thành phần SSR

## Tại sao SSR quan trọng?

Render phía Client (CSR) gửi tệp HTML trống và JavaScript để render trang. Render phía Server (SSR) gửi HTML đã render đầy đủ từ server.

**Lợi ích của SSR**:
- **SEO**: Công cụ tìm kiếm có thể thu thập nội dung
- **Hiệu suất**: Người dùng thấy nội dung nhanh hơn (First Contentful Paint)
- **Chia sẻ mạng xã hội**: Xem trước hình ảnh và mô tả hoạt động đúng

**Khi nào sử dụng SSR**:
- Trang web nhiều nội dung (blog, tin tức, thương mại điện tử)
- Trang cần SEO
- Ứng dụng mà thời gian tải ban đầu quan trọng

**Khi nào CSR được**:
- Bảng điều khiển và trang quản trị
- Ứng dụng sau xác thực
- Ứng dụng tương tác cao

## Next.js SSR

Next.js là框架 SSR React phổ biến nhất:

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

App Router mới hơn sử dụng React Server Components:

```javascript
// app/products/[id]/page.js
async function getProduct(id) {
  const res = await fetch(`${process.env.API_URL}/products/${id}`, {
    cache: 'no-store' // hoặc 'force-cache' cho tĩnh
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

Nuxt.js cung cấp SSR cho ứng dụng Vue:

```vue
<!-- pages/products/[id].vue -->
<script setup>
const route = useRoute();
const { data: product } = await useFetch(`/api/products/${route.params.id}`);

if (!product.value) {
  throw createError({
    statusCode: 404,
    message: 'Không tìm thấy sản phẩm'
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

## Tạo trang tĩnh (SSG)

Với nội dung không thay đổi thường xuyên, SSG render trước tại thời điểm构建:

```javascript
// Next.js SSG
export async function getStaticProps() {
  const products = await fetch(`${process.env.API_URL}/products`);
  const data = await products.json();

  return {
    props: { products: data.data },
    revalidate: 60 // ISR: tạo lại mỗi 60 giây
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

## Prompt AI cho SSR

```
Tạo trang sản phẩm Next.js với:
1. Render phía server cho SEO
2. Route động cho chi tiết sản phẩm
3. Trạng thái tải và ranh giới lỗi
4. Tối ưu hóa hình ảnh với next/image
5. Thẻ meta cho chia sẻ mạng xã hội
6. Tạo lại tĩnh tăng cường cho hiệu suất

Bao gồm cả ví dụ App Router và Pages Router.
```

## Mẫu lấy dữ liệu

```javascript
// Server Component (App Router)
async function ProductList() {
  const products = await fetch('https://api.example.com/products', {
    next: { revalidate: 3600 } // Đệm 1 giờ
  }).then(r => r.json());

  return (
    <div>
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

// Client Component cho tương tác
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
      {loading ? 'Đang thêm...' : 'Thêm vào giỏ'}
    </button>
  );
}
```

## Bài tập thực hành

Chuyển đổi ứng dụng React CSR sang SSR với Next.js:
- Trang danh sách sản phẩm với SSR
- Trang chi tiết sản phẩm với route động
- Bài viết blog với SSG và ISR
- Thẻ meta và tối ưu hóa SEO đúng cách
- Trạng thái tải và lỗi

## Điểm chính

- SSR cải thiện SEO và hiệu suất tải ban đầu
- Next.js và Nuxt.js là框架 SSR hàng đầu
- SSG lý tưởng cho nội dung hiếm khi thay đổi
- React Server Components đơn giản hóa lấy dữ liệu
