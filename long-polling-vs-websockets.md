# Long Polling vs WebSockets

### **Table of Contents**

1. **Introduction to Real-time Communication**
2. **What is Long Polling?**
   * How Long Polling Works
   * Example of Long Polling
3. **What are WebSockets?**
   * How WebSockets Work
   * Example of WebSockets
4. **Comparison: Long Polling vs WebSockets**
5. **When to Use Long Polling vs WebSockets?**
6. **Conclusion**

***

### **1. Introduction to Real-time Communication**

Real-time communication allows **instant updates** between a client (browser) and a server **without refreshing the page**.

📌 Examples of real-time applications:\
✔ **Chat applications** (WhatsApp, Messenger)\
✔ **Live stock market updates**\
✔ **Real-time notifications**\
✔ **Online multiplayer games**

There are two popular ways to achieve real-time updates:

1. **Long Polling** (Older method)
2. **WebSockets** (Modern, efficient method)

***

### **2. What is Long Polling?**

#### **How Long Polling Works?**

Long Polling is a technique where:

1. The client (browser) **sends a request** to the server.
2. The server **holds the request open** until new data is available.
3. Once new data is available, the server **responds** and closes the request.
4. The client **immediately sends another request**, repeating the process.

This **creates a loop** of continuous requests to keep the client updated.

#### **Example of Long Polling in Node.js**

📄 **Server-side (Node.js using Express)**

```javascript
const express = require("express");
const app = express();

app.get("/long-polling", (req, res) => {
    setTimeout(() => {
        res.json({ message: "New data available!" });
    }, 5000); // Wait for 5 seconds before responding
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

📄 **Client-side (JavaScript)**

```javascript
function longPolling() {
    fetch("http://localhost:3000/long-polling")
        .then(response => response.json())
        .then(data => {
            console.log(data.message);
            longPolling(); // Send another request immediately
        })
        .catch(err => console.error("Error:", err));
}

longPolling(); // Start long polling
```

✅ **What happens here?**\
✔ The client **sends a request** to the server.\
✔ The server **waits 5 seconds** before responding with new data.\
✔ The client **immediately sends another request**, repeating the process.

***

### **3. What are WebSockets?**

#### **How WebSockets Work?**

WebSockets create a **persistent** connection between the client and server:

1. The client **initiates a connection** (handshake).
2. The server **accepts** and **keeps the connection open**.
3. The client and server can **send messages anytime** in both directions.

Unlike Long Polling, **WebSockets do not require repeated requests**. The connection stays **open** until it is closed manually or lost.

#### **Example of WebSockets in Node.js**

📄 **Server-side (Node.js with WebSockets)**

```javascript
const WebSocket = require("ws");
const server = new WebSocket.Server({ port: 3000 });

server.on("connection", (socket) => {
    console.log("Client connected");

    setInterval(() => {
        socket.send("Hello from server!"); // Send data every 5 seconds
    }, 5000);

    socket.on("message", (message) => {
        console.log("Received:", message);
    });

    socket.on("close", () => {
        console.log("Client disconnected");
    });
});
```

📄 **Client-side (JavaScript)**

```javascript
const socket = new WebSocket("ws://localhost:3000");

socket.onmessage = (event) => {
    console.log("Message from server:", event.data);
};

socket.onopen = () => {
    socket.send("Hello Server!"); // Send message to server
};
```

✅ **What happens here?**\
✔ The client **connects** to the server using WebSockets.\
✔ The server **sends messages every 5 seconds** without new requests.\
✔ The client **receives messages instantly** without polling.

***

### **4. Comparison: Long Polling vs WebSockets**

| Feature         | Long Polling               | WebSockets                      |
| --------------- | -------------------------- | ------------------------------- |
| Connection Type | Repeated HTTP requests     | Persistent connection           |
| Data Transfer   | Only when new request sent | Can send and receive anytime    |
| Latency         | Higher due to delays       | Very low, almost instant        |
| Server Load     | High (many requests)       | Low (single connection)         |
| Complexity      | Easy to implement          | Requires WebSocket server       |
| Browser Support | All browsers               | Most modern browsers support it |

***

### **5. When to Use Long Polling vs WebSockets?**

#### **Use Long Polling when:**

✔ **WebSockets are not supported** by the client or server.\
✔ You need a **simple real-time update mechanism**.\
✔ You have **low-frequency updates** (e.g., notifications every few minutes).

#### **Use WebSockets when:**

✔ You need **instant communication** (e.g., live chat, gaming, stock prices).\
✔ You want **lower server load** with **faster updates**.\
✔ Your application requires **continuous bi-directional communication**.

***

### **6. Conclusion**

✅ **Long Polling** is a workaround for real-time updates but is inefficient for high-frequency updates.\
✅ **WebSockets** are more efficient, providing **instant, two-way communication**.\
✅ **Choose WebSockets** for chat apps, live data updates, and interactive features.\
✅ **Use Long Polling** if WebSockets are not available or if updates are infrequent.
