# Basics

### **Table of Contents**

1. **What is Express.js?**
2. **Setting Up Express.js**
3. **Routes in Express.js**
4. **Middleware in Express.js**
5. **Controllers in Express.js**
6. **Error Handling in Express.js**
7. **Folder Structure for an Express.js Project**
8. **Conclusion**

***

## **1. What is Express.js?**

Express.js is a **minimal and fast web framework** for Node.js. It simplifies handling HTTP requests, defining routes, and managing middleware.

✔ **Lightweight & flexible**\
✔ **Used for building REST APIs & web apps**\
✔ **Handles requests, responses, and middleware efficiently**

***

## **2. Setting Up Express.js**

#### **Install Express.js**

```bash
npm init -y  # Initializes a Node.js project
npm install express  # Installs Express.js
```

#### **Create `index.js`**

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello, Express.js!');
});

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

✔ **Creates a basic server**\
✔ **Handles GET requests to the root (`/`) route**

***

## **3. Routes in Express.js**

Routes define how an application responds to different HTTP requests.

#### **Example: Basic Routes**

```javascript
app.get('/home', (req, res) => {
    res.send('Welcome to Home Page');
});

app.post('/data', (req, res) => {
    res.send('POST request received');
});

app.put('/update', (req, res) => {
    res.send('PUT request received');
});

app.delete('/delete', (req, res) => {
    res.send('DELETE request received');
});
```

✔ **`GET`** - Fetch data\
✔ **`POST`** - Send data\
✔ **`PUT`** - Update data\
✔ **`DELETE`** - Remove data

***

## **4. Middleware in Express.js**

Middleware functions **modify request & response objects** before reaching the final route.

#### **Types of Middleware**

1. **Built-in Middleware** (e.g., `express.json()`, `express.urlencoded()`)
2. **Third-party Middleware** (e.g., `cors`, `helmet`, `morgan`)
3. **Custom Middleware** (defined by the user)

#### **Example: Using Middleware**

```javascript
app.use(express.json()); // Parses JSON body

app.use((req, res, next) => {
    console.log('Middleware executed');
    next(); // Pass control to the next middleware
});

app.get('/middleware', (req, res) => {
    res.send('Middleware example');
});
```

✔ **Executes before handling requests**\
✔ **Used for logging, authentication, data parsing**

***

## **5. Controllers in Express.js**

Controllers separate **business logic** from routes, making code clean & reusable.

#### **Example: Using a Controller**

📁 `controllers/userController.js`

```javascript
exports.getUser = (req, res) => {
    res.json({ name: 'John Doe', age: 25 });
};
```

📁 `routes/userRoutes.js`

```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/user', userController.getUser);

module.exports = router;
```

📁 `index.js`

```javascript
const express = require('express');
const app = express();
const userRoutes = require('./routes/userRoutes');

app.use('/api', userRoutes);

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

✔ **Keeps code modular and maintainable**\
✔ **Separates logic from routes**

***

## **6. Error Handling in Express.js**

1. Handles unexpected errors gracefully.
2. Starts with err as first parameter and runs at end in request and response cycle.

#### **Example: Global Error Handler**

```javascript
app.use((req, res, next) => {
    res.status(404).json({ error: 'Route not found' });
});

app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong' });
});
```

✔ **404 Handler** - For unknown routes\
✔ **Error Middleware** - Catches server errors

***

## **7. Folder Structure for an Express.js Project**

A clean folder structure improves **code organization & scalability**.

📁 **my-express-app/**\
├── 📁 `controllers/` → Business logic\
│ ├── `userController.js`\
├── 📁 `routes/` → Route definitions\
│ ├── `userRoutes.js`\
├── 📁 `middlewares/` → Custom middleware\
│ ├── `authMiddleware.js`\
├── 📁 `models/` → Database models (if using a DB)\
│ ├── `userModel.js`\
├── 📁 `config/` → Configuration settings\
│ ├── `db.js`\
├── 📁 `public/` → Static files (CSS, images)\
├── `index.js` → Main entry file\
├── `package.json` → Project metadata

✔ **Controllers** → Handle logic\
✔ **Routes** → Define endpoints\
✔ **Middlewares** → Process requests\
✔ **Models** → Manage data (if using a database)

***

## **8. Conclusion**

✔ **Express.js makes backend development easy**\
✔ **Routes** handle incoming requests\
✔ **Middleware** modifies requests before they reach routes\
✔ **Controllers** separate logic from routes\
✔ **Error Handling** ensures smooth app behavior\
✔ **Folder structure** keeps code clean & maintainable
