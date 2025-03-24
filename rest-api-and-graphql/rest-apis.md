# REST API's

### **Table of Contents**

1. **What is a RESTful API?**
2. **Setting Up Express.js**
3. **Creating Basic CRUD Routes**
4. **Handling Requests & Responses**
5. **Error Handling in Express**

***

### **1. What is a RESTful API?**

#### **Explanation:**

A **RESTful API (Representational State Transfer)** allows clients (like web apps, mobile apps) to communicate with a server using **HTTP methods**.

#### **Common HTTP Methods in RESTful APIs**

| Method     | Purpose              | Example              |
| ---------- | -------------------- | -------------------- |
| **GET**    | Retrieve data        | Get a list of users  |
| **POST**   | Create new data      | Add a new user       |
| **PUT**    | Update existing data | Update a user's info |
| **DELETE** | Remove data          | Delete a user        |

***

### **2. Setting Up Express.js**

#### **Install Express**

```bash
npm init -y  
npm install express  
```

#### **Create `server.js`**

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.use(express.json()); // Middleware to parse JSON data

app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

***

### **3. Creating Basic CRUD Routes**

#### **1. Get All Users (`GET /users`)**

```javascript
const users = [{ id: 1, name: "Alice" }, { id: 2, name: "Bob" }];

app.get('/users', (req, res) => {
  res.json(users);
});
```

***

#### **2. Create a New User (`POST /users`)**

```javascript
app.post('/users', (req, res) => {
  const newUser = { id: users.length + 1, name: req.body.name };
  users.push(newUser);
  res.status(201).json(newUser);
});
```

***

#### **3. Update a User (`PUT /users/:id`)**

```javascript
app.put('/users/:id', (req, res) => {
  const user = users.find(u => u.id == req.params.id);
  if (!user) return res.status(404).json({ message: "User not found" });

  user.name = req.body.name;
  res.json(user);
});
```

***

#### **4. Delete a User (`DELETE /users/:id`)**

```javascript
app.delete('/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id == req.params.id);
  if (index === -1) return res.status(404).json({ message: "User not found" });

  users.splice(index, 1);
  res.json({ message: "User deleted" });
});
```

***

### **4. Handling Requests & Responses**

* **`req.body`** → Data sent from the client (for POST, PUT).
* **`req.params`** → Extracts URL parameters (`:id`).
* **`res.json()`** → Sends a JSON response.
* **`res.status(code)`** → Sends an HTTP status code (e.g., `201` for created, `404` for not found).

***

### **5. Error Handling in Express**

To handle errors globally, use **middleware**:

```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: "Something went wrong!" });
});
```

***

### **Summary:**

| Feature         | Code Example                                    |
| --------------- | ----------------------------------------------- |
| **Get Users**   | `app.get('/users', (req, res) => {...})`        |
| **Create User** | `app.post('/users', (req, res) => {...})`       |
| **Update User** | `app.put('/users/:id', (req, res) => {...})`    |
| **Delete User** | `app.delete('/users/:id', (req, res) => {...})` |
