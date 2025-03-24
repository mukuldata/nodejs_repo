# DB Transactions

### **Table of Contents**

1. **What is a Database Transaction?**
2. **MongoDB Transactions (Sessions & Transactions)**
3. **SQL Transactions (`BEGIN`, `COMMIT`, `ROLLBACK`)**

***

### **1. What is a Database Transaction?**

#### **Explanation:**

A **transaction** is a sequence of database operations that must be executed as a single unit. It follows the **ACID** properties:

* **Atomicity** – Either all operations succeed, or none happen.
* **Consistency** – The database remains valid before and after the transaction.
* **Isolation** – Transactions do not interfere with each other.
* **Durability** – Once committed, data is permanently stored.

Example scenario:

* Deduct money from **User A**
* Add money to **User B**
* If one step fails, both should **rollback** (undo).

***

### **2. MongoDB Transactions (Sessions & Transactions)**

#### **Explanation:**

MongoDB **transactions** allow multiple operations on multiple documents to be **executed as a unit**.

#### **Example:**

```javascript
const mongoose = require('mongoose');

async function transferMoney(session) {
  try {
    // Start a transaction
    session.startTransaction();

    // Deduct money from User A
    await User.updateOne({ name: "User A" }, { $inc: { balance: -100 } }, { session });

    // Add money to User B
    await User.updateOne({ name: "User B" }, { $inc: { balance: 100 } }, { session });

    // Commit transaction
    await session.commitTransaction();
    console.log("Transaction Successful");
  } catch (error) {
    // Rollback transaction
    await session.abortTransaction();
    console.log("Transaction Failed", error);
  } finally {
    session.endSession();
  }
}

// Start a session and run the function
mongoose.startSession().then(session => transferMoney(session));
```

#### **What Happens?**

* **Deducts** 100 from **User A**.
* **Adds** 100 to **User B**.
* If any operation **fails**, the transaction **rolls back**.

***

### **3. SQL Transactions (`BEGIN`, `COMMIT`, `ROLLBACK`)**

#### **Explanation:**

In **SQL**, transactions are managed using:

* `BEGIN TRANSACTION` → Start transaction.
* `COMMIT` → Save changes if successful.
* `ROLLBACK` → Undo changes if something fails.

#### **Example:**

```sql
BEGIN TRANSACTION;

UPDATE users SET balance = balance - 100 WHERE name = 'User A';

UPDATE users SET balance = balance + 100 WHERE name = 'User B';

-- If both succeed
COMMIT;

-- If any step fails
ROLLBACK;
```

#### **What Happens?**

* Money is **deducted from User A** and **added to User B**.
* If any step **fails**, `ROLLBACK` restores the original state.

***

### **Summary:**

| Feature              | MongoDB Transactions          | SQL Transactions       |
| -------------------- | ----------------------------- | ---------------------- |
| **How to Start**     | `session.startTransaction()`  | `BEGIN TRANSACTION`    |
| **Commit Changes**   | `session.commitTransaction()` | `COMMIT`               |
| **Rollback Changes** | `session.abortTransaction()`  | `ROLLBACK`             |
| **Use Case**         | Multiple document updates     | Multiple table updates |

**Transactions ensure data consistency and reliability in both MongoDB and SQL.**
