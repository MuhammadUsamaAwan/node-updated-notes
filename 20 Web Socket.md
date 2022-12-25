```
npm i socket.io
```

```prisma
// schema.prisma

model User {
  id String @id
  username String
  room String
}
```

```ts
// server.ts

import express from 'express';
const app = express();
import http from 'http';
const server = http.createServer(app);
import { Server } from 'socket.io';
const io = new Server(server);
import { PrismaClient } from '@prisma/client';
const PORT = process.env.PORT || 5000;

const prisma = new PrismaClient();
app.use(express.static('public'));

io.on('connection', socket => {
  // join room
  socket.on('join-room', async (username, room) => {
    await prisma.user.create({
      data: {
        id: socket.id,
        username,
        room,
      },
    });
    const roomUsers = await prisma.user.findMany({
      where: {
        room,
      },
    });
    socket.join(room);
    io.to(room).emit('user-joined', username, new Date());
    io.to(room).emit('room-users', roomUsers);
  });

  // disconnect
  socket.on('disconnect', async () => {
    const user = await prisma.user.delete({
      where: {
        id: socket.id,
      },
    });
    const roomUsers = await prisma.user.findMany({
      where: {
        room: user.room,
      },
    });
    if (user) {
      io.to(user.room).emit('user-left', user.username, new Date());
      io.to(user.room).emit('room-users', roomUsers);
      socket.broadcast.to(user.room).emit('typing-message-remove');
    }
  });

  // new messge
  socket.on('new-message', (message, username, room) => {
    io.to(room).emit('receive-message', username, new Date(), message);
  });

  // user is typing
  socket.on('user-typing', (username, room) => {
    socket.broadcast.to(room).emit('typing-message', username);
  });

  // user typing false
  socket.on('user-typing-false', room => {
    socket.broadcast.to(room).emit('typing-message-remove');
  });
});

server.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```
