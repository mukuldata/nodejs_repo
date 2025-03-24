# CSRF Protection

### **Table of Contents**

1. **What is CSRF (Cross-Site Request Forgery)?**
2. **How CSRF Attacks Work?**
3. **CSRF Protection using csurf Middleware**
4. **What is XSS (Cross-Site Scripting)?**
5. **Preventing XSS in Express.js**
6. **What is SQL Injection?**
7. **Preventing SQL Injection**
8. **Conclusion**

***

### **1. What is CSRF (Cross-Site Request Forgery)?**

CSRF (Cross-Site Request Forgery) is a type of attack where a hacker tricks a user into **performing actions they didn’t intend** on a trusted website.

#### **Example of CSRF Attack**

* A user logs into their bank account (`bank.com`).
* The user unknowingly clicks on a malicious link (`hacker.com`).
* That link **triggers a request** to `bank.com` to transfer money to the hacker’s account **without user consent**.

***

### **2. How CSRF Attacks Work?**

1. **User logs into a trusted website** (e.g., an online banking site).
2. The hacker sends a **hidden request** (like a money transfer) using a malicious website.
3. Since the user is already logged in, the website **processes the request** thinking it’s from the user.

***

### **3. CSRF Protection using csurf Middleware**

#### **Install csurf Middleware**

Run the following command:

```bash
npm install csurf cookie-parser
```

#### **Use csurf in Express.js**

```javascript
const express = require('express');
const csrf = require('csurf');
const cookieParser = require('cookie-parser');

const app = express();
app.use(express.urlencoded({ extended: true }));
app.use(cookieParser());

// Enable CSRF protection
const csrfProtection = csrf({ cookie: true });
app.use(csrfProtection);

app.get('/form', (req, res) => {
    res.send(`<form action="/submit" method="POST">
        <input type="hidden" name="_csrf" value="${req.csrfToken()}">
        <input type="text" name="message">
        <button type="submit">Submit</button>
    </form>`);
});

app.post('/submit', (req, res) => {
    res.send('Form submitted successfully!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

🔹 **How It Works?**

* Generates a **CSRF token** for each form request.
* The server **verifies the token** when the form is submitted.
* If the token is **missing or incorrect**, the request is rejected.

***

### **4. What is XSS (Cross-Site Scripting)?**

XSS (Cross-Site Scripting) is an attack where a hacker **injects malicious JavaScript code** into a website.

#### **Example of XSS Attack**

* A comment box allows users to enter text.
*   A hacker enters:

    ```html
    <script>alert('Hacked!');</script>
    ```
* If the site doesn’t sanitize input, this script **executes on all visitors’ browsers**.

***

### **5. Preventing XSS in Express.js**

#### **1. Use helmet.js to Set Security Headers**

Install `helmet` to protect against XSS:

```bash
npm install helmet
```

Use it in Express:

```javascript
const helmet = require('helmet');
app.use(helmet());
```

🔹 Helmet helps prevent **browser-based attacks** like XSS.

#### **2. Sanitize User Input Using express-validator**

Install express-validator:

```bash
npm install express-validator
```

Use it in Express:

```javascript
const { body, validationResult } = require('express-validator');

app.post('/comment', 
    body('comment').escape(), 
    (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }
        res.send('Comment submitted safely!');
});
```

🔹 This **removes harmful characters** from user input before storing it.

***

### **6. What is SQL Injection?**

SQL Injection is an attack where a hacker **manipulates database queries** by injecting malicious SQL code.

#### **Example of SQL Injection Attack**

```javascript
const userInput = "'; DROP TABLE users; --";
db.query(`SELECT * FROM users WHERE name = '${userInput}'`);
```

🔹 If unsanitized, this **deletes the entire users table!**

***

### **7. Preventing SQL Injection**

#### **1. Use Parameterized Queries (Prepared Statements)**

If using MySQL, install mysql2:

```bash
npm install mysql2
```

Use prepared statements:

```javascript
const mysql = require('mysql2');
const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '',
    database: 'test_db'
});

const username = 'john';
connection.execute('SELECT * FROM users WHERE username = ?', [username], (err, results) => {
    if (err) throw err;
    console.log(results);
});
```

🔹 This ensures **user input is treated as data** instead of SQL code.

#### **2. Use ORM (like Sequelize or Mongoose)**

ORMs handle SQL safely:

```javascript
const { User } = require('./models');
User.findOne({ where: { username: 'john' } }).then(user => console.log(user));
```

***

### **8. Conclusion**

✅ **CSRF Protection:** Use `csurf` to protect **form submissions** from unauthorized requests.\
✅ **XSS Prevention:** Use `helmet` for security headers & `express-validator` to sanitize user input.\
✅ **SQL Injection Protection:** Use **parameterized queries** and ORM tools like Sequelize or Mongoose.
