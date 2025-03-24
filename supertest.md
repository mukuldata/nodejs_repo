# Supertest

### **Table of Contents**

1. **Introduction to API Testing**
2. **What is Supertest?**
3. **Setting Up Supertest in a Node.js Project**
4. **Creating a Sample Express API**
5. **Writing API Tests with Supertest & Jest**
6. **Writing API Tests with Supertest & Mocha**
7. **Handling Authentication in API Testing**
8. **Running Tests & Viewing Results**
9. **Conclusion**

***

### **1. Introduction to API Testing**

API Testing is the process of testing **RESTful APIs** to check if they return the expected responses. It helps ensure that the **backend** is working correctly.

✅ **Why API Testing?**\
✔ Ensures correct API responses.\
✔ Helps detect bugs early.\
✔ Validates API performance.\
✔ Checks security aspects (authentication, authorization).

***

### **2. What is Supertest?**

**Supertest** is a **JavaScript testing library** used to test **REST APIs** in **Node.js applications**. It allows sending **HTTP requests** (GET, POST, PUT, DELETE) to APIs and checking the responses.

✅ **Features of Supertest**\
✔ Simple syntax for API requests.\
✔ Supports integration with **Jest** or **Mocha**.\
✔ Works with Express and other frameworks.\
✔ Can handle **authentication testing**.

***

### **3. Setting Up Supertest in a Node.js Project**

#### **Step 1: Install Supertest & Testing Framework**

In a Node.js project, install **Supertest** along with a testing library:

```sh
npm install --save-dev supertest jest
```

or

```sh
npm install --save-dev supertest mocha chai
```

#### **Step 2: Update `package.json`**

Modify the `"scripts"` section to run tests:

For **Jest**:

```json
"scripts": {
    "test": "jest"
}
```

For **Mocha**:

```json
"scripts": {
    "test": "mocha"
}
```

***

### **4. Creating a Sample Express API**

📄 **server.js**

```javascript
const express = require("express");
const app = express();

app.use(express.json());

app.get("/api/hello", (req, res) => {
    res.json({ message: "Hello, World!" });
});

app.post("/api/data", (req, res) => {
    res.json({ success: true, data: req.body });
});

module.exports = app;
```

📄 **index.js (to start the server)**

```javascript
const app = require("./server");

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

***

### **5. Writing API Tests with Supertest & Jest**

📄 **server.test.js**

```javascript
const request = require("supertest");
const app = require("./server");

describe("API Tests", () => {
    it("should return Hello, World!", async () => {
        const res = await request(app).get("/api/hello");
        expect(res.status).toBe(200);
        expect(res.body.message).toBe("Hello, World!");
    });

    it("should send data successfully", async () => {
        const res = await request(app)
            .post("/api/data")
            .send({ name: "John Doe" });
        
        expect(res.status).toBe(200);
        expect(res.body.success).toBe(true);
        expect(res.body.data.name).toBe("John Doe");
    });
});
```

#### **Run Jest Tests**

```sh
npm test
```

✅ **Expected Output**:

```
PASS  ./server.test.js
✓ should return Hello, World! (10ms)
✓ should send data successfully (15ms)
```

***

### **6. Writing API Tests with Supertest & Mocha**

📄 **server.mocha.test.js**

```javascript
const request = require("supertest");
const app = require("./server");
const { expect } = require("chai");

describe("API Tests with Mocha", () => {
    it("should return Hello, World!", async () => {
        const res = await request(app).get("/api/hello");
        expect(res.status).to.equal(200);
        expect(res.body.message).to.equal("Hello, World!");
    });

    it("should send data successfully", async () => {
        const res = await request(app)
            .post("/api/data")
            .send({ name: "John Doe" });

        expect(res.status).to.equal(200);
        expect(res.body.success).to.be.true;
        expect(res.body.data.name).to.equal("John Doe");
    });
});
```

#### **Run Mocha Tests**

```sh
npm test
```

✅ **Expected Output**:

```
API Tests with Mocha
  ✓ should return Hello, World!
  ✓ should send data successfully
```

***

### **7. Handling Authentication in API Testing**

Many APIs require **JWT authentication**. Here’s how to test protected routes.

📄 **server.js (Protected API Route)**

```javascript
const express = require("express");
const jwt = require("jsonwebtoken");
const app = express();

app.use(express.json());

const SECRET_KEY = "mysecretkey";

app.post("/api/login", (req, res) => {
    const token = jwt.sign({ user: "John Doe" }, SECRET_KEY, { expiresIn: "1h" });
    res.json({ token });
});

app.get("/api/protected", (req, res) => {
    const token = req.headers.authorization?.split(" ")[1];
    if (!token) return res.status(401).json({ message: "Access Denied" });

    try {
        jwt.verify(token, SECRET_KEY);
        res.json({ message: "Protected data" });
    } catch {
        res.status(401).json({ message: "Invalid Token" });
    }
});

module.exports = app;
```

📄 **server.test.js (Testing Authentication with Jest & Supertest)**

```javascript
const request = require("supertest");
const app = require("./server");

describe("Auth API Tests", () => {
    let token = "";

    it("should login and receive a token", async () => {
        const res = await request(app).post("/api/login");
        expect(res.status).toBe(200);
        expect(res.body.token).toBeDefined();
        token = res.body.token;
    });

    it("should access protected route with valid token", async () => {
        const res = await request(app)
            .get("/api/protected")
            .set("Authorization", `Bearer ${token}`);

        expect(res.status).toBe(200);
        expect(res.body.message).toBe("Protected data");
    });

    it("should deny access without a token", async () => {
        const res = await request(app).get("/api/protected");
        expect(res.status).toBe(401);
        expect(res.body.message).toBe("Access Denied");
    });
});
```

✅ **This test checks:**\
✔ **Login API returns a token**\
✔ **Protected route allows access with a valid token**\
✔ **Protected route denies access without a token**

***

### **8. Running Tests & Viewing Results**

Run tests using:

```sh
npm test
```

✅ **Example Output:**

```
PASS  ./server.test.js
✓ should return Hello, World! 
✓ should send data successfully 
✓ should login and receive a token 
✓ should access protected route with valid token
✓ should deny access without a token
```

***

### **9. Conclusion**

✅ **Supertest** simplifies API testing.\
✅ Works well with **Jest & Mocha**.\
✅ Supports **authentication testing**.\
✅ Ensures API correctness before deployment.

Using **Supertest**, developers can **automate API testing** and prevent **unexpected issues** in production.&#x20;
