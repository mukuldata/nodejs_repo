# Timers

### **Table of Contents**

1. **Introduction to Timers in Node.js**
2. **setTimeout() – Execute After a Delay**
3. **setInterval() – Execute Repeatedly**
4. **clearTimeout() and clearInterval()**
5. **process.nextTick() – Execute Before the Next Event Loop Cycle**
6. **setImmediate() – Execute After I/O Operations**
7. **Key Differences Between Timers**
8. **Conclusion**

***

## **1. Introduction to Timers in Node.js**

In Node.js, timers allow scheduling code execution **after a delay** or **at regular intervals**.

Timers include:

* `setTimeout()` → Runs a function **once** after a delay.
* `setInterval()` → Runs a function **repeatedly** at a fixed interval.
* `process.nextTick()` → Runs a function **immediately** after the current operation.
* `setImmediate()` → Runs a function **after I/O operations complete**.

#### **Example Use Cases**

✔ Sending notifications after a delay (`setTimeout`).\
✔ Running a task every second (`setInterval`).\
✔ Executing critical code before moving to the next event loop (`process.nextTick`).

***

## **2. setTimeout() – Execute After a Delay**

`setTimeout()` runs a function **once** after a given time delay (in milliseconds).

#### **Syntax**

```javascript
setTimeout(callbackFunction, delay);
```

* `callbackFunction`: Function to execute.
* `delay`: Time in milliseconds (1 second = 1000 ms).

#### **Example: Print a Message After 3 Seconds**

```javascript
console.log('Start');

setTimeout(() => {
    console.log('Hello after 3 seconds!');
}, 3000);

console.log('End');
```

#### **Output**

```
Start
End
Hello after 3 seconds!
```

✔ Even though `setTimeout` is at the top, it runs **after 3 seconds** because Node.js **does not block execution**.

***

## **3. setInterval() – Execute Repeatedly**

`setInterval()` runs a function **repeatedly** at a fixed interval.

#### **Syntax**

```javascript
setInterval(callbackFunction, interval);
```

* `callbackFunction`: Function to execute.
* `interval`: Time in milliseconds between executions.

#### **Example: Print a Message Every 2 Seconds**

```javascript
let count = 0;
const intervalId = setInterval(() => {
    count++;
    console.log(`Message printed ${count} times`);
    if (count === 5) clearInterval(intervalId); // Stops after 5 times
}, 2000);
```

#### **Output**

```
Message printed 1 times
Message printed 2 times
Message printed 3 times
Message printed 4 times
Message printed 5 times
```

✔ **Use case**: Keeping track of **real-time data updates** (like live scores).

***

## **4. clearTimeout() and clearInterval()**

* `clearTimeout(timerId)` → Stops a `setTimeout` before execution.
* `clearInterval(intervalId)` → Stops a `setInterval` from running continuously.

#### **Example: Stopping setTimeout Before Execution**

```javascript
const timeoutId = setTimeout(() => {
    console.log('This will not run');
}, 5000);

clearTimeout(timeoutId);  // Cancels the timeout
```

✔ **Useful when we no longer need to execute the delayed function.**

***

## **5. process.nextTick() – Execute Before the Next Event Loop Cycle**

`process.nextTick()` schedules a function to run **immediately** after the current operation, before the event loop continues.

#### **Example: Immediate Execution**

```javascript
console.log('Start');

process.nextTick(() => {
    console.log('Executed in process.nextTick');
});

console.log('End');
```

#### **Output**

```
Start
End
Executed in process.nextTick
```

✔ **Even though `process.nextTick` is placed last, it executes before the event loop continues.**\
✔ **Use case**: Running important tasks **before any I/O operation** in the event loop.

***

## **6. setImmediate() – Execute After I/O Operations**

`setImmediate()` schedules a function to run **after I/O operations are complete**.

#### **Example: setImmediate vs. process.nextTick**

```javascript
console.log('Start');

setImmediate(() => {
    console.log('Executed in setImmediate');
});

process.nextTick(() => {
    console.log('Executed in process.nextTick');
});

console.log('End');
```

#### **Output**

```
Start
End
Executed in process.nextTick
Executed in setImmediate
```

✔ `process.nextTick()` runs **before** `setImmediate()`.\
✔ `setImmediate()` runs **after I/O tasks finish**.

***

## **7. Key Differences Between Timers**

| Timer                  | When it Executes                                                          |
| ---------------------- | ------------------------------------------------------------------------- |
| **setTimeout()**       | After a specified delay (once)                                            |
| **setInterval()**      | Repeatedly after a fixed interval                                         |
| **clearTimeout()**     | Cancels `setTimeout()`                                                    |
| **clearInterval()**    | Cancels `setInterval()`                                                   |
| **process.nextTick()** | Immediately after the current operation, before the next event loop cycle |
| **setImmediate()**     | After I/O operations are complete                                         |

***

## **8. Conclusion**

✔ `setTimeout()` → Delays execution.\
✔ `setInterval()` → Runs repeatedly.\
✔ `clearTimeout()` / `clearInterval()` → Stops scheduled execution.\
✔ `process.nextTick()` → Executes **before the next event loop cycle**.\
✔ `setImmediate()` → Executes **after I/O operations complete**.

These timers help manage **asynchronous tasks efficiently** in Node.js.
