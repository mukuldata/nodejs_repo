# MongoDB operations

## **Basic MongoDB CRUD Operations**

### **Table of Contents**

1. **Inserting Data (`insertOne`)**
2. &#x20;**Finding Data (`find`)**
3. &#x20;**Updating Data (`updateOne`)**
4. &#x20;**Deleting Data (`deleteOne`)**

***

### **1. Inserting Data (`insertOne`)**

#### **Explanation:**

The `insertOne()` method is used to insert a single document (record) into a MongoDB collection.

#### **Example:**

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/mydatabase', { 
    useNewUrlParser: true, 
    useUnifiedTopology: true 
});

const userSchema = new mongoose.Schema({
    name: String,
    email: String,
    age: Number
});

const User = mongoose.model('User', userSchema);

const addUser = async () => {
    const newUser = new User({
        name: "John Doe",
        email: "john@example.com",
        age: 25
    });

    await newUser.save();
    console.log("User added successfully");
};

addUser();
```

#### **What Happens?**

* A user document with `name`, `email`, and `age` fields is added to the `users` collection.
* `newUser.save()` stores the document in MongoDB.

***

### **2. Finding Data (`find`)**

#### **Explanation:**

The `find()` method is used to retrieve documents from a collection.

#### **Example:**

```javascript
const getUsers = async () => {
    const users = await User.find();
    console.log("Users:", users);
};

getUsers();
```

#### **What Happens?**

* It fetches all users from the `users` collection.
* You can add filters like `User.find({ age: 25 })` to get users of a specific age.

***

### **3. Updating Data (`updateOne`)**

#### **Explanation:**

The `updateOne()` method updates the first document that matches the filter condition.

#### **Example:**

```javascript
const updateUser = async () => {
    await User.updateOne({ name: "John Doe" }, { $set: { age: 26 } });
    console.log("User updated successfully");
};

updateUser();
```

#### **What Happens?**

* Finds a user with the name `"John Doe"` and updates their `age` to 26.
* `$set` ensures only the specified field (`age`) is modified.

***

### **4. Deleting Data (`deleteOne`)**

#### **Explanation:**

The `deleteOne()` method removes a single document from the collection.

#### **Example:**

```javascript
const deleteUser = async () => {
    await User.deleteOne({ name: "John Doe" });
    console.log("User deleted successfully");
};

deleteUser();
```

#### **What Happens?**

* Finds and removes a user where `name` is `"John Doe"`.

***

#### **Summary:**

* **`insertOne()`** → Adds a new document.
* **`find()`** → Retrieves documents.
* **`updateOne()`** → Modifies an existing document.
* **`deleteOne()`** → Removes a document.
