# Streams and Buffers

### **Table of Contents**

1. **Introduction to Streams and Buffers**
2. **What are Streams?**
   * Why Use Streams?
   * Types of Streams in Node.js
3. **What is a Buffer?**
   * How Buffers Work
4. **Readable Streams** (Reading Data)
5. **Writable Streams** (Writing Data)
6. **Duplex Streams** (Both Read & Write)
7. **Transform Streams** (Modifying Data)
8. **Example Code for Each Type of Stream**
9. **Key Takeaways**

***

## **1. Introduction to Streams and Buffers**

In Node.js, **streams and buffers** help handle **large amounts of data efficiently**. Instead of reading/writing the whole file at once, we process data **in chunks**.

**Example:**

* Watching a YouTube video → The video loads in parts (streaming), not all at once.
* Downloading a large file → It downloads in **chunks** rather than waiting for the whole file.

***

## **2. What are Streams?**

Streams are a way to **handle data piece by piece**, rather than all at once.

#### **Why Use Streams?**

✔ Efficient for large files (like videos, logs, or live data).\
✔ Uses less memory compared to reading a full file at once.\
✔ Enables real-time data processing (like chat apps).

#### **Types of Streams in Node.js**

| Stream Type   | Description                                            |
| ------------- | ------------------------------------------------------ |
| **Readable**  | Reads data (e.g., reading a file)                      |
| **Writable**  | Writes data (e.g., saving logs)                        |
| **Duplex**    | Reads & writes data (e.g., a chat app)                 |
| **Transform** | Modifies data while passing (e.g., compressing a file) |

***

## **3. What is a Buffer?**

A **buffer** is a temporary storage area for data while transferring between different locations.

#### **How Buffers Work**

* Node.js **stores raw data** in buffers before processing it.
* They are useful for handling **binary data** (like images, videos, and streams).

**Example:**

```javascript
const buffer = Buffer.from('Hello');
console.log(buffer); 
```

#### **Output:**

```
<Buffer 48 65 6c 6c 6f>  // Binary representation of 'Hello'
```

***

## **4. Readable Streams (Reading Data)**

A **Readable Stream** allows reading data **in chunks** instead of loading everything at once.

#### **Example: Reading a file using a Readable Stream**

```javascript
const fs = require('fs');

const readStream = fs.createReadStream('example.txt', 'utf8');

readStream.on('data', (chunk) => {
    console.log('Received chunk:', chunk);
});

readStream.on('end', () => {
    console.log('Finished reading the file');
});
```

#### **Output (if `example.txt` contains "Hello World!")**

```
Received chunk: Hello World!
Finished reading the file
```

✔ **Benefits**: Efficient memory usage, handles large files without loading everything at once.

***

## **5. Writable Streams (Writing Data)**

A **Writable Stream** allows writing data **in chunks** instead of writing everything at once.

#### **Example: Writing data to a file**

```javascript
const fs = require('fs');

const writeStream = fs.createWriteStream('output.txt');

writeStream.write('Hello, this is a writable stream!\n');
writeStream.write('Data is being written in chunks.\n');

writeStream.end(() => {
    console.log('Finished writing to file');
});
```

#### **Output in `output.txt`**

```
Hello, this is a writable stream!
Data is being written in chunks.
```

✔ **Benefit**: No need to store the entire content before writing.

***

## **6. Duplex Streams (Reading & Writing Data)**

A **Duplex Stream** allows **both reading and writing** at the same time.

#### **Example: Creating a Custom Duplex Stream**

```javascript
const { Duplex } = require('stream');

const myStream = new Duplex({
    read(size) {
        this.push('Hello ');
        this.push('World!\n');
        this.push(null);
    },
    write(chunk, encoding, callback) {
        console.log('Writing:', chunk.toString());
        callback();
    }
});

myStream.on('data', (chunk) => {
    console.log('Reading:', chunk.toString());
});

myStream.write('Node.js Streams are cool!\n');
```

#### **Output**

```
Reading: Hello 
Reading: World!
Writing: Node.js Streams are cool!
```

✔ **Use case**: Chat applications, network sockets.

***

## **7. Transform Streams (Modifying Data While Streaming)**

A **Transform Stream** modifies data while passing through.

#### **Example: Converting Text to Uppercase**

```javascript
const { Transform } = require('stream');

const upperCaseTransform = new Transform({
    transform(chunk, encoding, callback) {
        this.push(chunk.toString().toUpperCase());
        callback();
    }
});

process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```

#### **How it Works?**

* Takes input from the console (`process.stdin`).
* Converts it to **uppercase** (`Transform stream`).
* Outputs the modified text (`process.stdout`).

✔ **Use case**: Compression, encryption, data modification.

***

## **8. Key Takeaways**

✔ Streams allow efficient handling of large data (like videos, logs, live feeds).\
✔ Buffers help store temporary binary data.\
✔ Readable Streams → For reading data.\
✔ Writable Streams → For writing data.\
✔ Duplex Streams → For both reading & writing.\
✔ Transform Streams → For modifying data while streaming.
