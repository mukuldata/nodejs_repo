# Helmet

### **Table of Contents**

1. **What is Helmet.js?**
2. **Why Use Helmet.js?**
3. **Installing Helmet.js**
4. **Using Helmet.js in Express**
5. **Helmet.js Middleware Options Explained**
6. **Example: Secure Express App with Helmet.js**
7. **Conclusion**

***

### **1. What is Helmet.js?**

Helmet.js is a Node.js middleware that **secures Express applications** by setting various **HTTP headers**. These headers help protect web applications from common security threats like **Cross-Site Scripting (XSS), Clickjacking, and MIME sniffing**.

***

### **2. Why Use Helmet.js?**

✔ **Prevents security vulnerabilities**\
✔ **Adds security headers automatically**\
✔ **Easy to implement**\
✔ **Helps prevent common web attacks**

***

### **3. Installing Helmet.js**

To install Helmet.js in your Express application, run:

```bash
npm install helmet
```

***

### **4. Using Helmet.js in Express**

📌 **Basic Usage in Express.js**

```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();

// Use Helmet middleware
app.use(helmet());

app.get('/', (req, res) => {
    res.send('Helmet.js is securing your Express app!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

This **automatically** applies **various security headers** to your Express application.

***

### **5. Helmet.js Middleware Options Explained**

Helmet comes with several built-in security headers. Let's explore them:

| **Middleware**                          | **Purpose**                                                                   |
| --------------------------------------- | ----------------------------------------------------------------------------- |
| `helmet.contentSecurityPolicy()`        | Prevents XSS attacks by restricting allowed sources for scripts, styles, etc. |
| `helmet.crossOriginEmbedderPolicy()`    | Prevents embedding of unauthorized resources.                                 |
| `helmet.crossOriginOpenerPolicy()`      | Isolates browsing context to protect against cross-origin attacks.            |
| `helmet.crossOriginResourcePolicy()`    | Restricts access to resources from unauthorized origins.                      |
| `helmet.dnsPrefetchControl()`           | Controls browser's DNS prefetching to prevent DNS-based attacks.              |
| `helmet.expectCt()`                     | Enforces Certificate Transparency logs for HTTPS.                             |
| `helmet.frameguard()`                   | Prevents clickjacking by blocking embedding in `<iframe>`.                    |
| `helmet.hidePoweredBy()`                | Hides the `X-Powered-By: Express` header to avoid targeted attacks.           |
| `helmet.hsts()`                         | Enforces HTTPS connection for security.                                       |
| `helmet.ieNoOpen()`                     | Prevents old Internet Explorer security flaws.                                |
| `helmet.noSniff()`                      | Prevents MIME type sniffing.                                                  |
| `helmet.originAgentCluster()`           | Enforces same-origin policies for security.                                   |
| `helmet.permittedCrossDomainPolicies()` | Restricts Adobe Flash cross-domain policies.                                  |
| `helmet.referrerPolicy()`               | Controls how `Referer` header is sent for security.                           |
| `helmet.xssFilter()`                    | Helps prevent Cross-Site Scripting (XSS) attacks.                             |

***

### **6. Example: Secure Express App with Helmet.js**

#### **Step 1: Install Dependencies**

```bash
npm install express helmet
```

#### **Step 2: Create an Express Server with Helmet.js**

📌 **server.js**

```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();

// Enable Helmet with specific options
app.use(helmet());

// Customizing specific Helmet security headers
app.use(helmet({
    contentSecurityPolicy: false, // Disables CSP for simplicity (not recommended for production)
    frameguard: { action: 'deny' }, // Blocks embedding in iframes
    referrerPolicy: { policy: 'no-referrer' }, // Hides Referer header
    hidePoweredBy: true, // Hides Express header
}));

app.get('/', (req, res) => {
    res.send('Helmet.js is protecting your app!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

#### **Step 3: Run the Server**

```bash
node server.js
```

Now, your Express app is protected by **Helmet.js**.

***

### **7. Conclusion**

🔹 **Helmet.js adds security headers** to your Express app, protecting it from common attacks.\
🔹 It is **lightweight**, **easy to use**, and **highly configurable**.\
🔹 Recommended for **all Express.js applications** to enhance security.
