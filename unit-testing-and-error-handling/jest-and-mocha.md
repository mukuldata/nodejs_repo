# Jest and Mocha

### **Table of Contents**

1. **Introduction to Unit Testing**&#x20;
2. **What is Jest?**
3. **What is Mocha?**
4. **Setting Up Jest for Unit Testing**
5. **Writing Unit Tests with Jest**
6. **Setting Up Mocha for Unit Testing**
7. **Writing Unit Tests with Mocha & Chai**
8. **Mocking Functions in Jest & Mocha**
9. **Running Tests & Generating Reports**
10. **Conclusion**

***

### **1. Introduction to Unit Testing**

Unit testing is a **software testing method** where individual functions or components are tested **separately** to ensure they work as expected.

✅ **Why Unit Testing?**\
✔ Detects bugs early.\
✔ Ensures code reliability.\
✔ Helps with refactoring code safely.\
✔ Improves development speed.

***

### **2. What is Jest?**

**Jest** is a **JavaScript testing framework** developed by Facebook. It is mainly used for testing **React** and **Node.js** applications.

✅ **Features of Jest**\
✔ Fast and simple.\
✔ Comes with a built-in assertion library.\
✔ Supports **mocks** and **spies** for function testing.\
✔ Has built-in **code coverage reporting**.

***

### **3. What is Mocha?**

**Mocha** is a **flexible testing framework** that runs tests in **Node.js** and the browser.

✅ **Features of Mocha**\
✔ Allows writing tests in a structured way.\
✔ Supports **asynchronous testing**.\
✔ Works well with **Chai**, an assertion library.\
✔ Provides detailed error reports.

***

### **4. Setting Up Jest for Unit Testing**

To use Jest in a Node.js project:

#### **Step 1: Install Jest**

Run the following command:

```sh
npm init -y   # Initialize a Node.js project (if not already done)
npm install --save-dev jest
```

#### **Step 2: Update `package.json`**

Modify the `"scripts"` section:

```json
"scripts": {
    "test": "jest"
}
```

***

### **5. Writing Unit Tests with Jest**

#### **Example: Testing a Math Function**

📄 **math.js** (Function to Test)

```javascript
function add(a, b) {
    return a + b;
}

module.exports = add;
```

📄 **math.test.js** (Test File)

```javascript
const add = require("./math");

test("adds 2 + 3 to equal 5", () => {
    expect(add(2, 3)).toBe(5);
});
```

#### **Running the Jest Test**

```sh
npm test
```

✅ **Output**:

```
PASS  ./math.test.js
✓ adds 2 + 3 to equal 5
```

***

### **6. Setting Up Mocha for Unit Testing**

#### **Step 1: Install Mocha & Chai**

```sh
npm install --save-dev mocha chai
```

#### **Step 2: Update `package.json`**

```json
"scripts": {
    "test": "mocha"
}
```

***

### **7. Writing Unit Tests with Mocha & Chai**

📄 **math.js** (Function to Test)

```javascript
function multiply(a, b) {
    return a * b;
}

module.exports = multiply;
```

📄 **math.test.js** (Mocha Test File)

```javascript
const multiply = require("./math");
const { expect } = require("chai");

describe("Math Functions", () => {
    it("should multiply 2 and 3 to get 6", () => {
        expect(multiply(2, 3)).to.equal(6);
    });
});
```

#### **Running the Mocha Test**

```sh
npm test
```

✅ **Output**:

```
Math Functions
  ✓ should multiply 2 and 3 to get 6
```

***

### **8. Mocking Functions in Jest & Mocha**

Sometimes, we want to **mock** a function instead of calling it directly.

#### **Mocking in Jest**

```javascript
const fetchData = jest.fn(() => "Mocked Data");

test("fetchData should return mocked data", () => {
    expect(fetchData()).toBe("Mocked Data");
});
```

#### **Mocking in Mocha (Using Sinon)**

First, install **Sinon**:

```sh
npm install --save-dev sinon
```

Then, use it in your test:

```javascript
const sinon = require("sinon");

const myFunction = {
    fetchData: () => "Real Data"
};

const mock = sinon.stub(myFunction, "fetchData").returns("Mocked Data");

console.log(myFunction.fetchData());  // Output: Mocked Data
```

***

### **9. Running Tests & Generating Reports**

#### **Running Jest Tests with Coverage**

```sh
npm test -- --coverage
```

✅ This generates a **code coverage report** showing tested files.

#### **Running Mocha Tests**

```sh
npm test
```

***

### **10. Conclusion**

✅ **Jest** is a fast and simple testing framework.\
✅ **Mocha & Chai** provide flexible testing with custom assertions.\
✅ **Mocking** helps test functions without dependencies.\
✅ **Unit tests** ensure code reliability and prevent bugs.
