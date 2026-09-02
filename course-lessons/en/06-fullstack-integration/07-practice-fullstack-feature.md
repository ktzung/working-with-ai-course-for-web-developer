# Practice: Build a Full-Stack Feature

## Learning Objectives
- Apply all full-stack integration concepts
- Build a complete feature end-to-end
- Practice connecting React frontend to Express backend

## Project: Team Dashboard

Build a real-time team dashboard that displays project metrics, task status, and team activity.

## Requirements

**Frontend (React + Next.js)**:
- Dashboard with real-time metrics
- Task board with drag-and-drop
- Team member list with online status
- Activity feed with live updates
- Responsive design

**Backend (Express + MongoDB)**:
- REST API for tasks and projects
- WebSocket for real-time updates
- Redis caching for metrics
- JWT authentication

**Integration**:
- API calls with React Query
- Real-time updates with Socket.io
- Error handling and loading states
- Optimistic updates

## Step 1: Set Up the Project

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

## Step 2: Build the API

Use AI to generate the complete API:

```
Create an Express.js API for a team dashboard with:
1. Projects CRUD with member management
2. Tasks CRUD with status, priority, assignee
3. Activity logging for all changes
4. Metrics endpoint (task counts, completion rates)
5. WebSocket events for real-time updates
6. Redis caching for metrics
7. JWT authentication

Include models, controllers, routes, and WebSocket handlers.
```

## Step 3: Build the Frontend

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
    refetchInterval: 30000 // Refetch every 30 seconds
  });

  const { data: tasks } = useQuery({
    queryKey: ['tasks'],
    queryFn: () => api.get('/tasks')
  });

  // Real-time updates
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

## Step 4: Add Real-Time Features

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

## Step 5: Add Caching

```javascript
// Backend: Cache metrics
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

## Step 6: Testing

```javascript
// E2E test
test('dashboard shows real-time updates', async ({ page }) => {
  await page.goto('/dashboard');

  // Create task via API
  await api.post('/tasks', { title: 'New Task', project: projectId });

  // Verify it appears on dashboard
  await expect(page.locator('text=New Task')).toBeVisible();
});
```

## Deliverables

1. ✅ Next.js dashboard with real-time metrics
2. ✅ Express API with CRUD operations
3. ✅ Socket.io for live updates
4. ✅ Redis caching for performance
5. ✅ React Query for data management
6. ✅ Responsive design
7. ✅ Test suite

## Key Takeaways

- Full-stack features combine frontend, backend, and real-time communication
- React Query manages server state with caching and refetching
- Socket.io enables instant updates across connected clients
- Caching at multiple levels improves performance
