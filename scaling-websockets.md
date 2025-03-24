# Scaling WebSockets

### **Table of Contents**

1. **Introduction to WebSocket Scaling**
2. **Challenges in Scaling WebSockets**
3. **What is Redis Pub/Sub?**
4. **How Redis Helps in Scaling WebSockets**
5. **Handling WebSocket Disconnections & Reconnects**
6. **Example Implementation of WebSockets with Redis Pub/Sub**
7. **Conclusion**

***

### **1. Introduction to WebSocket Scaling**

WebSockets allow **real-time, bi-directional communication** between a client and a server. However, when an application grows and requires **multiple servers**, WebSockets face **scalability issues**.

#### **Why Scaling is Needed?**

1. **Single server cannot handle too many WebSocket connections.**
2. **Load balancing across multiple servers is required.**
3. **Clients connected to different servers should receive the same real-time messages.**

**Solution:** Use **Redis Pub/Sub** to synchronize messages across multiple WebSocket servers.

***

### **2. Challenges in Scaling WebSockets**

| Challenge                    | Problem Description                                                                                             |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Single Server Limitation** | A single WebSocket server cannot handle thousands of connections efficiently.                                   |
| **Load Balancing Issues**    | When using multiple servers, a WebSocket connection stays on one server, making it difficult to share messages. |
| **Data Consistency**         | Messages sent from one server should be available to users on another server.                                   |
| **Reconnection Handling**    | If a client disconnects, it should reconnect and get missing messages.                                          |

#### **Example Problem:**

* **User A connects to Server 1**
* **User B connects to Server 2**
* If **User A** sends a message, **User B** should receive it. But since they are on different servers, they cannot communicate directly.

**Solution:** Redis **Pub/Sub** acts as a central messaging system to **broadcast** messages to all servers.

***

### **3. What is Redis Pub/Sub?**

Redis Pub/Sub (Publish-Subscribe) is a messaging system that allows **servers to communicate** by **publishing** and **subscribing** to channels.

#### **How Redis Pub/Sub Works?**

1. **Publishers (WebSocket Servers)** send messages to a Redis **channel**.
2. **Subscribers (Other WebSocket Servers)** listen to the channel and receive messages in real-time.

#### **Example of Redis Pub/Sub Commands**

*   **Publish a message:**

    ```sh
    PUBLISH chat_channel "Hello from Server 1"
    ```
*   **Subscribe to a channel:**

    ```sh
    SUBSCRIBE chat_channel
    ```

***

### **4. How Redis Helps in Scaling WebSockets?**

| **Problem**                                          | **Solution with Redis Pub/Sub**                                            |
| ---------------------------------------------------- | -------------------------------------------------------------------------- |
| WebSocket connections are limited to a single server | Redis allows WebSocket servers to **share real-time messages**             |
| Messages need to be sent across multiple servers     | Redis **broadcasts messages** to all connected servers                     |
| Clients may connect to different servers             | Every WebSocket server **subscribes** to Redis channels to receive updates |

#### **How Redis Works in a Multi-Server WebSocket Setup?**

1. **Multiple WebSocket servers** handle clients.
2. **Each WebSocket server connects to Redis**.
3. When a user **sends a message**, the server **publishes it to Redis**.
4. All other WebSocket servers **receive the message via Redis** and send it to their connected clients.

***

### **5. Handling WebSocket Disconnections & Reconnects**

#### **Why Do WebSockets Disconnect?**

* **Network issues**
* **Server restart**
* **Load balancer switching clients to a different server**

#### **How to Handle Disconnections?**

1. **Auto-Reconnect:** The client should attempt to reconnect if disconnected.
2. **Session Storage:** Store session data in Redis so users can resume their chat after reconnecting.
3. **Message Replay:** If a user reconnects, fetch recent messages from Redis to avoid missing updates.

#### **Client-side Auto-Reconnect Example:**

```javascript
let socket;

function connectWebSocket() {
    socket = new WebSocket("ws://localhost:3000");

    socket.onopen = () => {
        console.log("WebSocket connected");
    };

    socket.onmessage = (event) => {
        console.log("Message from server:", event.data);
    };

    socket.onclose = () => {
        console.log("WebSocket disconnected. Reconnecting...");
        setTimeout(connectWebSocket, 3000); // Reconnect after 3 seconds
    };
}

connectWebSocket(); // Start WebSocket connection
```

***

### **6. Example Implementation of WebSockets with Redis Pub/Sub**

#### **Install Required Dependencies**

```sh
npm install ws redis express
```

#### **WebSocket Server with Redis Pub/Sub**

📄 **Server 1 (ws\_server1.js)**

```javascript
const WebSocket = require("ws");
const redis = require("redis");

const wss = new WebSocket.Server({ port: 3001 });
const redisClient = redis.createClient();

// Subscribe to Redis channel
redisClient.subscribe("chat_channel");
redisClient.on("message", (channel, message) => {
    wss.clients.forEach(client => {
        if (client.readyState === WebSocket.OPEN) {
            client.send(message);
        }
    });
});

wss.on("connection", (ws) => {
    ws.on("message", (message) => {
        console.log("Received:", message);
        redisClient.publish("chat_channel", message); // Publish message to Redis
    });
});

console.log("WebSocket Server 1 running on port 3001");
```

📄 **Server 2 (ws\_server2.js)**

```javascript
const WebSocket = require("ws");
const redis = require("redis");

const wss = new WebSocket.Server({ port: 3002 });
const redisClient = redis.createClient();

// Subscribe to Redis channel
redisClient.subscribe("chat_channel");
redisClient.on("message", (channel, message) => {
    wss.clients.forEach(client => {
        if (client.readyState === WebSocket.OPEN) {
            client.send(message);
        }
    });
});

wss.on("connection", (ws) => {
    ws.on("message", (message) => {
        console.log("Received:", message);
        redisClient.publish("chat_channel", message); // Publish message to Redis
    });
});

console.log("WebSocket Server 2 running on port 3002");
```

📄 **Client-Side WebSocket Code**

```javascript
const socket = new WebSocket("ws://localhost:3001");

socket.onopen = () => {
    console.log("Connected to WebSocket Server");
    socket.send("Hello from Client!");
};

socket.onmessage = (event) => {
    console.log("Received:", event.data);
};
```

✅ **What Happens Here?**\
✔ Two WebSocket servers (on port 3001 and 3002) **subscribe to Redis**.\
✔ When a client **sends a message**, the server **publishes it to Redis**.\
✔ All WebSocket servers **receive the message** and send it to connected clients.

***

### **7. Conclusion**

✅ **WebSockets need scaling** when handling large numbers of users.\
✅ **Redis Pub/Sub** allows **multiple WebSocket servers** to communicate efficiently.\
✅ **Disconnections should be handled** with **auto-reconnect** and **message replay**.\
✅ **Using Redis with WebSockets** enables real-time, **multi-server** applications.
