# Environment Variables & dotenv

### **Table of Contents**

1. **Introduction to Environment Variables**
2. **Why Use Environment Variables?**
3. **Using dotenv in Node.js**
   * Installing dotenv
   * Creating a `.env` file
   * Accessing environment variables
4. **Best Practices for Using dotenv**
5. **Conclusion**

***

### **1. Introduction to Environment Variables**

Environment variables are **key-value pairs** stored outside your code. They store **configuration settings** like:\
✔ Database connection URLs\
✔ API keys\
✔ Secret keys\
✔ Port numbers

Example of an **environment variable**:

```bash
PORT=5000
DATABASE_URL=mongodb://localhost:27017/mydb
```

***

### **2. Why Use Environment Variables?**

✔ **Security** – Sensitive data like API keys are not exposed in the code.\
✔ **Portability** – Same code can run on **different environments** (development, production).\
✔ **Flexibility** – Easily change configurations **without modifying the code**.

Example:

```javascript
const API_KEY = "123456789"; // ❌ Bad practice: Exposes secret in code
```

✅ **Instead, use an environment variable:**

```javascript
const API_KEY = process.env.API_KEY; // Access from .env file
```

***

### **3. Using dotenv in Node.js**

#### **1️⃣ Installing dotenv**

Run the following command to install dotenv:

```bash
npm install dotenv
```

***

#### **2️⃣ Creating a `.env` File**

In the root folder of your project, create a **.env** file and add:

```env
PORT=4000
DATABASE_URL=mongodb://localhost:27017/mydatabase
SECRET_KEY=mysecretkey
```

***

#### **3️⃣ Loading dotenv in Your Code**

In your `server.js` or `index.js`, add:

```javascript
require('dotenv').config(); // Load .env file

console.log("Server running on port:", process.env.PORT);
console.log("Database URL:", process.env.DATABASE_URL);
```

🔹 `process.env.PORT` → Fetches the `PORT` value from `.env`.\
🔹 `process.env.DATABASE_URL` → Fetches the `DATABASE_URL`.

***

#### **4️⃣ Using dotenv with Express**

Example of using environment variables in an **Express server**:

```javascript
require('dotenv').config(); 
const express = require('express');

const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

🔹 The app will run on **PORT=4000** if it's set in `.env`, otherwise it defaults to `3000`.

***

### **4. Best Practices for Using dotenv**

✔ **Never commit the `.env` file** – Add it to `.gitignore`:

```gitignore
.env
```

✔ **Use a `.env.example` file** – Share the structure without real values:

```env
PORT=
DATABASE_URL=
SECRET_KEY=
```

✔ **Use `dotenv` early** – Call `require('dotenv').config()` at the top.\
✔ **Keep secrets encrypted in production** – Use **cloud secret managers** like AWS Secrets Manager or Google Secret Manager.

***

### **5. Conclusion**

✅ **Environment variables store configuration settings securely.**\
✅ **dotenv helps load `.env` files in Node.js.**\
✅ **Never hardcode secrets in your code!**
