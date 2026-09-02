# Real-Time Features with AI

## Learning Objectives
- Implement WebSocket connections
- Build real-time features with Socket.io
- Use AI to generate real-time code

## Why Real-Time Matters

Some features need instant updates — chat messages, notifications, live dashboards, collaborative editing. HTTP requests are too slow for these; WebSockets provide persistent, bidirectional communication.

## WebSocket Basics

WebSocket creates a persistent connection between client and server:

```javascript
// Server (Node.js with ws library)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  console.log('Client connected');

  ws.on('message', (message) => {
    console.log('Received:', message);
    // Broadcast to all clients
    wss.clients.forEach(client => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });

  ws.on('close', () => {
    console.log('Client disconnected');
  });
});
```

```javascript
// Client
const ws = new WebSocket('ws://localhost:8080');

ws.onopen = () => {
  ws.send('Hello Server!');
};

ws.onmessage = (event) => {
  console.log('Message from server:', event.data);
};
```

## Socket.io — WebSocket Made Easy

Socket.io adds reconnection, rooms, and fallbacks:

```javascript
// Server
const io = require('socket.io')(server, {
  cors: {
    origin: process.env.CLIENT_URL,
    methods: ['GET', 'POST']
  }
});

io.on('connection', (socket) => {
  console.log('User connected:', socket.id);

  // Join a room
  socket.on('join-room', (roomId) => {
    socket.join(roomId);
    socket.to(roomId).emit('user-joined', socket.id);
  });

  // Handle chat messages
  socket.on('send-message', (data) => {
    io.to(data.roomId).emit('receive-message', {
      id: Date.now(),
      text: data.text,
      sender: socket.id,
      timestamp: new Date()
    });
  });

  // Handle typing indicators
  socket.on('typing', (data) => {
    socket.to(data.roomId).emit('user-typing', socket.id);
  });

  socket.on('disconnect', () => {
    console.log('User disconnected:', socket.id);
  });
});
```

```javascript
// Client (React)
import { io } from 'socket.io-client';
import { useEffect, useState, useRef } from 'react';

function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  const [typing, setTyping] = useState(false);
  const socketRef = useRef();

  useEffect(() => {
    socketRef.current = io(process.env.REACT_APP_API_URL);

    socketRef.current.emit('join-room', roomId);

    socketRef.current.on('receive-message', (message) => {
      setMessages(prev => [...prev, message]);
    });

    socketRef.current.on('user-typing', () => {
      setTyping(true);
      setTimeout(() => setTyping(false), 2000);
    });

    return () => socketRef.current.disconnect();
  }, [roomId]);

  const sendMessage = (text) => {
    socketRef.current.emit('send-message', { roomId, text });
  };

  return (
    <div>
      {messages.map(msg => (
        <div key={msg.id}>{msg.text}</div>
      ))}
      {typing && <span>Someone is typing...</span>}
      <MessageInput onSend={sendMessage} />
    </div>
  );
}
```

## Real-Time Notifications

```javascript
// Server: Send notification to specific user
const sendNotification = (userId, notification) => {
  io.to(`user-${userId}`).emit('notification', {
    id: Date.now(),
    type: notification.type,
    message: notification.message,
    read: false,
    createdAt: new Date()
  });
};

// Client: Listen for notifications
useEffect(() => {
  socket.emit('register-user', userId);

  socket.on('notification', (notification) => {
    setNotifications(prev => [notification, ...prev]);
    toast(notification.message);
  });

  return () => socket.off('notification');
}, [userId]);
```

## Live Dashboard Updates

```javascript
// Server: Broadcast metrics every 5 seconds
setInterval(() => {
  const metrics = {
    activeUsers: getActiveUserCount(),
    requestsPerSecond: getRequestRate(),
    errorRate: getErrorRate()
  };
  io.emit('metrics-update', metrics);
}, 5000);

// Client: Real-time chart
function LiveDashboard() {
  const [metrics, setMetrics] = useState([]);

  useEffect(() => {
    socket.on('metrics-update', (data) => {
      setMetrics(prev => [...prev.slice(-50), data]);
    });
  }, []);

  return <LineChart data={metrics} />;
}
```

## AI Prompt for Real-Time Features

```
Create a real-time chat application with Socket.io:
1. Server with rooms, typing indicators, and message history
2. React client with message list, input, and typing status
3. User authentication for socket connections
4. Message persistence with MongoDB
5. Online/offline status tracking
6. Read receipts
7. File sharing in chat

Include error handling and reconnection logic.
```

## Practice Exercise

Add real-time features to your Task Management app:
- Live task status updates across all connected clients
- Notifications when tasks are assigned
- Activity feed showing recent changes
- Online presence indicators for team members

## Key Takeaways

- WebSockets enable persistent, bidirectional communication
- Socket.io simplifies WebSocket implementation with rooms and reconnection
- Real-time features enhance user experience for collaborative apps
- AI can generate complete real-time implementations
