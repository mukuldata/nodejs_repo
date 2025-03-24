# Socket.io

### **Table of Contents**

1. **Introduction to Real-time Communication**
2. **What is Socket.io?**
3. **How Does Socket.io Work?**
4. **Setting Up Socket.io in a Node.js App**
5. **Basic Example of Socket.io (Chat App)**
6. **Handling Events in Socket.io**
7. **Broadcasting Messages to Multiple Clients**
8. **Rooms and Namespaces in Socket.io**
9. **Handling Disconnections and Errors**
10. **Scaling Socket.io with Redis Adapter**
11. **Security Considerations**
12. **Conclusion**

***

### **1. Introduction to Real-time Communication**

Real-time applications allow users to **send and receive data instantly** without refreshing the page. Examples include:\
✅ **Chat applications** (WhatsApp, Messenger)\
✅ **Live notifications** (Facebook, Twitter updates)\
✅ **Online multiplayer games**\
✅ **Real-time dashboards** (Stock market, analytics)

Traditional HTTP requests (`GET`, `POST`) **do not support** continuous, two-way communication, so **WebSockets** are used for real-time interaction.

***

### **2. What is Socket.io?**

**Socket.io** is a JavaScript library that enables **real-time, bidirectional communication** between the server and clients using WebSockets.

✅ **Why use Socket.io?**\
✔ Works on top of **WebSockets**\
✔ Supports **automatic reconnections**\
✔ Can fall back to **HTTP polling** if WebSockets are not supported\
✔ Easy to implement with **Node.js and JavaScript**

***

### **3. How Does Socket.io Work?**

Socket.io works in **two parts**:

1. **Server-side (Node.js)** – Listens for incoming connections and handles communication.
2. **Client-side (Browser/Frontend)** – Sends and receives real-time messages.

**How the connection works:**

1. The client sends a **WebSocket handshake request** to the server.
2. If WebSockets are supported, a **persistent connection** is established.
3. The client and server **exchange events and data** in real-time.

***

### **4. Setting Up Socket.io in a Node.js App**

#### **Step 1: Install Dependencies**

Run the following command to install Socket.io:

```sh
npm install express socket.io
```

#### **Step 2: Create a Basic Server**

📄 **server.js**

```javascript
const express = require("express");
const http = require("http");
const socketIo = require("socket.io");

const app = express();
const server = http.createServer(app);
const io = socketIo(server); // Attach Socket.io to the server

io.on("connection", (socket) => {
    console.log("A user connected");

    socket.on("disconnect", () => {
        console.log("A user disconnected");
    });
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
});
```

✅ **What this does:**\
✔ Starts an **Express server** on port `3000`.\
✔ Listens for **client connections** using `socket.io`.\
✔ Logs when a user connects or disconnects.

***

### **5. Basic Example of Socket.io (Chat App)**

#### **Client-Side Code (HTML + JavaScript)**

📄 **index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Chat App</title>
    <script src="https://cdn.socket.io/4.0.0/socket.io.min.js"></script>
</head>
<body>
    <h1>Chat App</h1>
    <input id="messageInput" type="text" placeholder="Type a message">
    <button onclick="sendMessage()">Send</button>
    <ul id="messages"></ul>

    <script>
        const socket = io("http://localhost:3000");

        function sendMessage() {
            const message = document.getElementById("messageInput").value;
            socket.emit("chatMessage", message);
        }

        socket.on("chatMessage", (msg) => {
            const li = document.createElement("li");
            li.textContent = msg;
            document.getElementById("messages").appendChild(li);
        });
    </script>
</body>
</html>
```

#### **Server-Side Code**

📄 **server.js**

```javascript
const express = require("express");
const http = require("http");
const socketIo = require("socket.io");

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

io.on("connection", (socket) => {
    console.log("A user connected");

    socket.on("chatMessage", (msg) => {
        io.emit("chatMessage", msg); // Broadcast message to all users
    });

    socket.on("disconnect", () => {
        console.log("A user disconnected");
    });
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
});
```

✅ **What this does:**\
✔ Allows users to send messages.\
✔ Broadcasts messages **to all connected users**.

***

### **6. Handling Events in Socket.io**

Socket.io uses **custom events** to send and receive messages.

#### **Example: Sending a Notification**

📄 **server.js**

```javascript
io.on("connection", (socket) => {
    socket.emit("welcome", "Welcome to the server!");

    socket.on("userAction", (data) => {
        console.log(`User performed: ${data}`);
    });
});
```

📄 **Client (JavaScript)**

```javascript
const socket = io("http://localhost:3000");

socket.on("welcome", (message) => {
    console.log(message);
});

socket.emit("userAction", "Clicked a button");
```

✅ **What this does:**\
✔ Server sends a **welcome message** when the user connects.\
✔ Client sends an event `"userAction"` to the server.

***

### **7. Broadcasting Messages to Multiple Clients**

📄 **server.js**

```javascript
socket.on("chatMessage", (msg) => {
    io.emit("chatMessage", msg); // Send to all connected users
});
```

✅ **What this does:**\
✔ Sends messages **to all users** in real time.

***

### **8. Rooms and Namespaces in Socket.io**

#### **Rooms (Group Chat Example)**

📄 **server.js**

```javascript
socket.join("room1");

socket.on("chatMessage", (msg) => {
    io.to("room1").emit("chatMessage", msg);
});
```

✅ **What this does:**\
✔ Users in `"room1"` receive messages meant for that room.

***

### **9. Handling Disconnections and Errors**

📄 **server.js**

```javascript
socket.on("disconnect", () => {
    console.log("User disconnected");
});

socket.on("error", (err) => {
    console.error("Socket Error:", err);
});
```

✅ **What this does:**\
✔ Logs user **disconnections**.\
✔ Handles **errors** gracefully.

***

### **10. Scaling Socket.io with Redis Adapter**

For large applications, **Redis** helps share WebSocket connections across multiple servers.

📄 **server.js**

```javascript
const redisAdapter = require("socket.io-redis");

io.adapter(redisAdapter({ host: "localhost", port: 6379 }));
```

✅ **What this does:**\
✔ Ensures users connected to **different servers** can communicate.

***

### **11. Security Considerations**

🔒 **Best practices for secure WebSockets:**\
✔ **Enable authentication** before connecting.\
✔ **Use HTTPS and WSS** (secure WebSockets).\
✔ **Rate-limit events** to prevent spam attacks.\
✔ **Sanitize user input** to avoid injections.

***

### **12. Conclusion**

✅ **Socket.io** enables real-time two-way communication.\
✅ Works with **WebSockets**, supports **fallback to HTTP polling**.\
✅ Used in **chat apps, notifications, multiplayer games**.\
✅ Easy to **scale** using Redis Adapter.
