# Mocking and Stubbing

### **Table of Contents**

1. **Introduction to Testing**
2. **What is Sinon.js?**
3. **Understanding Mocks, Stubs, and Spies**
4. **Setting Up Sinon.js in a Node.js Project**
5. **Using Sinon.js for Spying**
6. **Using Sinon.js for Stubbing**
7. **Using Sinon.js for Mocking**
8. **Testing Asynchronous Functions with Sinon.js**
9. **Combining Sinon.js with Mocha & Chai**
10. **Conclusion**

***

### **1. Introduction to Testing**

When testing JavaScript applications, we often need to **simulate function calls, replace dependencies, and control external behavior** (like API calls or database interactions). This is where **Mocking & Stubbing** come into play.

✅ **Why use Mocks & Stubs?**\
✔ Helps test components **in isolation**.\
✔ Avoids calling **real APIs** or **databases**.\
✔ Makes tests **faster and reliable**.

***

### **2. What is Sinon.js?**

**Sinon.js** is a powerful testing library that provides:

* **Spies** – Monitor function calls.
* **Stubs** – Replace a function with a controlled version.
* **Mocks** – Fake objects with pre-defined expectations.

✅ **Why Sinon.js?**\
✔ Works with **Mocha, Jest, and Chai**.\
✔ Supports **async testing**.\
✔ Helps control **external dependencies** (like APIs, databases, or timers).

***

### **3. Understanding Mocks, Stubs, and Spies**

| Feature  | Purpose                                              | Example Use Case                             |
| -------- | ---------------------------------------------------- | -------------------------------------------- |
| **Spy**  | Observes function calls but doesn't change behavior. | Check if a function was called.              |
| **Stub** | Replaces a function with custom behavior.            | Replace an API call with fake data.          |
| **Mock** | A fake object with pre-defined expectations.         | Ensure a function is called exactly 3 times. |

***

### **4. Setting Up Sinon.js in a Node.js Project**

#### **Step 1: Install Dependencies**

Run the following command:

```sh
npm install --save-dev sinon mocha chai
```

***

### **5. Using Sinon.js for Spying**

A **spy** tracks how a function is used (how many times it's called, with what arguments, etc.).

📄 **spy.test.js**

```javascript
const sinon = require("sinon");
const { expect } = require("chai");

describe("Sinon Spy Example", () => {
    it("should track function calls", () => {
        const myFunction = sinon.spy();

        myFunction("Hello");
        myFunction("World");

        expect(myFunction.calledTwice).to.be.true;
        expect(myFunction.firstCall.args[0]).to.equal("Hello");
        expect(myFunction.secondCall.args[0]).to.equal("World");
    });
});
```

✅ **What this does:**\
✔ Tracks how many times `myFunction` was called.\
✔ Checks the arguments passed to it.

Run the test:

```sh
mocha spy.test.js
```

***

### **6. Using Sinon.js for Stubbing**

A **stub** replaces a function with a fake version that returns a custom response.

📄 **stub.test.js**

```javascript
const sinon = require("sinon");
const { expect } = require("chai");

function getUser(id, callback) {
    // Simulate fetching user from DB (real implementation)
    setTimeout(() => callback({ id, name: "John Doe" }), 1000);
}

describe("Sinon Stub Example", () => {
    it("should return fake user", () => {
        const callback = sinon.stub().returns({ id: 1, name: "Fake User" });

        getUser(1, callback);

        expect(callback.calledOnce).to.be.true;
        expect(callback.returnValues[0].name).to.equal("Fake User");
    });
});
```

✅ **What this does:**\
✔ Replaces `getUser` function with a stub.\
✔ Ensures `callback` is called with **fake data** instead of fetching from a real database.

Run the test:

```sh
mocha stub.test.js
```

***

### **7. Using Sinon.js for Mocking**

A **mock** is a fake object that expects certain behavior (like **calling a function exactly 3 times**).

📄 **mock.test.js**

```javascript
const sinon = require("sinon");
const { expect } = require("chai");

describe("Sinon Mock Example", () => {
    it("should expect function to be called once", () => {
        const user = {
            save: () => console.log("Saving user..."),
        };

        const mock = sinon.mock(user);
        mock.expects("save").once();

        user.save(); // Simulate calling the function

        mock.verify(); // Check if expectations were met
    });
});
```

✅ **What this does:**\
✔ Ensures `user.save()` is called **exactly once**.\
✔ Fails the test if `save()` is **never called** or **called multiple times**.

Run the test:

```sh
mocha mock.test.js
```

***

### **8. Testing Asynchronous Functions with Sinon.js**

📄 **async.test.js**

```javascript
const sinon = require("sinon");
const { expect } = require("chai");

function fetchData(callback) {
    setTimeout(() => {
        callback("Success");
    }, 1000);
}

describe("Testing async function with Sinon.js", () => {
    it("should call callback with 'Success'", (done) => {
        const callback = sinon.spy();

        fetchData(callback);

        setTimeout(() => {
            expect(callback.calledOnce).to.be.true;
            expect(callback.calledWith("Success")).to.be.true;
            done();
        }, 1100);
    });
});
```

✅ **What this does:**\
✔ Uses `setTimeout` to test an **async function**.\
✔ Uses a **spy** to track the callback call.

Run the test:

```sh
mocha async.test.js
```

***

### **9. Combining Sinon.js with Mocha & Chai**

#### **Step 1: Install Dependencies**

```sh
npm install --save-dev mocha chai sinon
```

#### **Step 2: Create a Simple Express API**

📄 **server.js**

```javascript
const express = require("express");
const app = express();

app.get("/hello", (req, res) => {
    res.json({ message: "Hello, World!" });
});

module.exports = app;
```

#### **Step 3: Write Tests with Sinon.js, Mocha, and Chai**

📄 **server.test.js**

```javascript
const request = require("supertest");
const app = require("./server");
const sinon = require("sinon");
const { expect } = require("chai");

describe("API Test with Sinon.js", () => {
    it("should return Hello, World!", async () => {
        const res = await request(app).get("/hello");

        expect(res.status).to.equal(200);
        expect(res.body.message).to.equal("Hello, World!");
    });

    it("should spy on console.log", () => {
        const logSpy = sinon.spy(console, "log");

        console.log("Test message");

        expect(logSpy.calledOnce).to.be.true;
        expect(logSpy.calledWith("Test message")).to.be.true;

        logSpy.restore();
    });
});
```

#### **Step 4: Run Tests**

```sh
mocha server.test.js
```

***

### **10. Conclusion**

✅ **Spies** track function calls.\
✅ **Stubs** replace real functions with fake responses.\
✅ **Mocks** enforce behavior expectations.\
✅ Works with **Mocha, Jest, and Chai**.\
✅ Helps test **API calls, databases, and authentication logic**.

By using **Sinon.js**, developers can **write better tests** and **ensure reliable applications**.&#x20;
