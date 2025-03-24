# Middlewares

### **Table of Contents**

1. **What is Middleware?**
2. **How Middleware Works in Express.js**
3. **Types of Middleware**
   * Built-in Middleware
   * Third-party Middleware
   * Custom Middleware
4. **Examples of Each Middleware Type**
5. **Executing Multiple Middleware Functions**
6. **Error-handling Middleware**
7. **Conclusion**

***

## **1. What is Middleware?**

Middleware is a function that **executes between the request and response cycle** in an Express.js application. It can modify the request and response objects, stop the request, or pass it to the next middleware.

✔ **Used for logging, authentication, request parsing, and more**\
✔ **Middleware functions run in the order they are defined**

***

## **2. How Middleware Works in Express.js**

Middleware functions have access to:\
✅ **Request (`req`)** → The data coming from the client\
✅ **Response (`res`)** → The data being sent back\
✅ **Next (`next()`)** → Calls the next middleware function

#### **Example Middleware Structure**

```javascript
app.use((req, res, next) => {
    console.log('Middleware executed');
    next(); // Pass control to the next middleware
});
```

✔ **`next()` ensures the next middleware runs**\
✔ **Middleware can be global or route-specific**

***

## **3. Types of Middleware**

Middleware in Express.js is categorized into three types:

#### **A. Built-in Middleware** (comes with Express.js)

* `express.json()` → Parses JSON data
* `express.urlencoded()` → Parses URL-encoded data
* `express.static()` → Serves static files

#### **B. Third-party Middleware** (installed via `npm`)

* `cors` → Enables Cross-Origin Resource Sharing
* `helmet` → Improves security
* `morgan` → Logs HTTP requests

#### **C. Custom Middleware** (created by developers)

* Can be used for logging, authentication, validation, etc.

***

## **4. Examples of Each Middleware Type**

### **A. Built-in Middleware Example**

#### **1. `express.json()` - Parses JSON data**

```javascript
const express = require('express');
const app = express();

app.use(express.json()); // Middleware to parse JSON

app.post('/data', (req, res) => {
    console.log(req.body); // Access parsed JSON data
    res.send('Data received');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

✔ Converts JSON request body into JavaScript object

#### **2. `express.static()` - Serves Static Files**

```javascript
app.use(express.static('public'));
```

✔ Makes files inside the `public` folder accessible via browser

***

### **B. Third-party Middleware Example**

#### **1. `cors` - Allows Cross-Origin Requests**

```bash
npm install cors
```

```javascript
const cors = require('cors');
app.use(cors());
```

✔ Allows frontend apps on different domains to access your backend

#### **2. `morgan` - Logs HTTP Requests**

```bash
npm install morgan
```

```javascript
const morgan = require('morgan');
app.use(morgan('dev')); // Logs request method & status
```

✔ Useful for debugging and monitoring API usage

***

### **C. Custom Middleware Example**

#### **1. Logging Middleware**

```javascript
const logger = (req, res, next) => {
    console.log(`${req.method} request to ${req.url}`);
    next();
};

app.use(logger);
```

✔ Logs incoming requests for monitoring

#### **2. Authentication Middleware**

```javascript
const authMiddleware = (req, res, next) => {
    if (!req.headers.authorization) {
        return res.status(403).json({ error: 'Unauthorized' });
    }
    next();
};

app.use(authMiddleware);
```

✔ Blocks requests without an **Authorization** header

***

## **5. Executing Multiple Middleware Functions**

Middleware functions **run in sequence** if `next()` is used.

```javascript
const middleware1 = (req, res, next) => {
    console.log('Middleware 1 executed');
    next();
};

const middleware2 = (req, res, next) => {
    console.log('Middleware 2 executed');
    next();
};

app.use(middleware1);
app.use(middleware2);

app.get('/', (req, res) => {
    res.send('Hello, Express.js!');
});
```

✔ Middleware runs **before reaching the final route handler**

***

## **6. Error-handling Middleware**

Special middleware for handling errors **must have four parameters (`err, req, res, next`)**.

```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong' });
});
```

✔ Handles errors **gracefully**\
✔ Prevents application crashes

***

## **7. Conclusion**

✔ **Middleware functions control the request-response flow**\
✔ **Built-in middleware** (e.g., `express.json()`, `express.static()`)\
✔ **Third-party middleware** (e.g., `cors`, `morgan`)\
✔ **Custom middleware** (e.g., logging, authentication)\
✔ **Error-handling middleware** prevents server crashes
