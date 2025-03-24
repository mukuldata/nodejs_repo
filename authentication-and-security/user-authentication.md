# User Authentication

## **User Authentication in Node.js (JWT, Passport.js, bcrypt)**

### **Table of Contents**

1. **What is User Authentication?**
2. **Why is Authentication Important?**
3. **JSON Web Token (JWT) Authentication**
   * How JWT Works
   * Implementing JWT Authentication (Example)
4. **Passport.js Authentication**
   * What is Passport.js?
   * Using Passport.js for Authentication (Example)
5. **Password Hashing with bcrypt**
   * Why Use bcrypt?
   * Implementing bcrypt for Secure Passwords (Example)
6. **Conclusion**

***

### **1. What is User Authentication?**

User authentication is the process of verifying a user's identity before allowing access to an application. It ensures that only authorized users can access protected resources.

***

### **2. Why is Authentication Important?**

* Protects sensitive data from unauthorized access.
* Ensures only valid users can perform actions.
* Helps maintain **user sessions** securely.

***

### **3. JSON Web Token (JWT) Authentication**

#### **How JWT Works?**

JWT is a token-based authentication method. The server generates a token after successful login, which the user sends with each request for authorization.

✔ **JWT Structure:**\
A JWT consists of **three parts**:

1. **Header** – contains token type and algorithm.
2. **Payload** – stores user details (e.g., `id`, `email`).
3. **Signature** – ensures the token is not tampered with.

📌 **Example JWT:**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkwIiwiaWF0IjoxNjUwMzQ1NjAwfQ.jzDFzL5Vo84Hp8bPZ0Hj
```

#### **Implementing JWT Authentication (Example)**

📌 **Step 1: Install dependencies**

```bash
npm install express jsonwebtoken dotenv
```

📌 **Step 2: Generate JWT Token**

```javascript
const express = require('express');
const jwt = require('jsonwebtoken');
require('dotenv').config();

const app = express();
app.use(express.json());

const users = [{ id: 1, username: 'mukul', password: '1234' }];

app.post('/login', (req, res) => {
    const { username, password } = req.body;
    const user = users.find(u => u.username === username && u.password === password);

    if (!user) return res.status(401).json({ message: 'Invalid credentials' });

    // Generate a token
    const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET, { expiresIn: '1h' });
    res.json({ token });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

✔ **Now, the user gets a token after login!**

📌 **Step 3: Protect Routes with JWT**

```javascript
const authenticate = (req, res, next) => {
    const token = req.headers.authorization?.split(' ')[1];
    if (!token) return res.status(403).json({ message: 'Token required' });

    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        req.user = decoded;
        next();
    } catch (error) {
        res.status(401).json({ message: 'Invalid token' });
    }
};

// Protected route
app.get('/dashboard', authenticate, (req, res) => {
    res.json({ message: 'Welcome to the dashboard!', user: req.user });
});
```

✔ Now, users must send their token to access protected routes!

***

### **4. Passport.js Authentication**

#### **What is Passport.js?**

Passport.js is a flexible authentication middleware for Node.js. It supports **multiple authentication strategies** (Google, Facebook, JWT, etc.).

#### **Using Passport.js for Authentication (Example)**

📌 **Step 1: Install Passport & Local Strategy**

```bash
npm install passport passport-local express-session
```

📌 **Step 2: Configure Passport.js**

```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const users = [{ id: 1, username: 'mukul', password: '1234' }];

// Configure strategy
passport.use(new LocalStrategy((username, password, done) => {
    const user = users.find(u => u.username === username);
    if (!user || user.password !== password) return done(null, false);
    return done(null, user);
}));

passport.serializeUser((user, done) => done(null, user.id));
passport.deserializeUser((id, done) => {
    const user = users.find(u => u.id === id);
    done(null, user);
});
```

📌 **Step 3: Use Passport in Express**

```javascript
const express = require('express');
const session = require('express-session');

const app = express();
app.use(express.json());
app.use(session({ secret: 'mysecret', resave: false, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());

app.post('/login', passport.authenticate('local', {
    successRedirect: '/dashboard',
    failureRedirect: '/login'
}));

app.get('/dashboard', (req, res) => {
    if (!req.isAuthenticated()) return res.status(401).json({ message: 'Not logged in' });
    res.json({ message: 'Welcome to the dashboard!' });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

✔ Passport manages user sessions and authentication!

***

### **5. Password Hashing with bcrypt**

#### **Why Use bcrypt?**

* **Hashes passwords** before storing them.
* **Protects against brute-force attacks.**
* **Adds a salt** to make passwords unique.

#### **Implementing bcrypt for Secure Passwords**

📌 **Step 1: Install bcrypt**

```bash
npm install bcrypt
```

📌 **Step 2: Hash a Password Before Storing**

```javascript
const bcrypt = require('bcrypt');

const hashPassword = async (password) => {
    const saltRounds = 10;
    const hashed = await bcrypt.hash(password, saltRounds);
    console.log('Hashed Password:', hashed);
};

hashPassword('mypassword123');
```

✔ The output will be a **hashed version** of the password.

📌 **Step 3: Verify Password at Login**

```javascript
const verifyPassword = async (password, hash) => {
    const isMatch = await bcrypt.compare(password, hash);
    console.log('Password Match:', isMatch);
};

verifyPassword('mypassword123', '$2b$10$5uVqJ...'); // Hash from database
```

✔ **bcrypt ensures only correct passwords match!**

***

### **6. Conclusion**

✔ **JWT** – Best for API authentication with tokens.\
✔ **Passport.js** – Best for session-based authentication.\
✔ **bcrypt** – Ensures passwords are **securely stored**.

&#x20;**Use JWT for APIs, Passport.js for session-based auth, and bcrypt to hash passwords!**
