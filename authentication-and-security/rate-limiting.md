# Rate Limiting

### **Table of Contents**

1. **What is Rate Limiting?**
2. **Why Use Rate Limiting?**
3. **What is Brute Force Protection?**
4. **Installing express-rate-limit**
5. **Using express-rate-limit in Express.js**
6. **Customizing Rate Limits**
7. **Conclusion**

***

### **1. What is Rate Limiting?**

Rate limiting **controls the number of requests** a user or IP address can make to a server within a specific time.

#### **Example**

* Allow a user to make only **100 requests per 10 minutes**
* If they exceed this limit, their requests will be blocked temporarily

***

### **2. Why Use Rate Limiting?**

✔ **Prevents API abuse** (protects against too many requests)\
✔ **Reduces server load** (helps prevent crashes from excessive traffic)\
✔ **Stops Denial-of-Service (DoS) attacks**\
✔ **Protects login endpoints from brute-force attacks**

***

### **3. What is Brute Force Protection?**

Brute force attacks happen when hackers try **many username-password combinations** to guess a login.

#### **How Rate Limiting Helps?**

* Restricts repeated login attempts from the same IP
* Slows down automated attacks

***

### **4. Installing express-rate-limit**

Run the following command to install:

```bash
npm install express-rate-limit
```

***

### **5. Using express-rate-limit in Express.js**

📌 **Basic Implementation**

```javascript
const express = require('express');
const rateLimit = require('express-rate-limit');

const app = express();

// Apply rate limiting to all routes
const limiter = rateLimit({
    windowMs: 10 * 60 * 1000, // 10 minutes
    max: 100, // Limit each IP to 100 requests per windowMs
    message: 'Too many requests, please try again later.',
});

app.use(limiter);

app.get('/', (req, res) => {
    res.send('Welcome to the secure API!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

🔹 This allows **100 requests per 10 minutes** per IP.\
🔹 If the limit is exceeded, the user gets a **"Too many requests"** error.

***

### **6. Customizing Rate Limits**

You can customize `express-rate-limit` for **specific routes**, like login or API endpoints.

#### **Example: Limit Login Attempts**

```javascript
const loginLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // Allow only 5 login attempts per 15 minutes
    message: 'Too many failed login attempts, please try again later.',
});

app.post('/login', loginLimiter, (req, res) => {
    res.send('Login endpoint');
});
```

🔹 A user can try logging in **only 5 times per 15 minutes**.\
🔹 If they fail more than **5 times**, they must wait before trying again.

***

### **7. Conclusion**

🔹 `express-rate-limit` **prevents excessive requests** and **brute-force attacks**.\
🔹 Helps protect **API endpoints** and **login routes**.\
🔹 **Customizable**: You can apply different limits to different routes.\
🔹 Recommended for **all production applications** to improve security.
