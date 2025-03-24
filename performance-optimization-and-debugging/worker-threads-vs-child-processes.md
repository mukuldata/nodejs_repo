# Worker Threads vs Child Processes

### **Table of Contents**

1. **Introduction to Worker Threads & Child Processes**
2. **Understanding Worker Threads**
   * When to Use Worker Threads?
   * Example of Worker Threads
3. **Understanding Child Processes**
   * When to Use Child Processes?
   * Example of Child Processes
4. **Fork vs Spawn: Understanding the Differences**
   * What is `fork()`?
   * What is `spawn()`?
   * Difference Between `fork()` and `spawn()`
5. **Comparison: Worker Threads vs Child Processes**
6. **Conclusion**

***

### **1. Introduction to Worker Threads & Child Processes**

Node.js is **single-threaded**, meaning it can handle only one task at a time. However, for CPU-intensive tasks, we need **parallel processing**. Node.js provides two ways to achieve this:

* **Worker Threads** (Used for parallel execution in the same process)
* **Child Processes** (Used for running separate processes)

***

### **2. Understanding Worker Threads**

The **Worker Threads** module in Node.js allows running **JavaScript code in parallel** inside a separate thread **without creating a new process**.

#### **When to Use Worker Threads?**

✅ Best for **CPU-heavy tasks** (e.g., image processing, large computations)\
✅ Useful when **sharing memory between threads**

***

#### **Example of Worker Threads**

**Step 1: Install Worker Threads (for older Node.js versions)**

```bash
npm install worker_threads
```

**Step 2: Create a Worker Thread (`worker.js`)**

```javascript
const { parentPort } = require('worker_threads');

parentPort.on('message', (data) => {
  let sum = 0;
  for (let i = 0; i < data; i++) {
    sum += i;
  }
  parentPort.postMessage(sum);
});
```

**Step 3: Use Worker Thread in Main File (`main.js`)**

```javascript
const { Worker } = require('worker_threads');

const worker = new Worker('./worker.js');

worker.postMessage(1000000); // Send data to worker thread

worker.on('message', (result) => {
  console.log(`Sum calculated by worker: ${result}`);
});
```

📌 **How it works:**

* `worker.js` runs in **a separate thread**
* The main process sends a message to the worker
* The worker processes data and sends back the result

***

### **3. Understanding Child Processes**

The **Child Process** module allows Node.js to **execute external processes** by spawning separate system processes.

#### **When to Use Child Processes?**

✅ Running **external scripts or commands** (e.g., Python, shell scripts)\
✅ Performing **I/O-heavy tasks** (e.g., file processing, database queries)

***

#### **Example of Child Processes**

**Using `exec()` to Run a Shell Command**

```javascript
const { exec } = require('child_process');

exec('ls', (error, stdout, stderr) => {
  if (error) {
    console.error(`Error: ${error.message}`);
    return;
  }
  console.log(`Output:\n${stdout}`);
});
```

📌 **How it works:**

* Executes the `ls` command to list files
* Returns output in the callback

***

### **4. Fork vs Spawn: Understanding the Differences**

Node.js provides two common methods to create child processes:

| Method    | Creates a New Process? | Shares Memory? | When to Use?                                         |
| --------- | ---------------------- | -------------- | ---------------------------------------------------- |
| `fork()`  | Yes                    | Yes (via IPC)  | When you need communication between parent and child |
| `spawn()` | Yes                    | No             | When running an external command or script           |

***

#### **What is `fork()`?**

The `fork()` method creates a **new child process** that runs a separate instance of the Node.js engine but can **communicate with the parent**.

**Example of `fork()`**

```javascript
const { fork } = require('child_process');

const child = fork('./child.js');

child.send('Hello, child process!');

child.on('message', (message) => {
  console.log(`Message from child: ${message}`);
});
```

📌 **How it works:**

* Creates a new Node.js process
* Allows **message passing** between parent and child

***

#### **What is `spawn()`?**

The `spawn()` method runs a **new process** for external commands without **sharing memory**.

**Example of `spawn()`**

```javascript
const { spawn } = require('child_process');

const child = spawn('node', ['script.js']);

child.stdout.on('data', (data) => {
  console.log(`Output: ${data}`);
});
```

📌 **How it works:**

* Runs the `script.js` file in a new process
* Captures the output stream

***

### **5. Comparison: Worker Threads vs Child Processes**

| Feature     | Worker Threads                     | Child Processes                       |
| ----------- | ---------------------------------- | ------------------------------------- |
| Execution   | Runs in same process               | Runs as a separate process            |
| Memory      | Shared with main thread            | Separate memory                       |
| Performance | Faster (low overhead)              | Slower (process creation overhead)    |
| Best for    | **CPU-bound tasks** (calculations) | **I/O-heavy tasks** (file processing) |

***

### **6. Conclusion**

✔ Use **Worker Threads** for **CPU-intensive tasks** that require **shared memory**\
✔ Use **Child Processes** for **running external commands** and **separate processing**\
✔ Use **`fork()`** for **Node.js inter-process communication**\
✔ Use **`spawn()`** for **executing system commands**

**Final Thought:** If you need **parallel execution** inside the same process, use **Worker Threads**. If you need to **run independent tasks**, use **Child Processes**.
