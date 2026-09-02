# Connecting Frontend and Backend

## Learning Objectives
- Make API calls from frontend to backend
- Handle loading states and errors
- Use AI to generate data fetching code

## The Full-Stack Connection

Your frontend and backend are two separate applications that communicate through HTTP. The frontend sends requests, the backend processes them and returns data. Getting this connection right is essential for a working application.

## Fetch API Basics

The built-in `fetch` API is the simplest way to make HTTP requests:

```javascript
// GET request
const getUsers = async () => {
  try {
    const response = await fetch('/api/users');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Failed to fetch users:', error);
    throw error;
  }
};

// POST request
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

## Axios — A Better Alternative

Axios provides a cleaner API with automatic error handling:

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: '/api',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Request interceptor — add auth token
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor — handle errors globally
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

## React Data Fetching

### With useEffect

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

### With React Query (TanStack Query)

React Query handles caching, loading states, and refetching automatically:

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
          <button onClick={() => deleteUser.mutate(user.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

## AI Prompt for Data Fetching

```
Create a React data fetching layer with:
1. Axios instance with interceptors for auth and error handling
2. Custom hooks for common operations (useUsers, useProducts)
3. React Query integration with caching and refetching
4. Loading, error, and empty states for each data view
5. Optimistic updates for mutations
6. Type-safe API calls with TypeScript

Include examples for GET, POST, PUT, DELETE operations.
```

## Handling CORS

Cross-Origin Resource Sharing (CORS) blocks requests between different domains during development:

```javascript
// Backend: Enable CORS
const cors = require('cors');

app.use(cors({
  origin: process.env.CLIENT_URL || 'http://localhost:3000',
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH']
}));
```

```javascript
// Frontend: Vite proxy (vite.config.js)
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

## Environment Variables

```javascript
// Frontend (.env)
VITE_API_URL=http://localhost:5000/api

// Usage
const API_URL = import.meta.env.VITE_API_URL;
```

## Practice Exercise

Connect a React frontend to your Task Management API:
- Display list of tasks with loading states
- Create new task with form submission
- Update task status with optimistic updates
- Delete task with confirmation
- Handle authentication and protected routes

## Key Takeaways

- Frontend communicates with backend through HTTP requests
- Axios provides cleaner API and automatic error handling
- React Query manages caching, loading, and refetching
- CORS must be configured for cross-origin requests
