# DB connections

### **Table of Contents**

1\. **Connecting to MongoDB**\
2\. **Connecting to SQL (MySQL/PostgreSQL)**

***

### **Connecting to MongoDB**

MongoDB is a NoSQL database that stores data in a flexible JSON-like format. To connect to MongoDB in a Node.js application, we use the **Mongoose** library.

#### **1. Install Mongoose**

Run the following command to install Mongoose:

```bash
npm install mongoose
```

#### **2. Connect to MongoDB**

Create a file `database.js` and write the following code:

```javascript
const mongoose = require('mongoose');

const connectDB = async () => {
    try {
        await mongoose.connect('mongodb://localhost:27017/mydatabase', {
            useNewUrlParser: true,
            useUnifiedTopology: true
        });
        console.log('MongoDB connected successfully');
    } catch (error) {
        console.error('MongoDB connection failed:', error);
        process.exit(1); // Exit process with failure
    }
};

connectDB();
```

#### **Explanation:**

* `mongoose.connect()` establishes a connection to the MongoDB database.
* `useNewUrlParser: true` and `useUnifiedTopology: true` help handle MongoDB driver updates.
* If the connection fails, it logs an error and exits the process.

***

### **Connecting to SQL (MySQL/PostgreSQL)**

SQL databases like MySQL and PostgreSQL store structured data using tables. We use the **mysql2** package for MySQL and **pg** package for PostgreSQL.

#### **1. Install Required Packages**

For MySQL:

```bash
npm install mysql2
```

For PostgreSQL:

```bash
npm install pg
```

#### **2. Connect to MySQL**

Create a file `mysqlConnection.js` and write:

```javascript
const mysql = require('mysql2');

const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'password',
    database: 'mydatabase'
});

connection.connect((err) => {
    if (err) {
        console.error('MySQL connection failed:', err);
        return;
    }
    console.log('MySQL connected successfully');
});

module.exports = connection;
```

#### **Explanation:**

* The `mysql.createConnection()` method sets up a MySQL connection.
* If there is an error, it logs the error message.

***

#### **3. Connect to PostgreSQL**

Create a file `pgConnection.js` and write:

```javascript
const { Client } = require('pg');

const client = new Client({
    host: 'localhost',
    user: 'postgres',
    password: 'password',
    database: 'mydatabase',
    port: 5432
});

client.connect()
    .then(() => console.log('PostgreSQL connected successfully'))
    .catch(err => console.error('PostgreSQL connection failed:', err));

module.exports = client;
```

#### **Explanation:**

* We use the `pg` package to create a connection with PostgreSQL.
* The `client.connect()` method connects to the database and logs success or failure.

***

#### **Summary:**

* MongoDB uses **Mongoose** to connect and interact with a NoSQL database.
* MySQL and PostgreSQL use **mysql2** and **pg** libraries, respectively, for structured data storage.
* Proper error handling ensures smooth database connections.
