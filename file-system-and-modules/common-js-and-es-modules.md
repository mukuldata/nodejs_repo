# Common JS and ES Modules

### **Table of Contents**

1. **Introduction to Modules in Node.js**
2. **CommonJS (require & module.exports)**
3. **ES Modules (import & export)**
4. **Key Differences Between CommonJS and ES Modules**
5. **When to Use CommonJS vs ES Modules**
6. **Conclusion**

***

## **1. Introduction to Modules in Node.js**

Modules allow us to split our code into separate files for **better organization and reusability**.\
There are **two main module systems** in Node.js:

* **CommonJS (CJS)** – Uses `require()` and `module.exports`.
* **ES Modules (ESM)** – Uses `import` and `export`.

***

## **2. CommonJS (CJS) – require() and module.exports**

CommonJS is the **default** module system in Node.js (before ES6).\
It is **synchronous** and works well in **server-side** applications.

#### **Example: Using CommonJS Modules**

**1️⃣ Create a module (math.js)**

```javascript
// math.js
function add(a, b) {
    return a + b;
}

module.exports = add; // Export function
```

**2️⃣ Import and use it in another file (app.js)**

```javascript
// app.js
const add = require('./math'); // Import module
console.log(add(5, 3)); // Output: 8
```

✔ **Uses `require()` to import**.\
✔ **Uses `module.exports` to export**.

***

## **3. ES Modules (ESM) – import and export**

ES Modules is the **modern** module system, introduced in **ES6 (ECMAScript 2015)**.\
It is **asynchronous** and works well for **both server-side and browser environments**.

#### **Example: Using ES Modules**

**1️⃣ Create a module (math.mjs)**

```javascript
// math.mjs
export function add(a, b) {
    return a + b;
}
```

**2️⃣ Import and use it in another file (app.mjs)**

```javascript
// app.mjs
import { add } from './math.mjs'; // Import module
console.log(add(5, 3)); // Output: 8
```

✔ **Uses `import` to import**.\
✔ **Uses `export` to export**.\
✔ **Uses `.mjs` file extension in Node.js (or `"type": "module"` in package.json)**.

***

## **4. Key Differences Between CommonJS and ES Modules**

| Feature                | CommonJS (CJS)                 | ES Modules (ESM)              |
| ---------------------- | ------------------------------ | ----------------------------- |
| **Syntax**             | `require()` / `module.exports` | `import` / `export`           |
| **Execution**          | **Synchronous**                | **Asynchronous**              |
| **File Type**          | `.js` (default)                | `.mjs` or `"type": "module"`  |
| **Where Used?**        | **Node.js only**               | **Browser & Node.js**         |
| **Importing Behavior** | **Loads entire module**        | **Can import specific parts** |

***

## **5. When to Use CommonJS vs ES Modules**

✔ **Use CommonJS (CJS)** when:

* You are working on an **older Node.js project**.
* You need **synchronous** imports.
* You are using **many npm packages** that still rely on CommonJS.

✔ **Use ES Modules (ESM)** when:

* You are working on a **modern Node.js project**.
* You need to **import/export selectively**.
* You are writing **universal (browser + Node.js) JavaScript**.

#### **How to Enable ES Modules in Node.js?**

1. **Use `.mjs` file extension**\
   OR
2. **Set `"type": "module"` in package.json**

```json
{
  "type": "module"
}
```

***

## **6. Conclusion**

✔ **CommonJS (`require()` & `module.exports`)** → Used in traditional Node.js applications.\
✔ **ES Modules (`import` & `export`)** → Used in modern JavaScript and browsers.\
✔ **Key differences** → CJS is synchronous, ESM is asynchronous.\
✔ **Choose based on project requirements**.
