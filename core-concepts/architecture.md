# Architecture

### **Table of Contents**

1. **Introduction to Node.js Architecture**
2. **Key Components of Node.js Architecture**
   * Call Stack
   * Node.js APIs & Background Tasks
   * Event Loop
   * Callback Queue
   * Microtask Queue (Promise Queue)
3. **How Node.js Processes Tasks**
4. **Example: Understanding with a Real-Life Scenario**
5. **Example Code in Node.js**
6. **Execution Flow of Code**
7. **Final Output Explanation**
8. **Key Takeaways**

***

## **1. Introduction to Node.js Architecture**

1. Node.js follows an **event-driven, non-blocking architecture**, making it highly efficient for handling multiple tasks without waiting for one to complete.&#x20;
2. This is achieved using **the Event Loop**, which continuously checks for pending tasks and executes them asynchronously.

***

## **2. Key Components of Node.js Architecture**

### **Call Stack**

* The Call Stack is where **synchronous** functions are executed.
* It follows **Last In, First Out (LIFO)**—the last function added is executed first.
* If a function takes time, it **blocks** execution unless handled asynchronously.

#### **Example:**

```javascript
function greet() {
    console.log("Hello, World!");
}
greet();
console.log("This runs after greet()");
```

#### **Output:**

```
Hello, World!
This runs after greet()
```

***

### **Node.js APIs & Background Tasks**

* Some functions (like reading files or making API requests) are sent to the **background** so they don’t block the main thread.
* These tasks are handled by **Node.js APIs**, allowing other code to continue running.

#### **Example:**

```javascript
const fs = require("fs");
fs.readFile("file.txt", "utf8", (err, data) => {
    console.log("File Read Complete");
});
console.log("Reading File...");
```

#### **Output:**

```
Reading File...
File Read Complete
```

The file read happens in the **background**, so "Reading File..." is printed first.

***

### **Event Loop**

* The **heart of Node.js**, which checks if the **Call Stack is empty** and moves tasks from the **Callback Queue**.
* It ensures that **non-blocking** tasks run efficiently.

***

### **Callback Queue**

* Stores **asynchronous callbacks** waiting to be executed.
* The **Event Loop** moves these tasks to the Call Stack when it is empty.

***

### **Microtask Queue (Promise Queue)**

* Contains **Promises and process.nextTick()** callbacks.
* **Higher priority than the Callback Queue**.
* Executes **before** setTimeout and other async functions.

#### **Example:**

```javascript
setTimeout(() => console.log("setTimeout"), 0);
Promise.resolve().then(() => console.log("Promise resolved"));
console.log("Synchronous code");
```

#### **Output:**

```
Synchronous code
Promise resolved
setTimeout
```

**Why?**

1. **Synchronous code** runs first.
2. **Promise (Microtask Queue) runs before setTimeout (Callback Queue).**

***

## **3. How Node.js Processes Tasks**

#### **Step-by-Step Execution:**

1. Runs **synchronous code** in the **Call Stack**.
2. Sends **async tasks** (file read, API calls) to the **background**.
3. Adds **callbacks** to the **Callback Queue** when complete.
4. **Event Loop** moves tasks from **queues** to the **Call Stack** when empty.

***

## **4. Example: Understanding with a Real-Life Scenario**

Imagine you are **making tea**:

1. **Turn on the stove** (Synchronous Task ✅).
2. **Boil water** (Async Task, runs in the background 🚀).
3. **Prepare tea leaves and sugar** (Other synchronous tasks keep running ✅).
4. **Water boils, beeps** (Event Loop picks callback from queue).
5. **Make tea** (Callback function executes).

***

## **5. Example Code in Node.js**

```javascript
console.log("1: Start Cooking");

setTimeout(() => {
    console.log("2: Water is Boiling (Async Task)");
}, 3000);

console.log("3: Preparing Bread and Butter");

Promise.resolve().then(() => console.log("4: Making Tea with Boiled Water (Microtask)"));

console.log("5: Breakfast is Almost Ready!");
```

***

## **6. Execution Flow of Code**

1️⃣ **Synchronous tasks execute first (Call Stack)**

* `"1: Start Cooking"`
* `"3: Preparing Bread and Butter"`
* `"5: Breakfast is Almost Ready!"`

2️⃣ **Microtask Queue executes next (Promises)**

* `"4: Making Tea with Boiled Water (Microtask)"`

3️⃣ **Asynchronous tasks execute last (Callback Queue - setTimeout)**

* `"2: Water is Boiling (Async Task)"` (After 3 seconds)

***

## **7. Final Output Explanation**

```
1: Start Cooking
3: Preparing Bread and Butter
5: Breakfast is Almost Ready!
4: Making Tea with Boiled Water (Microtask)
2: Water is Boiling (Async Task)
```

* **setTimeout() executes last** (because it is in the Callback Queue).
* **Promise executes before setTimeout** (because Microtasks have higher priority).

***

## **8. Key Takeaways**

✔ Node.js uses non-blocking architecture to handle multiple tasks efficiently.\
✔ Event Loop ensures the Call Stack is never blocked by moving async tasks.\
✔ Microtasks (Promises) execute before Callback Queue tasks (setTimeout, API responses).\
✔ Understanding Call Stack, Event Loop, and Queues is key to writing efficient Node.js applications.
