# CORS

### **Table of Contents**

1. **What is CORS?**
2. **Why is CORS Needed?**
3. **How CORS Works?**
4. **Handling CORS in Express**
   * Allowing All Origins
   * Restricting Specific Origins
   * Allowing Specific Methods and Headers
5. **Example: Implementing CORS in Express**
6. **Conclusion**

***

## **1. What is CORS?**

CORS (**Cross-Origin Resource Sharing**) is a security feature in web browsers that controls how resources (like APIs) can be accessed from different domains.

**Example:**

* Your frontend is hosted on `http://myapp.com`
* Your backend API is hosted on `http://api.myapp.com`
* By default, the browser **blocks** the request because they are on **different origins**.
* CORS allows you to **control** which domains can access your API.

***

## **2. Why is CORS Needed?**

* Browsers block requests between different origins for **security reasons**.
* If not configured properly, your API might be **inaccessible** to clients.
* CORS helps by defining **rules** for cross-origin requests.

***

## **3. How CORS Works?**

* When the browser sends a **cross-origin request**, it includes an **Origin** header.
* If the server **allows** the origin, it sends back an `Access-Control-Allow-Origin` header.
* If the server **blocks** the request, the browser rejects it.

**Example of a CORS Request**

Frontend (`http://myfrontend.com`) sending a request to an API:

```javascript
fetch('http://mybackend.com/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log('CORS error', error));
```

Server response **without CORS**: ❌

```json
{
  "error": "Blocked by CORS policy"
}
```

***

## **4. Handling CORS in Express**

We use the **CORS middleware** in Express to configure cross-origin requests.

### **A) Allowing All Origins (Unsafe)**

This allows **any domain** to access the API.

```javascript
const express = require('express');
const cors = require('cors'); // Import CORS middleware

const app = express();

app.use(cors()); // Allow all origins

app.get('/data', (req, res) => {
    res.json({ message: "CORS enabled for all origins" });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

✅ **Pros:** Works for all clients\
❌ **Cons:** Not secure, may expose API to malicious sites

***

### **B) Restricting Specific Origins**

We can allow only **trusted domains**:

```javascript
const corsOptions = {
    origin: ['http://myfrontend.com', 'http://admin.myapp.com'], // Allowed origins
};

app.use(cors(corsOptions));
```

✔ Only requests from `http://myfrontend.com` and `http://admin.myapp.com` will be accepted.

***

### **C) Allowing Specific Methods and Headers**

By default, only **GET, POST** requests are allowed. We can enable other methods like **PUT, DELETE**:

```javascript
const corsOptions = {
    origin: 'http://myfrontend.com',
    methods: ['GET', 'POST', 'PUT', 'DELETE'], // Allowed HTTP methods
    allowedHeaders: ['Content-Type', 'Authorization'], // Allowed headers
};

app.use(cors(corsOptions));
```

✅ **Secures the API** by limiting who can access it and how.

***

## **5. Example: Implementing CORS in Express**

📌 **Step 1:** Install CORS package

```bash
npm install cors
```

📌 **Step 2:** Use it in your Express app

```javascript
const express = require('express');
const cors = require('cors');

const app = express();

// Configure CORS
const corsOptions = {
    origin: 'http://myfrontend.com', // Allow only this domain
    methods: ['GET', 'POST'], // Only allow GET and POST
    allowedHeaders: ['Content-Type'], // Allow only this header
};

app.use(cors(corsOptions));

app.get('/data', (req, res) => {
    res.json({ message: "CORS setup is working!" });
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

***

## **6. Conclusion**

* CORS is **important for security** when exposing APIs to frontend applications.
* By default, browsers **block cross-origin requests**, so CORS must be configured.
* Express provides a simple way to handle CORS using the **cors** middleware.
* Always **restrict** origins, methods, and headers to improve security.
