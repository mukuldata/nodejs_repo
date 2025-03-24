# WebHooks

### **Table of Contents**

1. **Introduction to Webhooks**
2. **How Webhooks Work**
3. **Webhooks vs. APIs**
4. **Common Use Cases of Webhooks**
5. **Setting Up a Webhook in Node.js**
6. **Consuming Webhooks in a React App**
7. **Securing Webhooks**
8. **Testing Webhooks with ngrok**
9. **Best Practices for Webhooks**
10. **Conclusion**

***

### **1. Introduction to Webhooks**

1. A **webhook** is a way for one application to send real-time data to another application **automatically** whenever a specific event happens.&#x20;
2. It acts like a **notification system** that pushes data from one system to another **instantly** instead of requiring the second system to constantly check for updates.

#### **Example:**

Imagine you have a **payment system** (like Razorpay, Stripe, or PayPal). When a user makes a successful payment, the payment provider sends a **webhook notification** to your backend with payment details, so you can update the order status in your database.

***

### **2. How Webhooks Work**

1. **An event occurs** in the source system (e.g., a user makes a payment).
2. **The source system sends an HTTP request** (a webhook) to a pre-configured URL.
3. **The destination system (your app) receives the webhook** and processes the data.

#### **Webhook Example Flow:**

🔹 **Trigger:** A new user signs up on a website.\
🔹 **Webhook:** The website sends a webhook to an email service (e.g., Mailchimp).\
🔹 **Action:** Mailchimp adds the new user to an email list automatically.

***

### **3. Webhooks vs. APIs**

| Feature        | Webhooks                                      | APIs                                       |
| -------------- | --------------------------------------------- | ------------------------------------------ |
| **Data Flow**  | Push (automatic)                              | Pull (request-based)                       |
| **Real-time?** | Yes                                           | No (requires polling)                      |
| **Use Case**   | Instant notifications (e.g., payment updates) | Fetching data on demand                    |
| **Efficiency** | More efficient (no unnecessary requests)      | Less efficient (requires frequent polling) |

#### **When to Use Webhooks vs. APIs?**

* Use **webhooks** when you need **real-time updates** (e.g., payment status, order tracking).
* Use **APIs** when you need **on-demand data fetching** (e.g., retrieving user profiles).

***

### **4. Common Use Cases of Webhooks**

✅ **Payment Processing:** Notify when a transaction is successful (e.g., Stripe, Razorpay).\
✅ **CI/CD Pipelines:** Trigger deployments automatically when code is pushed (e.g., GitHub Actions).\
✅ **Chatbots & Notifications:** Send messages to Slack/Telegram when an event occurs.\
✅ **E-commerce:** Update inventory when a product is sold.\
✅ **CRM Integration:** Add leads to a CRM system when a form is submitted.

***

### **5. Setting Up a Webhook in Node.js**

#### **Step 1: Create a Simple Express Server**

```sh
mkdir webhook-server && cd webhook-server
npm init -y
npm install express body-parser
```

📄 **server.js**

```javascript
const express = require("express");
const bodyParser = require("body-parser");

const app = express();
const PORT = 5000;

// Middleware to parse JSON data
app.use(bodyParser.json());

// Webhook endpoint
app.post("/webhook", (req, res) => {
    console.log("Webhook received:", req.body);
    res.status(200).json({ message: "Webhook received successfully!" });
});

// Start server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

#### **Step 2: Start the Webhook Server**

```sh
node server.js
```

***

### **6. Consuming Webhooks in a React App**

When a webhook sends data to your backend, you can update the frontend (React) using **Socket.io** or **API polling**.

Example: Fetch new payment status in React.

📄 **React Component (useEffect to Fetch Webhook Data)**

```javascript
import React, { useEffect, useState } from "react";

const PaymentStatus = () => {
    const [status, setStatus] = useState("Pending");

    useEffect(() => {
        fetch("http://localhost:5000/payment-status")
            .then(response => response.json())
            .then(data => setStatus(data.status))
            .catch(error => console.error("Error fetching data:", error));
    }, []);

    return <h2>Payment Status: {status}</h2>;
};

export default PaymentStatus;
```

***

### **7. Securing Webhooks**

Webhooks can be a **security risk** if they are not properly secured. Here’s how to protect them:

✅ **Use Secret Keys**: Add a verification token to validate the webhook sender.\
✅ **Validate the Request Origin**: Only accept requests from trusted sources.\
✅ **Use HTTPS**: Always encrypt webhook communication.\
✅ **Rate Limiting**: Prevent too many requests from overloading your server.

#### **Example: Verify Webhook Secret**

📄 **server.js (Webhook Security Example)**

```javascript
const WEBHOOK_SECRET = "mysecretkey";

app.post("/webhook", (req, res) => {
    const receivedSecret = req.headers["x-webhook-secret"];
    
    if (receivedSecret !== WEBHOOK_SECRET) {
        return res.status(403).json({ error: "Unauthorized webhook request" });
    }

    console.log("Valid webhook received:", req.body);
    res.status(200).json({ message: "Webhook processed successfully!" });
});
```

***

### **8. Testing Webhooks with ngrok**

Since webhooks **require a public URL**, we can use **ngrok** to expose our local server.

#### **Step 1: Install ngrok**

```sh
npm install -g ngrok
```

#### **Step 2: Run ngrok to Expose Local Server**

```sh
ngrok http 5000
```

🔹 **ngrok** will generate a temporary public URL like:

```sh
https://randomsubdomain.ngrok.io
```

Use this **ngrok URL** as the webhook endpoint instead of `http://localhost:5000/webhook`.

***

### **9. Best Practices for Webhooks**

✅ **Use Authentication** → Validate requests using secret tokens.\
✅ **Retry Mechanism** → Webhook providers should retry if the request fails.\
✅ **Logging & Monitoring** → Store logs of received webhooks for debugging.\
✅ **Handle Duplicate Events** → Sometimes, webhooks are sent multiple times. Use unique IDs to avoid processing duplicates.\
✅ **Optimize Performance** → Process webhooks asynchronously (e.g., use a queue like RabbitMQ or Redis).

***

### **10. Conclusion**

🔹 **Webhooks are event-driven HTTP requests** that allow real-time communication between apps.\
🔹 Unlike APIs, webhooks **push** data instead of requiring frequent requests.\
🔹 **Common use cases include payments, automation, and notifications.**\
🔹 **Webhooks can be secured using secret keys and HTTPS.**\
🔹 **Testing webhooks locally is easy with ngrok.**
