# Error Handling

### **Table of Contents**

1. **Introduction to Error Handling**
2. **Using Try-Catch for Error Handling**
3. **Throwing Custom Errors**
4. **Express Error Middleware**
5. **Handling Async Errors with Express**
6. **Centralized Error Handling in Express**
7. **Conclusion**

***

### **1. Introduction to Error Handling**

Error handling is important in any application to:\
✔ Prevent the app from crashing.\
✔ Show meaningful error messages to users.\
✔ Log errors for debugging.

In **Node.js & Express**, errors can occur due to:

* **Invalid user input**
* **Database connection issues**
* **Server failures**
* **Unhandled promises**

***

### **2. Using Try-Catch for Error Handling**

The `try-catch` block helps **catch errors** inside a function.

#### **Example: Using Try-Catch**

```javascript
function divideNumbers(a, b) {
    try {
        if (b === 0) throw new Error("Cannot divide by zero!");
        return a / b;
    } catch (error) {
        console.error("Error:", error.message);
    }
}

console.log(divideNumbers(10, 2));  // Output: 5
console.log(divideNumbers(10, 0));  // Output: Error: Cannot divide by zero!
```

**How it works?**\
✔ If `b === 0`, an error is thrown.\
✔ The `catch` block **handles the error** instead of crashing the app.

***

### **3. Throwing Custom Errors**

We can create **custom error messages** for better debugging.

#### **Example: Custom Error Handling**

```javascript
function getUserData(userId) {
    try {
        if (!userId) throw new Error("User ID is required!");
        // Simulating a database fetch
        return { id: userId, name: "John Doe" };
    } catch (error) {
        console.error("Error:", error.message);
    }
}

console.log(getUserData(123));  // Output: { id: 123, name: 'John Doe' }
console.log(getUserData());     // Output: Error: User ID is required!
```

**Why use custom errors?**\
✔ Provides clear error messages.\
✔ Helps in debugging issues faster.

***

### **4. Express Error Middleware**

In **Express**, we use **middleware** to handle errors globally.

#### **Example: Basic Express App with Error Middleware**

```javascript
const express = require("express");
const app = express();

// Normal route
app.get("/", (req, res) => {
    res.send("Hello World");
});

// Route that throws an error
app.get("/error", (req, res, next) => {
    next(new Error("Something went wrong!"));
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.message);
    res.status(500).json({ error: err.message });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

**How it works?**\
✔ Any error in a route is passed to `next(error)`.\
✔ The error middleware **catches the error** and **sends a response**.\
✔ The app **does not crash** on errors.

***

### **5. Handling Async Errors with Express**

When using `async` functions, errors may not be caught by `try-catch`.

#### **Problem: Async Error Without Handling**

```javascript
app.get("/async-error", async (req, res) => {
    throw new Error("Async Error Occurred!");
});
```

❌ **This will crash the server!**

#### **Solution: Use Try-Catch in Async Functions**

```javascript
app.get("/async-error", async (req, res, next) => {
    try {
        throw new Error("Async Error Occurred!");
    } catch (error) {
        next(error);
    }
});
```

✔ Now the error **goes to the error middleware** safely.

***

### **6. Centralized Error Handling in Express**

A **better way** to handle errors in Express is by creating a custom **Error Class** and a central **error handler**.

#### **Step 1: Create a Custom Error Class**

```javascript
class AppError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
    }
}
```

#### **Step 2: Use the Custom Error Class in Routes**

```javascript
app.get("/user", (req, res, next) => {
    try {
        throw new AppError("User not found!", 404);
    } catch (error) {
        next(error);
    }
});
```

#### **Step 3: Create a Central Error Middleware**

```javascript
app.use((err, req, res, next) => {
    const statusCode = err.statusCode || 500;
    res.status(statusCode).json({
        error: err.message || "Internal Server Error",
    });
});
```

**Why use centralized error handling?**\
✔ Makes the code **cleaner**.\
✔ Provides **consistent error responses**.\
✔ Helps in **debugging and logging**.

***

### **7. Conclusion**

✅ **Try-Catch** is useful for handling errors in normal functions.\
✅ Express **Error Middleware** catches errors globally.\
✅ **Async errors** should be passed to `next(error)`.\
✅ **Custom error classes** make error handling **more structured**.\
✅ **Centralized error handling** improves app stability.
