# SQL operations

### **Table of Contents**

1. **Inserting Data (`INSERT`)**
2. **Retrieving Data (`SELECT`)**
3. **Updating Data (`UPDATE`)**
4. **Deleting Data (`DELETE`)**

***

### **1. Inserting Data (`INSERT`)**

#### **Explanation:**

The `INSERT` statement is used to add new records (rows) to a table.

#### **Example:**

```sql
INSERT INTO users (name, email, age)  
VALUES ('John Doe', 'john@example.com', 25);
```

#### **What Happens?**

* A new row is added to the `users` table with values for `name`, `email`, and `age`.

***

### **2. Retrieving Data (`SELECT`)**

#### **Explanation:**

The `SELECT` statement is used to fetch records from a table.

#### **Example:**

```sql
SELECT * FROM users;
```

#### **What Happens?**

* Retrieves all rows from the `users` table.
*   To fetch specific columns:

    ```sql
    SELECT name, email FROM users;
    ```
*   To filter results:

    ```sql
    SELECT * FROM users WHERE age = 25;
    ```

***

### **3. Updating Data (`UPDATE`)**

#### **Explanation:**

The `UPDATE` statement modifies existing records in a table.

#### **Example:**

```sql
UPDATE users  
SET age = 26  
WHERE name = 'John Doe';
```

#### **What Happens?**

* Finds the row where `name` is `"John Doe"` and updates `age` to 26.

***

### **4. Deleting Data (`DELETE`)**

#### **Explanation:**

The `DELETE` statement removes records from a table.

#### **Example:**

```sql
DELETE FROM users  
WHERE name = 'John Doe';
```

#### **What Happens?**

* Finds and deletes the row where `name` is `"John Doe"`.

***

#### **Summary:**

* **`INSERT`** → Adds new records.
* **`SELECT`** → Retrieves data.
* **`UPDATE`** → Modifies existing records.
* **`DELETE`** → Removes records.
