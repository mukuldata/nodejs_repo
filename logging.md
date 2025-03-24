# Logging

### **Table of Contents**

1. **What is Logging?**
2. **Why is Logging Important?**
3. **What is Morgan?**
   * How to Use Morgan
   * Example: Using Morgan in Express
4. **What is Winston?**
   * Features of Winston
   * Example: Using Winston for Logging
5. **Combining Morgan and Winston**
6. **Conclusion**

***

### **1. What is Logging?**

Logging is the process of recording **events, errors, and important actions** in an application. It helps developers understand what happens in the system.

***

### **2. Why is Logging Important?**

* Helps **debug errors** quickly.
* Tracks **API requests** and responses.
* Helps in **monitoring** system health.
* Useful for **security audits** and analytics.

***

### **3. What is Morgan?**

Morgan is a **HTTP request logger** middleware for Express. It logs details about incoming requests like:

* HTTP method (`GET`, `POST`, etc.)
* URL path
* Response status code (`200`, `404`, `500`, etc.)
* Response time

#### **How to Use Morgan?**

📌 **Step 1:** Install Morgan

```bash
npm install morgan
```

📌 **Step 2:** Use it in your Express app

```javascript
const express = require('express');
const morgan = require('morgan');

const app = express();

// Use Morgan to log requests in the console
app.use(morgan('dev'));

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

#### **Example Output in Console:**

```
GET / 200 8.523 ms - 12
```

✔ **Morgan Formats:**

* `'dev'` → Compact log with method, status, and time.
* `'combined'` → Detailed log including user info.
* `'tiny'` → Minimal log.

***

### **4. What is Winston?**

Winston is a **flexible and powerful logging library** used for:

* Logging **errors and warnings**.
* Writing logs to **files** and **databases**.
* Formatting logs in **JSON** or **text**.

#### **Features of Winston:**

✔ Supports **multiple log levels** (`info`, `warn`, `error`).\
✔ Can **store logs** in files or remote servers.\
✔ Customizable **formats** (JSON, timestamped logs, etc.).

#### **How to Use Winston?**

📌 **Step 1:** Install Winston

```bash
npm install winston
```

📌 **Step 2:** Create a Logger

```javascript
const winston = require('winston');

const logger = winston.createLogger({
    level: 'info',  // Log level
    format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.json()
    ),
    transports: [
        new winston.transports.Console(), // Logs to console
        new winston.transports.File({ filename: 'app.log' }) // Logs to a file
    ]
});

// Example logs
logger.info('This is an info message');
logger.warn('This is a warning');
logger.error('This is an error message');
```

✔ This will log messages **to both the console and a file (`app.log`)**.

📌 **Step 3:** Integrate Winston with Express

```javascript
const express = require('express');
const winston = require('winston');

const app = express();
const logger = winston.createLogger({
    transports: [
        new winston.transports.Console(),
        new winston.transports.File({ filename: 'server.log' })
    ]
});

app.get('/', (req, res) => {
    logger.info('Home route accessed');
    res.send('Hello, World!');
});

app.listen(3000, () => logger.info('Server started on port 3000'));
```

✔ **Logs are now stored in `server.log`**

***

### **5. Combining Morgan & Winston**

We can use **Morgan for request logging** and **Winston for storing logs**.

#### **Example: Using Morgan with Winston**

```javascript
const express = require('express');
const morgan = require('morgan');
const winston = require('winston');

const app = express();

// Winston logger
const logger = winston.createLogger({
    transports: [
        new winston.transports.Console(),
        new winston.transports.File({ filename: 'server.log' })
    ]
});

// Custom Morgan setup that logs requests using Winston
app.use(morgan('combined', {
    stream: {
        write: (message) => logger.info(message.trim()) // Log HTTP requests in Winston
    }
}));

app.get('/', (req, res) => {
    logger.info('Home page accessed');
    res.send('Hello, World!');
});

app.listen(3000, () => logger.info('Server is running on port 3000'));
```

✔ **Morgan logs all requests** and sends them to **Winston for storage**.

***

### **6. Conclusion**

* **Morgan** logs incoming HTTP requests.
* **Winston** logs errors and system events to files/databases.
* **Combining both** ensures complete logging.
* Logging helps in debugging, monitoring, and security.

&#x20;**Use Morgan for request logging and Winston for error logging!**
