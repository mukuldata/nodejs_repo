# React with Node JS

### **Table of Contents**

1. **Introduction**
2. **Why Connect React with Node.js?**
3. **Setting Up the Node.js Backend (API)**
4. **Setting Up the React Frontend**
5. **Making API Requests from React (Fetching Data)**
6. **Sending Data from React to Node.js (POST Request)**
7. **Handling CORS Issues**
8. **Connecting React with Node.js using Proxy**
9. **Using Axios for API Calls**
10. **Conclusion**

***

### **1. Introduction**

When building a **full-stack web application**, we often use **React.js** for the frontend and **Node.js with Express** for the backend. The frontend interacts with the backend through **APIs (Application Programming Interfaces)**, which allow data to be sent and received between the two.

#### **How Does It Work?**

* The **React frontend** sends a request to the **Node.js API**.
* The **Node.js API** processes the request, retrieves/stores data, and sends a response back.
* The **React frontend** displays the received data.

***

### **2. Why Connect React with Node.js?**

✅ **Separation of Concerns** → Frontend (UI) and Backend (Logic) are separate.\
✅ **Scalability** → Allows the frontend and backend to evolve independently.\
✅ **Reusability** → Backend APIs can be used by other clients (mobile apps, web apps).\
✅ **Security** → Backend APIs can handle authentication and authorization.

***

### **3. Setting Up the Node.js Backend (API)**

First, create a **Node.js + Express server** to handle API requests.

#### **Step 1: Install Required Packages**

```sh
mkdir node-api
cd node-api
npm init -y
npm install express cors body-parser
```

#### **Step 2: Create the Express Server**

📄 **server.js**

```javascript
const express = require("express");
const cors = require("cors");

const app = express();
const PORT = 5000;

// Middleware
app.use(cors()); // Allows cross-origin requests
app.use(express.json()); // Parses incoming JSON requests

// Sample API Endpoint
app.get("/api/message", (req, res) => {
    res.json({ message: "Hello from Node.js API!" });
});

// Start Server
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
```

#### **Step 3: Run the Backend Server**

```sh
node server.js
```

***

### **4. Setting Up the React Frontend**

#### **Step 1: Create a React App**

```sh
npx create-react-app react-frontend
cd react-frontend
npm start
```

#### **Step 2: Create a Component to Fetch Data from the Backend**

📄 **src/components/Message.js**

```javascript
import React, { useEffect, useState } from "react";

const Message = () => {
    const [message, setMessage] = useState("");

    useEffect(() => {
        fetch("http://localhost:5000/api/message")
            .then((response) => response.json())
            .then((data) => setMessage(data.message))
            .catch((error) => console.error("Error fetching message:", error));
    }, []);

    return <h2>{message}</h2>;
};

export default Message;
```

#### **Step 3: Display the Component in App.js**

📄 **src/App.js**

```javascript
import React from "react";
import Message from "./components/Message";

function App() {
    return (
        <div>
            <h1>React & Node.js Connection</h1>
            <Message />
        </div>
    );
}

export default App;
```

***

### **5. Making API Requests from React (Fetching Data)**

#### **Using fetch()**

```javascript
fetch("http://localhost:5000/api/message")
    .then(response => response.json())
    .then(data => console.log(data.message))
    .catch(error => console.error("Error fetching data:", error));
```

#### **Using Axios (Alternative to fetch)**

```sh
npm install axios
```

```javascript
import axios from "axios";

axios.get("http://localhost:5000/api/message")
    .then(response => console.log(response.data.message))
    .catch(error => console.error("Error fetching data:", error));
```

***

### **6. Sending Data from React to Node.js (POST Request)**

Modify the **Node.js API** to accept data from React:

📄 **server.js (Add a new POST route)**

```javascript
app.post("/api/send-data", (req, res) => {
    const { name } = req.body;
    res.json({ message: `Hello, ${name}! Data received successfully.` });
});
```

#### **React: Sending Data with fetch()**

```javascript
const sendData = async () => {
    const response = await fetch("http://localhost:5000/api/send-data", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name: "Mukul" })
    });

    const data = await response.json();
    console.log(data.message);
};
```

#### **React: Sending Data with Axios**

```javascript
axios.post("http://localhost:5000/api/send-data", { name: "Mukul" })
    .then(response => console.log(response.data.message))
    .catch(error => console.error("Error sending data:", error));
```

***

### **7. Handling CORS Issues**

If you see a **CORS error**, update the **Node.js server**:

📄 **server.js**

```javascript
const cors = require("cors");
app.use(cors()); // Allows requests from different origins
```

***

### **8. Connecting React with Node.js using Proxy**

Instead of using `http://localhost:5000` in every API call, you can **set a proxy in React**.

📄 **package.json (React App)**

```json
"proxy": "http://localhost:5000"
```

Now, you can make requests like this:

```javascript
fetch("/api/message")
```

instead of

```javascript
fetch("http://localhost:5000/api/message")
```

***

### **9. Using Axios for API Calls**

Axios provides a simpler way to make API calls:

📄 **src/api.js**

```javascript
import axios from "axios";

const api = axios.create({
    baseURL: "http://localhost:5000",
});

export default api;
```

Now, you can use `api.get()` instead of writing the full URL:

```javascript
api.get("/api/message")
    .then(response => console.log(response.data.message))
    .catch(error => console.error("Error:", error));
```

***

### **10. Conclusion**

✅ **React communicates with Node.js using API calls**\
✅ **fetch() and Axios are commonly used for API requests**\
✅ **CORS must be handled to allow cross-origin requests**\
✅ **Proxy settings simplify API requests**\
✅ **Data can be sent via GET (fetch data) and POST (send data)**
