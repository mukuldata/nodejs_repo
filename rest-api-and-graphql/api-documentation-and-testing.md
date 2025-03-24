# API documentation and testing

### **Table of Contents**

1. **What is API Documentation?**
2. **Introduction to Swagger**
3. **Benefits of Using Swagger**
4. **Setting Up Swagger in Node.js (Express.js Example)**
5. **Tools for API Testing (Postman, Insomnia, Thunder Client)**
6. **Example API Testing in Postman**
7. **Summary**

***

### **1. What is API Documentation?**

API documentation is a **guide** that explains how an API works, what endpoints exist, and how to interact with them. Good API documentation helps developers understand how to use an API effectively.

Key elements of API documentation:\
✔ **Endpoints** (`GET /users`, `POST /login`)\
✔ **Request & Response examples**\
✔ **Authentication details**\
✔ **Error codes & responses**

***

### **2. Introduction to Swagger**

**Swagger** is a tool that helps generate **interactive API documentation** for RESTful services. It allows developers to:

* **View API endpoints**
* **Test API requests inside the browser**
* **Understand request/response structures**

Swagger is based on the **OpenAPI Specification (OAS)**, which defines the standard for REST API documentation.

***

### **3. Benefits of Using Swagger**

| Feature                          | Benefit                                             |
| -------------------------------- | --------------------------------------------------- |
| **Auto-generated documentation** | No need to manually write API docs                  |
| **Interactive UI**               | Test APIs directly from the browser                 |
| **Standardized format**          | Uses OpenAPI Specification                          |
| **Supports multiple languages**  | Works with Node.js, Python, Java, etc.              |
| **Easier collaboration**         | Frontend & Backend teams can refer to the same docs |

***

### **4. Setting Up Swagger in Node.js (Express.js Example)**

#### **Step 1: Install Swagger Dependencies**

Run the following command:

```bash
npm install swagger-ui-express swagger-jsdoc
```

#### **Step 2: Configure Swagger in Express.js**

```javascript
const express = require('express');
const swaggerUi = require('swagger-ui-express');
const swaggerJsDoc = require('swagger-jsdoc');

const app = express();

// Swagger configuration
const swaggerOptions = {
  definition: {
    openapi: "3.0.0",
    info: {
      title: "User API",
      version: "1.0.0",
      description: "Simple API documentation using Swagger",
    },
  },
  apis: ["./index.js"], // Define the file where API routes are present
};

const swaggerDocs = swaggerJsDoc(swaggerOptions);
app.use("/api-docs", swaggerUi.serve, swaggerUi.setup(swaggerDocs));

// Sample API route
/**
 * @swagger
 * /users:
 *   get:
 *     summary: Get all users
 *     responses:
 *       200:
 *         description: List of users
 */
app.get('/users', (req, res) => {
  res.json([{ id: 1, name: "John Doe" }]);
});

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
  console.log("Swagger Docs available at http://localhost:3000/api-docs");
});
```

#### **Step 3: View Swagger Documentation**

*   Start the server:

    ```bash
    node index.js
    ```
* Open **`http://localhost:3000/api-docs`** in your browser.
* You’ll see a UI where you can test the `/users` API!

***

### **5. Tools for API Testing (Postman, Insomnia, Thunder Client)**

Apart from Swagger, there are several tools available for **API testing**:

#### **1. Postman**

* **Most popular API testing tool**
* Supports **GET, POST, PUT, DELETE** requests
* Allows saving **collections** of API calls
* Provides **automated testing** features

#### **2. Insomnia**

* Lightweight alternative to Postman
* Simple UI with API testing and authentication support

#### **3. Thunder Client (VS Code Extension)**

* API testing **inside VS Code**
* Best for **quick** request testing

***

### **6. Example API Testing in Postman**

#### **Step 1: Install Postman**

* Download from [https://www.postman.com/downloads/](https://www.postman.com/downloads/)

#### **Step 2: Test a GET Request**

1. Open Postman
2. Enter **`http://localhost:3000/users`**
3. Click **Send**
4.  You should see a JSON response:

    ```json
    [
      {
        "id": 1,
        "name": "John Doe"
      }
    ]
    ```

#### **Step 3: Test a POST Request**

1. Change the request type to **POST**
2. Enter **`http://localhost:3000/users`**
3. Go to **Body → raw** → JSON format
4.  Add data:

    ```json
    {
      "id": 2,
      "name": "Alice"
    }
    ```
5. Click **Send**

***

### **7. Summary**

| Feature                  | Swagger           | Postman     | Insomnia    | Thunder Client |
| ------------------------ | ----------------- | ----------- | ----------- | -------------- |
| **Purpose**              | API Documentation | API Testing | API Testing | API Testing    |
| **Interactive UI**       | ✅                 | ✅           | ✅           | ✅              |
| **Automation**           | ❌                 | ✅           | ❌           | ❌              |
| **Works inside VS Code** | ❌                 | ❌           | ❌           | ✅              |

Swagger helps in **documenting APIs**, while tools like **Postman, Insomnia, and Thunder Client** help in **testing APIs**.&#x20;
