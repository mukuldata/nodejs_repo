# Best Practices(REST)

### **Table of Contents**

1. **What is API Design?**
2. **Versioning in APIs**
3. **Pagination in APIs**
4. **Filtering in APIs**
5. **Understanding `req.query` in Express**
6. **Summary**

***

### **1. What is API Design?**

#### **Explanation:**

API design focuses on creating a **structured, scalable, and efficient** way for clients (frontend apps, mobile apps) to communicate with the backend.\
Good API design includes:

* **Versioning** → Helps manage changes over time
* **Pagination** → Efficiently handles large datasets
* **Filtering** → Allows users to request specific data

***

### **2. Versioning in APIs**

#### **Why is Versioning Important?**

When an API evolves, **older versions** should still work while new features are added.

#### **Ways to Implement Versioning:**

| Approach                       | Example                                   | Usage                              |
| ------------------------------ | ----------------------------------------- | ---------------------------------- |
| **URL Versioning**             | `/v1/users`                               | Most common & easy to manage       |
| **Header Versioning**          | `Accept: application/vnd.example.v1+json` | Avoids URL clutter but less common |
| **Query Parameter Versioning** | `/users?version=1`                        | Flexible, but harder to track      |

#### **Example (URL Versioning in Express):**

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/v1/users', (req, res) => {
  res.json({ message: "API Version 1 - User List" });
});

app.get('/v2/users', (req, res) => {
  res.json({ message: "API Version 2 - Updated User List" });
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

***

### **3. Pagination in APIs**

#### **Why is Pagination Important?**

When dealing with large amounts of data (e.g., thousands of users), **loading everything at once** is inefficient. **Pagination** returns only a small portion of data at a time.

#### **How Pagination Works?**

Pagination uses **limit and page** parameters:

| Parameter | Purpose                  | Example                                  |
| --------- | ------------------------ | ---------------------------------------- |
| `limit`   | Number of items per page | `/users?limit=10` (10 users per page)    |
| `page`    | Page number              | `/users?limit=10&page=2` (next 10 users) |

#### **Example (Pagination in Express using `req.query`)**

```javascript
const users = Array.from({ length: 50 }, (_, i) => ({ id: i + 1, name: `User ${i + 1}` }));

app.get('/users', (req, res) => {
  const limit = parseInt(req.query.limit) || 10; // Default limit = 10
  const page = parseInt(req.query.page) || 1; // Default page = 1

  const startIndex = (page - 1) * limit;
  const endIndex = startIndex + limit;
  const paginatedUsers = users.slice(startIndex, endIndex);

  res.json({
    page,
    limit,
    totalUsers: users.length,
    data: paginatedUsers
  });
});
```

**Example API Calls:**

✅ `/users?limit=5&page=1` → Returns first 5 users\
✅ `/users?limit=5&page=2` → Returns next 5 users

***

### **4. Filtering in APIs**

#### **Why is Filtering Important?**

Instead of returning **all data**, APIs should allow users to **filter results** based on conditions (e.g., find users with a specific name).

#### **How Filtering Works?**

Filtering is done using **query parameters** in `req.query`.

| Parameter | Purpose             | Example             |
| --------- | ------------------- | ------------------- |
| `name`    | Filter by user name | `/users?name=Alice` |
| `age`     | Filter by age       | `/users?age=25`     |

#### **Example (Filtering Users in Express)**

```javascript
app.get('/users', (req, res) => {
  let filteredUsers = users;

  if (req.query.name) {
    filteredUsers = filteredUsers.filter(user => user.name.toLowerCase().includes(req.query.name.toLowerCase()));
  }

  res.json(filteredUsers);
});
```

**Example API Calls:**

✅ `/users?name=User 10` → Returns users with "User 10" in the name

***

### **5. Understanding `req.query` in Express**

#### **What is `req.query`?**

* `req.query` holds **query parameters** sent in the URL.
* Used for **pagination, filtering, sorting, and searching**.

#### **Example:**

```javascript
app.get('/search', (req, res) => {
  res.json({ queryReceived: req.query });
});
```

**Example API Calls:**

✅ `/search?name=John&age=30`\
✅ Output: `{ "name": "John", "age": "30" }`

***

### **6. Summary**

| Feature         | Example                    | Purpose                                        |
| --------------- | -------------------------- | ---------------------------------------------- |
| **Versioning**  | `/v1/users` vs `/v2/users` | Allows updating API while keeping old versions |
| **Pagination**  | `/users?limit=10&page=2`   | Loads large datasets in chunks                 |
| **Filtering**   | `/users?name=Alice`        | Returns specific data based on criteria        |
| **`req.query`** | `/search?name=John`        | Accesses query parameters in Express           |
