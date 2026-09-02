# Thực hành: Xây dựng tính năng Full-Stack

## Mục tiêu học tập
- Áp dụng tất cả khái niệm tích hợp full-stack
- Xây dựng tính năng hoàn chỉnh端到端
- Thực hành kết nối frontend React với backend Express

## Dự án: Bảng điều khiển nhóm

Xây dựng bảng điều khiển nhóm thời gian thực显示 số liệu dự án, trạng thái nhiệm vụ và hoạt động nhóm.

## Yêu cầu

**Frontend (React + Next.js)**:
- Bảng điều khiển với số liệu thời gian thực
- Bảng nhiệm vụ với kéo thả
- Danh sách thành viên nhóm với trạng thái trực tuyến
- Nguồn cấp dữ liệu hoạt động với cập nhật trực tiếp
- Thiết kếResponsive

**Backend (Express + MongoDB)**:
- REST API cho nhiệm vụ và dự án
- WebSocket cho cập nhật thời gian thực
- Bộ đệm Redis cho số liệu
- Xác thực JWT

**Tích hợp**:
- Gọi API với React Query
- Cập nhật thời gian thực với Socket.io
- Xử lý lỗi và trạng thái tải
- Cập nhật lạc quan

## Bước 1: Thiết lập dự án

```bash
# Backend
mkdir dashboard-api && cd dashboard-api
npm init -y
npm install express mongoose socket.io redis cors helmet

# Frontend
npx create-next-app@latest dashboard-ui
cd dashboard-ui
npm install @tanstack/react-query socket.io-client axios
```

## Bước 2: Xây dựng API

Sử dụng AI để tạo API hoàn chỉnh:

```
Tạo API Express.js cho bảng điều khiển nhóm với:
1. CRUD dự án với quản lý thành viên
2. CRUD nhiệm vụ với trạng thái, ưu tiên, người được giao
3. Ghi log hoạt động cho tất cả thay đổi
4. Endpoint số liệu (số lượng nhiệm vụ, tỷ lệ hoàn thành)
5. Sự kiện WebSocket cho cập nhật thời gian thực
6. Bộ đệm Redis cho số liệu
7. Xác thực JWT

Bao gồm model, controller, route và trình xử lý WebSocket.
```

## Bước 3: Xây dựng Frontend

```javascript
// app/dashboard/page.js
'use client';
import { useQuery } from '@tanstack/react-query';
import { useSocket } from '@/hooks/useSocket';
import MetricsCards from '@/components/MetricsCards';
import TaskBoard from '@/components/TaskBoard';
import ActivityFeed from '@/components/ActivityFeed';

export default function Dashboard() {
  const { data: metrics, isLoading } = useQuery({
    queryKey: ['metrics'],
    queryFn: () => api.get('/metrics'),
    refetchInterval: 30000 // Lấy lại mỗi 30 giây
  });

  const { data: tasks } = useQuery({
    queryKey: ['tasks'],
    queryFn: () => api.get('/tasks')
  });

  // Cập nhật thời gian thực
  useSocket('task-updated', (task) => {
    queryClient.invalidateQueries({ queryKey: ['tasks'] });
  });

  useSocket('metrics-update', (newMetrics) => {
    queryClient.setQueryData(['metrics'], newMetrics);
  });

  return (
    <div className="grid grid-cols-12 gap-6">
      <div className="col-span-12">
        <MetricsCards metrics={metrics} loading={isLoading} />
      </div>
      <div className="col-span-8">
        <TaskBoard tasks={tasks} />
      </div>
      <div className="col-span-4">
        <ActivityFeed />
      </div>
    </div>
  );
}
```

## Bước 4: Thêm tính năng thời gian thực

```javascript
// hooks/useSocket.js
import { useEffect, useRef } from 'react';
import { io } from 'socket.io-client';

export function useSocket(event, handler) {
  const socketRef = useRef();

  useEffect(() => {
    socketRef.current = io(process.env.NEXT_PUBLIC_API_URL);

    socketRef.current.on(event, handler);

    return () => {
      socketRef.current.off(event, handler);
    };
  }, [event, handler]);
}
```

## Bước 5: Thêm bộ đệm

```javascript
// Backend: Bộ đệm số liệu
const cacheMiddleware = (key, ttl) => async (req, res, next) => {
  const cached = await redis.get(key);
  if (cached) return res.json(JSON.parse(cached));

  const originalJson = res.json;
  res.json = (data) => {
    redis.set(key, JSON.stringify(data), 'EX', ttl);
    originalJson.call(res, data);
  };
  next();
};

router.get('/metrics', cacheMiddleware('metrics', 60), metricsController.get);
```

## Bước 6: Kiểm thử

```javascript
// Kiểm thử E2E
test('bảng điều khiển显示 cập nhật thời gian thực', async ({ page }) => {
  await page.goto('/dashboard');

  // Tạo nhiệm vụ qua API
  await api.post('/tasks', { title: 'Nhiệm vụ mới', project: projectId });

  // Xác minh nó xuất hiện trên bảng điều khiển
  await expect(page.locator('text=Nhiệm vụ mới')).toBeVisible();
});
```

## Sản phẩm bàn giao

1. ✅ Bảng điều khiển Next.js với số liệu thời gian thực
2. ✅ API Express với thao tác CRUD
3. ✅ Socket.io cho cập nhật trực tiếp
4. ✅ Bộ đệm Redis cho hiệu suất
5. ✅ React Query cho quản lý dữ liệu
6. ✅ Thiết kếResponsive
7. ✅ Bộ kiểm thử

## Điểm chính

- Tính năng full-stack kết hợp frontend, backend và giao tiếp thời gian thực
- React Query quản lý trạng thái server với bộ đệm và lấy lại
- Socket.io cho phép cập nhật tức thì trên các client đã kết nối
- Bộ đệm ở nhiều cấp độ cải thiện hiệu suất
