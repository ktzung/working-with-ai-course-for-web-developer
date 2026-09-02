# Tính năng thời gian thực với AI

## Mục tiêu học tập
- Triển khai kết nối WebSocket
- Xây dựng tính năng thời gian thực với Socket.io
- Sử dụng AI để tạo mã thời gian thực

## Tại sao thời gian thực quan trọng?

Một số tính năng cần cập nhật tức thì — tin nhắn trò chuyện, thông báo, bảng điều khiển trực tiếp, chỉnh sửa cộng tác. Yêu cầu HTTP quá chậm cho những thứ này; WebSocket cung cấp giao tiếp liên tục, hai chiều.

## WebSocket cơ bản

WebSocket tạo kết nối liên tục giữa client và server:

```javascript
// Server (Node.js với thư viện ws)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  console.log('Client đã kết nối');

  ws.on('message', (message) => {
    console.log('Nhận được:', message);
    // Phát sóng cho tất cả client
    wss.clients.forEach(client => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });

  ws.on('close', () => {
    console.log('Client đã ngắt kết nối');
  });
});
```

```javascript
// Client
const ws = new WebSocket('ws://localhost:8080');

ws.onopen = () => {
  ws.send('Xin chào Server!');
};

ws.onmessage = (event) => {
  console.log('Tin nhắn từ server:', event.data);
};
```

## Socket.io — WebSocket trở nên dễ dàng

Socket.io thêm kết nối lại, phòng và dự phòng:

```javascript
// Server
const io = require('socket.io')(server, {
  cors: {
    origin: process.env.CLIENT_URL,
    methods: ['GET', 'POST']
  }
});

io.on('connection', (socket) => {
  console.log('Người dùng đã kết nối:', socket.id);

  // Tham gia phòng
  socket.on('join-room', (roomId) => {
    socket.join(roomId);
    socket.to(roomId).emit('user-joined', socket.id);
  });

  // Xử lý tin nhắn trò chuyện
  socket.on('send-message', (data) => {
    io.to(data.roomId).emit('receive-message', {
      id: Date.now(),
      text: data.text,
      sender: socket.id,
      timestamp: new Date()
    });
  });

  // Xử lý chỉ báo nhập
  socket.on('typing', (data) => {
    socket.to(data.roomId).emit('user-typing', socket.id);
  });

  socket.on('disconnect', () => {
    console.log('Người dùng đã ngắt kết nối:', socket.id);
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
      {typing && <span>Ai đó đang nhập...</span>}
      <MessageInput onSend={sendMessage} />
    </div>
  );
}
```

## Thông báo thời gian thực

```javascript
// Server: Gửi thông báo cho người dùng cụ thể
const sendNotification = (userId, notification) => {
  io.to(`user-${userId}`).emit('notification', {
    id: Date.now(),
    type: notification.type,
    message: notification.message,
    read: false,
    createdAt: new Date()
  });
};

// Client: Lắng nghe thông báo
useEffect(() => {
  socket.emit('register-user', userId);

  socket.on('notification', (notification) => {
    setNotifications(prev => [notification, ...prev]);
    toast(notification.message);
  });

  return () => socket.off('notification');
}, [userId]);
```

## Cập nhật bảng điều khiển trực tiếp

```javascript
// Server: Phát sóng số liệu mỗi 5 giây
setInterval(() => {
  const metrics = {
    activeUsers: getActiveUserCount(),
    requestsPerSecond: getRequestRate(),
    errorRate: getErrorRate()
  };
  io.emit('metrics-update', metrics);
}, 5000);

// Client: Biểu đồ thời gian thực
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

## Prompt AI cho tính năng thời gian thực

```
Tạo ứng dụng trò chuyện thời gian thực với Socket.io:
1. Server với phòng, chỉ báo nhập và lịch sử tin nhắn
2. Client React với danh sách tin nhắn, đầu vào và trạng thái nhập
3. Xác thực người dùng cho kết nối socket
4. Lưu trữ tin nhắn với MongoDB
5. Theo dõi trạng thái trực tuyến/ngoại tuyến
6. Xác nhận已读
7. Chia sẻ tệp trong trò chuyện

Bao gồm xử lý lỗi và逻辑 kết nối lại.
```

## Bài tập thực hành

Thêm tính năng thời gian thực vào ứng dụng quản lý nhiệm vụ:
- Cập nhật trạng thái nhiệm vụ trực tiếp trên tất cả client đã kết nối
- Thông báo khi nhiệm vụ được giao
- Nguồn cấp dữ liệu hoạt động显示 thay đổi gần đây
- Chỉ báo présence trực tuyến cho thành viên nhóm

## Điểm chính

- WebSocket cho phép giao tiếp liên tục, hai chiều
- Socket.io đơn giản hóa triển khai WebSocket với phòng và kết nối lại
- Tính năng thời gian thực nâng cao trải nghiệm người dùng cho ứng dụng cộng tác
- AI có thể tạo triển khai thời gian thực hoàn chỉnh
