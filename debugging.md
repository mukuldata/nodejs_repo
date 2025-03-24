# Debugging

### **Table of Contents**

1. **Introduction to Memory Leaks in Node.js**
2. **Common Causes of Memory Leaks**
3. **Detecting Memory Leaks**
   * Using Chrome DevTools (Node Inspector)
   * Using Process Metrics (`process.memoryUsage()`)
   * Using `heapdump` for Heap Snapshots
4. **Debugging with Node Inspector**
   * Enabling Node.js Debug Mode
   * Capturing Heap Snapshots
   * Finding Memory Leaks
5. **Best Practices to Prevent Memory Leaks**
6. **Conclusion**

***

### **1. Introduction to Memory Leaks in Node.js**

A **memory leak** happens when a Node.js application **keeps consuming memory** without releasing it, eventually leading to **high memory usage** and performance issues.

#### **Symptoms of a Memory Leak:**

✅ **Increased memory usage** over time\
✅ **Application slowing down**\
✅ **Frequent crashes due to Out of Memory (OOM) errors**

***

### **2. Common Causes of Memory Leaks**

1. **Global Variables** – Unused data in global variables never gets garbage collected.
2. **Event Listeners Not Removed** – Adding event listeners but not removing them causes memory leaks.
3. **Unclosed Database Connections** – Connections left open consume memory.
4. **Unused Timers or Intervals** – Not clearing `setInterval` or `setTimeout` can cause leaks.
5. **Closures Holding References** – Functions that keep unnecessary references to objects.

***

### **3. Detecting Memory Leaks**

#### **1️⃣ Using Chrome DevTools (Node Inspector)**

You can use **Chrome DevTools** to analyze memory usage in Node.js applications.

**Step 1**: Start your Node.js app in debug mode:

```bash
node --inspect app.js
```

**Step 2**: Open **Chrome** and go to:

```
chrome://inspect
```

**Step 3**: Click "Inspect" under **Remote Target**, then go to the **Memory** tab.

***

#### **2️⃣ Using Process Metrics (`process.memoryUsage()`)**

You can check memory usage in your application with:

```javascript
setInterval(() => {
  console.log(process.memoryUsage());
}, 5000);
```

This prints memory usage every 5 seconds.

***

#### **3️⃣ Using `heapdump` for Heap Snapshots**

Install `heapdump` to analyze memory usage:

```bash
npm install heapdump
```

Take a heap snapshot in your code:

```javascript
const heapdump = require('heapdump');

setInterval(() => {
  heapdump.writeSnapshot(`./heap-${Date.now()}.heapsnapshot`);
  console.log("Heap snapshot saved.");
}, 60000);
```

Open the snapshot in **Chrome DevTools** under the **Memory tab**.

***

### **4. Debugging with Node Inspector**

#### **1️⃣ Enabling Node.js Debug Mode**

Start your app with debugging enabled:

```bash
node --inspect-brk app.js
```

This pauses execution at the start, allowing debugging before memory issues occur.

***

#### **2️⃣ Capturing Heap Snapshots**

1. Open **Chrome DevTools** (`chrome://inspect`).
2. Go to the **Memory tab** → Click **Take Snapshot**.
3. Run your application for some time and take another snapshot.
4. Compare snapshots to find objects that keep growing in size.

***

#### **3️⃣ Finding Memory Leaks**

* Look for objects that **keep increasing in size** across snapshots.
* Identify references **not getting garbage collected**.

Example: If a **global variable** stores objects indefinitely, they won’t be cleared:

```javascript
let memoryLeakArray = [];

setInterval(() => {
  memoryLeakArray.push(new Array(1000).fill('*'));
}, 1000);
```

This will cause a **memory leak** since objects are never removed.

***

### **5. Best Practices to Prevent Memory Leaks**

✔ **Use WeakMap or WeakSet** for object references that should be garbage collected.\
✔ **Remove unused event listeners** with `removeListener()`.\
✔ **Close database connections** properly.\
✔ **Clear intervals and timeouts** using `clearInterval()` and `clearTimeout()`.\
✔ **Monitor memory usage** regularly.

***

### **6. Conclusion**

🔹 **Memory leaks can slow down or crash applications.**\
🔹 **Use Chrome DevTools, process metrics, and heap snapshots to detect leaks.**\
🔹 **Prevent leaks by managing event listeners, timers, and database connections.**
