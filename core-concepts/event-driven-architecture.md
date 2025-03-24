# Event Driven Architecture

### **Table of Contents**

1. **Introduction to Event-Driven Architecture & Event Emitters**
2. **What is Event-Driven Architecture?**
   * How It Works
   * Real-Life Example
3. **What is an Event Emitter in Node.js?**
   * Understanding Events & Listeners
4. **Using the EventEmitter Class in Node.js**
   * Creating and Emitting Events
   * Handling Events with Listeners
5. **Example Code in Node.js**
6. **Execution Flow of Events**
7. **Key Takeaways**

***

## **1. Introduction to Event-Driven Architecture & Event Emitters**

Node.js is built on an **event-driven architecture**, which means that **actions (events) trigger specific responses (event listeners).**

Example:

* When you **click a button** on a webpage, something happens (an event listener executes).
* Similarly, in Node.js, **events are emitted**, and listeners handle them.

***

## **2. What is Event-Driven Architecture?**

**Event-Driven Architecture** is a design pattern where a system responds to **events** rather than following a strict sequence of instructions.

#### **How It Works**

1. **An event occurs** (like a user action or data arrival).
2. **The event is detected** by an event handler.
3. **An event listener responds** by executing some code.

#### **Real-Life Example: Ordering Pizza**

* You **order pizza** → (Event is emitted)
* The restaurant **prepares the pizza** → (Listener responds)
* The pizza is **delivered to you** → (Action is completed)

***

## **3. What is an Event Emitter in Node.js?**

In Node.js, **EventEmitter** is a class that allows us to create and handle events.

#### **Understanding Events & Listeners**

* **Events**: Triggers that indicate something happened.
* **Listeners**: Functions that execute when an event occurs.

Example:

* When a user logs in, an **event ("userLoggedIn")** is emitted.
* A **listener** runs a function to log the event.

***

## **4. Using the EventEmitter Class in Node.js**

To use event-driven programming, we use the built-in **events** module in Node.js.

#### **Step 1: Import the events module**

```javascript
const EventEmitter = require('events');
```

#### **Step 2: Create an EventEmitter object**

```javascript
const myEmitter = new EventEmitter();
```

#### **Step 3: Define an event listener**

```javascript
myEmitter.on('greet', () => {
    console.log('Hello, welcome!');
});
```

#### **Step 4: Emit an event**

```javascript
myEmitter.emit('greet');
```

***

## **5. Example Code in Node.js**

#### **Basic Example: Custom Event Emitter**

```javascript
const EventEmitter = require('events');

// Create an instance of EventEmitter
const myEmitter = new EventEmitter();

// Define an event listener for "message"
myEmitter.on('message', (text) => {
    console.log(`Received message: ${text}`);
});

// Emit the event
myEmitter.emit('message', 'Hello, Node.js!');
```

#### **Output:**

```
Received message: Hello, Node.js!
```

***

#### **Advanced Example: Multiple Listeners & Arguments**

```javascript
const EventEmitter = require('events');
const myEmitter = new EventEmitter();

// First listener
myEmitter.on('order', (item, quantity) => {
    console.log(`Order placed: ${quantity} ${item}(s)`);
});

// Second listener
myEmitter.on('order', (item, quantity) => {
    console.log(`Processing order for ${item}`);
});

// Emit the "order" event with arguments
myEmitter.emit('order', 'Pizza', 2);
```

#### **Output:**

```
Order placed: 2 Pizza(s)
Processing order for Pizza
```

***

## **6. Execution Flow of Events**

1️⃣ An **event is emitted** using `.emit()`.\
2️⃣ The **event listeners detect** the event.\
3️⃣ The **listeners execute the assigned function**.

**Example:**

* A "downloadComplete" event is emitted.
* A listener logs **"Download finished!"**.

***

## **7. Key Takeaways**

✔ Event-Driven Architecture makes applications efficient and scalable.\
✔ Event Emitters allow handling multiple events asynchronously.\
✔ Listeners execute when an event occurs, reducing blocking operations.\
✔ Node.js uses EventEmitters for handling file systems, HTTP requests, and real-time applications.

This approach is **widely used in real-time applications**, like **chat apps, notifications, and live updates!**
