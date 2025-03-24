# Async Programming

### **Table of Contents**

**Introduction to Non-Blocking I/O & Asynchronous Programming**

1. **What is Non-Blocking I/O?**
   * Blocking vs Non-Blocking I/O
   * Real-Life Example
2. **What is Asynchronous Programming?**
   * Synchronous vs Asynchronous Programming
   * Real-Life Example
3. **How Non-Blocking & Asynchronous Work Together in Node.js**
4. **Example Code in Node.js**
5. **Execution Flow of Code**
6. **Key Takeaways**

***

## **1. Introduction to Non-Blocking I/O & Asynchronous Programming**

Node.js is known for its **high performance and efficiency** in handling multiple requests at the same time. This is possible because of two key concepts:

* **Non-Blocking I/O**: Node.js does not wait for one task to finish before moving to the next.
* **Asynchronous Programming**: Tasks that take time (e.g., reading files, API calls) are handled in the background while the program keeps running other tasks.

***

## **2. What is Non-Blocking I/O?**

**I/O (Input/Output)** operations include reading files, fetching data from a database, or making API calls. Normally, these operations take time.

#### **Blocking vs Non-Blocking I/O**

| Feature        | Blocking I/O (Traditional)                             | Non-Blocking I/O (Node.js)                          |
| -------------- | ------------------------------------------------------ | --------------------------------------------------- |
| Execution Flow | Waits for one task to finish before moving to the next | Moves to the next task immediately                  |
| Efficiency     | Slower, as it waits for I/O tasks to complete          | Faster, as it does not wait                         |
| Example Task   | Reading a file and waiting until it's fully read       | Reading a file but doing other things while waiting |

#### **Real-Life Example: Ordering Food**

* **Blocking I/O**: You go to a restaurant, order food, and **wait at the counter** until it's ready.
* **Non-Blocking I/O**: You order food, get a token, and **sit at your table** while the food is prepared. You do other things in the meantime.

***

## **3. What is Asynchronous Programming?**

Asynchronous programming means that a program **does not wait** for tasks to finish. Instead, it runs them in the background and executes the next tasks immediately.

#### **Synchronous vs Asynchronous Programming**

| Feature     | Synchronous                              | Asynchronous                         |
| ----------- | ---------------------------------------- | ------------------------------------ |
| Execution   | One task at a time, waits for completion | Multiple tasks run in parallel       |
| Performance | Slow, as it waits                        | Fast, as tasks don’t block execution |
| Example     | Cooking one dish at a time               | Cooking multiple dishes at once      |

#### **Real-Life Example: Cooking Dinner**

* **Synchronous Cooking**: You boil rice, wait for it to finish, and then start frying vegetables.
* **Asynchronous Cooking**: You boil rice **while** frying vegetables, so both finish faster.

***

## **4. How Non-Blocking & Asynchronous Work Together in Node.js**

In Node.js:

* Non-blocking I/O **ensures that I/O operations do not block execution**.
* Asynchronous programming **allows tasks to run in the background** while the main program continues.
* This is done using **callbacks, Promises, and async/await**.

***

## **5. Example Code in Node.js**

#### **Blocking (Synchronous) Code**

```javascript
const fs = require("fs");

// Reading file synchronously (Blocking)
const data = fs.readFileSync("file.txt", "utf8");
console.log(data);

console.log("This runs after the file is read");
```

#### **Output:**

```
(File content)
This runs after the file is read
```

**Issue:** The program **waits** for the file to be read before continuing.

***

#### **Non-Blocking (Asynchronous) Code**

```javascript
const fs = require("fs");

// Reading file asynchronously (Non-blocking)
fs.readFile("file.txt", "utf8", (err, data) => {
    console.log(data);
});

console.log("This runs immediately!");
```

#### **Output:**

```
This runs immediately!
(File content)
```

**Why?**

* **fs.readFile()** runs in the background, allowing the program to continue executing other code.
* Once the file is read, the callback function prints the content.

***

## **6. Execution Flow of Code**

1️⃣ **Synchronous Code**

* Reads file **before moving to the next task** (blocking).

2️⃣ **Asynchronous Code**

* Starts reading file but **moves to the next task immediately** (non-blocking).
* When the file read completes, Node.js calls the callback function.

***

## **7. Key Takeaways**

✔ Non-blocking I/O allows Node.js to handle multiple tasks at once.\
✔ Asynchronous programming ensures efficient execution of tasks.\
✔ Node.js uses Callbacks, Promises, and Async/Await to handle asynchronous tasks.\
✔ Using non-blocking code improves performance and responsiveness.
