# Cluster Module

### **Table of Contents**

1. **What is the Cluster Module?**
2. **Why Use Clustering in Node.js?**
3. **How the Cluster Module Works**
4. **Example: Implementing Clustering in Express.js**
5. **Cluster Events and Handling Worker Processes**
6. **Limitations of Clustering**
7. **Conclusion**

***

### **1. What is the Cluster Module?**

1. Node.js runs **single-threaded**, meaning it can handle only **one task at a time**.
2. &#x20;But modern computers have **multiple CPU cores**.&#x20;
3. The **Cluster module** in Node.js allows us to take advantage of **multiple cores** by creating multiple worker processes that share the same server port.

***

### **2. Why Use Clustering in Node.js?**

By default, a Node.js application runs in a **single process** and can handle only one request at a time. Clustering helps by:\
✔ **Utilizing multiple CPU cores** to handle more requests\
✔ **Improving performance** by distributing the workload\
✔ **Ensuring high availability** (if one worker crashes, others keep running)

***

### **3. How the Cluster Module Works**

The Cluster module allows a Node.js application to:

1. **Create a master process** that manages worker processes
2. **Spawn multiple worker processes** (equal to CPU cores)
3. **Distribute incoming requests** among worker processes
4. **Restart workers** if they crash

Each worker runs independently but listens on the **same port**, managed by the master process.

***

### **4. Example: Implementing Clustering in Express.js**

#### **Step 1: Import Required Modules**

```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

const numCPUs = os.cpus().length; // Get number of CPU cores
```

#### **Step 2: Create a Master Process**

```javascript
if (cluster.isMaster) {
  console.log(`Master process ${process.pid} is running`);

  // Fork workers (equal to number of CPU cores)
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  // Listen for worker exit and restart if needed
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork();
  });

} else {
  // Worker processes handle requests
  const express = require('express');
  const app = express();

  app.get('/', (req, res) => {
    res.send(`Hello from Worker ${process.pid}`);
  });

  app.listen(3000, () => {
    console.log(`Worker ${process.pid} started`);
  });
}
```

#### **Step 3: Run the Server**

Start the Node.js application:

```bash
node index.js
```

#### **How It Works:**

* The **master process** forks multiple **worker processes**
* Each **worker** listens on **port 3000**
* Requests are **distributed** among available workers
* If a worker **crashes**, a new one is started

***

### **5. Cluster Events and Handling Worker Processes**

The **Cluster module** provides events to manage worker processes:

| Event                | Description                                |
| -------------------- | ------------------------------------------ |
| `cluster.fork()`     | Creates a new worker process               |
| `cluster.on('exit')` | Detects when a worker dies and restarts it |
| `worker.process.pid` | Gets the process ID of a worker            |

Example: Logging worker process creation:

```javascript
cluster.on('online', (worker) => {
  console.log(`Worker ${worker.process.pid} is online`);
});
```

***

### **6. Limitations of Clustering**

While clustering improves performance, there are some **limitations**:

* **Workers do not share memory** (data stored in one worker is not available to others)
* **Stateful applications need a shared data store** (e.g., Redis, Database)
* **Overhead of creating and managing worker processes**

For better **scalability**, consider **load balancers** like **NGINX** or **PM2 (Process Manager 2)**.

***

### **7. Conclusion**

✔ **Cluster module allows Node.js to utilize multiple CPU cores**\
✔ **Each worker process handles a part of the request load**\
✔ **Master process manages workers and restarts them if they fail**\
✔ **Useful for high-performance applications**
