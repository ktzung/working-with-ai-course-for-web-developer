# Kết nối Frontend và Backend

## Mục tiêu học tập
- Gọi API từ frontend đến backend
- Xử lý trạng thái tải và lỗi
- Sử dụng AI để tạo mã lấy dữ liệu

## Kết nối Full-Stack

Frontend và backend là hai ứng dụng riêng biệt giao tiếp qua HTTP. Frontend gửi yêu cầu, backend xử lý và trả về dữ liệu. Kết nối đúng là điều cần thiết cho ứng dụng hoạt động.

## Fetch API cơ bản

API `fetch` tích hợp sẵn là cách đơn giản nhất để gửi yêu cầu HTTP:

```javascript
// Yêu cầu GET
const getUsers = async () => {
  try {
    const response = await fetch('/api/users');
    if (!response.ok) {
      throw new Error(`Lỗi HTTP! trạng thái: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Không thể lấy người dùng:', error);
    throw error;
  }
};

// Yêu cầu POST
const createUser = async (userData) => {
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    },
    body: JSON.stringify(userData)
  });
  return response.json();
};
```

## Axios — Lựa chọn tốt hơn

Axios cung cấp API sạch hơn với xử lý lỗi tự động:

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: '/api',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Interceptor yêu cầu — thêm token xác thực
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Interceptor phản hồi — xử lý lỗi toàn cục
api.interceptors.response.use(
  response => response.data,
  error => {
    if (error.response?.status === 401) {
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

## Lấy dữ liệu React

### Với useEffect

```javascript
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await api.get('/users');
        setUsers(response.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    fetchUsers();
  }, []);

  if (loading) return <Spinner />;
  if (error) return <ErrorMessage message={error} />;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Với React Query (TanStack Query)

React Query xử lý bộ đệm, trạng thái tải và tự động lấy lại:

```javascript
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

function UserList() {
  const queryClient = useQueryClient();

  const { data: users, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: () => api.get('/users')
  });

  const deleteUser = useMutation({
    mutationFn: (id) => api.delete(`/users/${id}`),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    }
  });

  if (isLoading) return <Spinner />;

  return (
    <ul>
      {users?.map(user => (
        <li key={user.id}>
          {user.name}
          <button onClick={() => deleteUser.mutate(user.id)}>Xóa</button>
        </li>
      ))}
    </ul>
  );
}
```

## Prompt AI cho lấy dữ liệu

```
Tạo lớp lấy dữ liệu React với:
1. Phiên bản Axios với interceptor cho xác thực và xử lý lỗi
2. Hook tùy chỉnh cho các thao tác phổ biến (useUsers, useProducts)
3. Tích hợp React Query với bộ đệm và lấy lại
4. Trạng thái tải, lỗi và trống cho mỗi chế độ xem dữ liệu
5. Cập nhật lạc quan cho các mutation
6. Gọi API an toàn kiểu với TypeScript

Bao gồm ví dụ cho các thao tác GET, POST, PUT, DELETE.
```

## Xử lý CORS

Chia sẻ tài nguyênCross-Origin (CORS) chặn yêu cầu giữa các域名 khác nhau trong quá trình phát triển:

```javascript
// Backend: Bật CORS
const cors = require('cors');

app.use(cors({
  origin: process.env.CLIENT_URL || 'http://localhost:3000',
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH']
}));
```

```javascript
// Frontend: Proxy Vite (vite.config.js)
export default {
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:5000',
        changeOrigin: true
      }
    }
  }
};
```

## Biến môi trường

```javascript
// Frontend (.env)
VITE_API_URL=http://localhost:5000/api

// Sử dụng
const API_URL = import.meta.env.VITE_API_URL;
```

## Bài tập thực hành

Kết nối frontend React với API quản lý nhiệm vụ:
- Hiển thị danh sách nhiệm vụ với trạng thái tải
- Tạo nhiệm vụ mới với gửi biểu mẫu
- Cập nhật trạng thái nhiệm vụ với cập nhật lạc quan
- Xóa nhiệm vụ với xác nhận
- Xử lý xác thực và route được bảo vệ

## Điểm chính

- Frontend giao tiếp với backend qua yêu cầu HTTP
- Axios cung cấp API sạch hơn và xử lý lỗi tự động
- React Query quản lý bộ đệm, tải và lấy lại
- CORS phải được cấu hình cho yêu cầuCross-Origin
