# Built-in Modules

### **Table of Contents**

1. **What are Built-in Modules?**
2. **Why Use Built-in Modules?**
3. **Common Built-in Modules in Node.js**
   * 3.1 `fs` (File System)
   * 3.2 `http` (Creating a Web Server)
   * 3.3 `path` (Handling File Paths)
   * 3.4 `os` (Operating System Information)
   * 3.5 `events` (Event Handling)
   * 3.6 `util` (Utility Functions)
4. **Conclusion**

***

## **1. What are Built-in Modules?**

1. Node.js provides **built-in modules** that allow developers to perform common tasks **without installing additional packages**.&#x20;
2. These modules help in **file handling, networking, operating system interactions, and more**.

***

## **2. Why Use Built-in Modules?**

✔ **No need to install external dependencies**\
✔ **Optimized for performance**\
✔ **Easily accessible with `require()` or `import`**

***

## **3. Common Built-in Modules in Node.js**

### **3.1 `fs` (File System Module)**

The `fs` module allows us to **read, write, delete, and manage files** in Node.js.

#### **Example: Reading a File (`fs.readFile`)**

```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data); // Output: Content of example.txt
});
```

✔ **Handles file operations**\
✔ **Asynchronous & Synchronous versions available**

***

### **3.2 `http` (Creating a Web Server)**

The `http` module is used to **create a simple web server**.

#### **Example: Creating a Simple Web Server**

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, Node.js Server!');
});

server.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

✔ **Handles HTTP requests and responses**\
✔ **Used to build APIs and web applications**

***

### **3.3 `path` (Handling File Paths)**

The `path` module helps in working with **file and directory paths**.

#### **Example: Getting the File Name**

```javascript
const path = require('path');

const filePath = '/home/user/docs/file.txt';

console.log(path.basename(filePath));  // Output: file.txt
console.log(path.dirname(filePath));   // Output: /home/user/docs
console.log(path.extname(filePath));   // Output: .txt
```

✔ **Useful for working with file paths**\
✔ **Cross-platform compatibility**

***

### **3.4 `os` (Operating System Information)**

The `os` module provides **system-related information** like CPU details, memory, and OS type.

#### **Example: Getting OS Information**

```javascript
const os = require('os');

console.log(os.type());        // Output: Linux / Windows_NT / Darwin
console.log(os.platform());    // Output: win32 / linux / darwin
console.log(os.totalmem());    // Output: Total system memory in bytes
console.log(os.freemem());     // Output: Free memory in bytes
console.log(os.cpus());        // Output: CPU information
```

✔ **Helps in system monitoring and resource management**

***

### **3.5 `events` (Event Handling)**

The `events` module allows us to work with **event-driven programming**.

#### **Example: Creating and Using an Event Emitter**

```javascript
const EventEmitter = require('events');

const eventEmitter = new EventEmitter();

eventEmitter.on('greet', (name) => {
    console.log(`Hello, ${name}!`);
});

eventEmitter.emit('greet', 'Alice'); // Output: Hello, Alice!
```

✔ **Useful for handling asynchronous events**\
✔ **Foundation for event-driven applications**

***

### **3.6 `util` (Utility Functions)**

The `util` module provides **helpful utility functions** for debugging and formatting.

#### **Example: Converting a Function to Return a Promise (`util.promisify`)**

```javascript
const fs = require('fs');
const util = require('util');

const readFile = util.promisify(fs.readFile);

readFile('example.txt', 'utf8')
    .then((data) => console.log(data))
    .catch((err) => console.error(err));
```

✔ **Simplifies working with async/await**

***

## **4. Conclusion**

✔ Node.js **built-in modules** help perform various tasks efficiently.\
✔ **`fs`** - File operations\
✔ **`http`** - Web server creation\
✔ **`path`** - File path handling\
✔ **`os`** - System-related information\
✔ **`events`** - Event-driven programming\
✔ **`util`** - Utility functions
