# Process Management

### **Table of Contents**

1. **Introduction to Process Management in Node.js**
2. **process.env – Accessing Environment Variables**
3. **process.exit() – Stopping a Process**
4. **process.argv – Command Line Arguments**
5. **Other Useful Process Methods**
6. **Conclusion**

***

## **1. Introduction to Process Management in Node.js**

A **process** in Node.js is an instance of a running program.\
The `process` object allows us to:\
✔ Access **environment variables** (`process.env`).\
✔ Stop execution **manually** (`process.exit()`).\
✔ Read **command-line arguments** (`process.argv`).

**Example Use Cases:**

* Using **environment variables** for security (e.g., API keys).
* Exiting a script when an error occurs.
* Passing values **from the command line**.

***

## **2. process.env – Accessing Environment Variables**

Environment variables (`process.env`) store **sensitive** or **configurable** data, such as:

* API keys
* Database URLs
* App modes (development/production)

#### **Example: Access an Environment Variable**

```javascript
console.log('Your API Key:', process.env.API_KEY);
```

✔ If `API_KEY` is set, it prints the value.\
✔ If `API_KEY` is **not set**, it prints `undefined`.

#### **Setting Environment Variables**

**On Windows (Command Prompt)**

```sh
set API_KEY=123456
node app.js
```

**On macOS/Linux (Terminal)**

```sh
export API_KEY=123456
node app.js
```

#### **Example: Using Environment Variables in a Script**

```javascript
const port = process.env.PORT || 3000; // Default to 3000 if PORT is not set
console.log(`Server running on port ${port}`);
```

✔ This ensures **port flexibility** across different environments.

***

## **3. process.exit() – Stopping a Process**

`process.exit()` **stops** the Node.js process **immediately**.

* `process.exit(0)` → Success (no errors).
* `process.exit(1)` → Failure (error occurred).

#### **Example: Exit If Required Environment Variables Are Missing**

```javascript
if (!process.env.API_KEY) {
    console.error('Missing API Key! Exiting...');
    process.exit(1); // Stop execution
}
console.log('API Key Found, Proceeding...');
```

✔ If `API_KEY` is missing, the script **stops** immediately.\
✔ **Use case**: Preventing execution **when required settings are missing**.

***

## **4. process.argv – Command Line Arguments**

`process.argv` stores **command-line arguments**.

* `process.argv[0]` → Path to Node.js executable.
* `process.argv[1]` → Path to the script file.
* `process.argv[2]` → First argument from the command line.

#### **Example: Print Command Line Arguments**

```javascript
console.log(process.argv);
```

**Run in Terminal**

```sh
node app.js hello world
```

**Output**

```
[
  '/usr/bin/node',   // Node.js path
  '/path/to/app.js', // Script path
  'hello',           // First argument
  'world'            // Second argument
]
```

✔ **Use case**: Making scripts **interactive** by allowing users to pass input.

***

#### **Example: Simple CLI Calculator Using process.argv**

```javascript
const args = process.argv.slice(2); // Ignore first two arguments
const num1 = parseInt(args[0]);
const num2 = parseInt(args[1]);

if (!num1 || !num2) {
    console.error('Please provide two numbers!');
    process.exit(1);
}

console.log(`Sum: ${num1 + num2}`);
```

**Run in Terminal**

```sh
node app.js 5 10
```

**Output**

```
Sum: 15
```

✔ **Use case**: Creating command-line tools.

***

## **5. Other Useful Process Methods**

| Method                    | Description                                            |
| ------------------------- | ------------------------------------------------------ |
| **process.cwd()**         | Returns the **current working directory**              |
| **process.uptime()**      | Returns the time (in seconds) since the script started |
| **process.memoryUsage()** | Shows how much memory the process is using             |
| **process.kill(pid)**     | Terminates a process by its **process ID (PID)**       |

#### **Example: Print Current Working Directory**

```javascript
console.log('Current Directory:', process.cwd());
```

***

## **6. Conclusion**

✔ `process.env` → Reads **environment variables**.\
✔ `process.exit()` → Stops execution **manually**.\
✔ `process.argv` → Reads **command-line arguments**.\
✔ Useful for **security, automation, and command-line tools**.



