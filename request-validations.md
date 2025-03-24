# Request Validations

## **Request Validation in Express.js (Joi & Express-Validator)**

### **Table of Contents**

1. **What is Request Validation?**
2. **Why is Request Validation Important?**
3. **Joi for Validation**
   * What is Joi?
   * Installing Joi
   * Example of Using Joi
4. **Express-Validator for Validation**
   * What is Express-Validator?
   * Installing Express-Validator
   * Example of Using Express-Validator
5. **Comparing Joi and Express-Validator**
6. **Conclusion**

***

## **1. What is Request Validation?**

Request validation ensures that incoming data (from users or APIs) is **correct, secure, and complete** before processing.

🚀 **Example:** If a user submits a registration form, we should check:\
✅ **Email is valid**\
✅ **Password is strong**\
✅ **Username is not empty**

***

## **2. Why is Request Validation Important?**

* **Prevents errors** → Avoids crashes due to invalid data
* **Improves security** → Blocks malicious input (e.g., SQL injection)
* **Ensures data integrity** → Only correct data is stored

***

## **3. Joi for Validation**

#### **What is Joi?**

Joi is a powerful **schema-based validation library** that helps define data rules and validate user input.

#### **Installing Joi**

```bash
npm install joi
```

#### **Example: Using Joi for Validation**

```javascript
const express = require('express');
const Joi = require('joi'); // Import Joi

const app = express();
app.use(express.json());

// Define a validation schema
const userSchema = Joi.object({
    name: Joi.string().min(3).required(),
    email: Joi.string().email().required(),
    age: Joi.number().integer().min(18).required(),
});

app.post('/register', (req, res) => {
    const { error } = userSchema.validate(req.body);

    if (error) {
        return res.status(400).json({ message: error.details[0].message });
    }

    res.send('User registered successfully!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

✔ **If data is incorrect, an error message is returned**\
✔ **If data is valid, the user is registered**

#### **Example Input & Output**

✅ **Valid Request:**

```json
{
  "name": "John",
  "email": "john@example.com",
  "age": 25
}
```

🔄 **Response:** `"User registered successfully!"`

❌ **Invalid Request:**

```json
{
  "name": "Jo",
  "email": "invalid-email",
  "age": 16
}
```

🔄 **Response:** `"Age must be greater than or equal to 18"`

***

## **4. Express-Validator for Validation**

#### **What is Express-Validator?**

Express-Validator is a **middleware-based** validation library that validates request data using predefined rules.

#### **Installing Express-Validator**

```bash
npm install express-validator
```

#### **Example: Using Express-Validator**

```javascript
const express = require('express');
const { body, validationResult } = require('express-validator');

const app = express();
app.use(express.json());

app.post('/register',
    [
        body('name').isLength({ min: 3 }).withMessage('Name must be at least 3 characters long'),
        body('email').isEmail().withMessage('Invalid email format'),
        body('age').isInt({ min: 18 }).withMessage('Age must be 18 or above'),
    ],
    (req, res) => {
        const errors = validationResult(req);

        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }

        res.send('User registered successfully!');
    }
);

app.listen(3000, () => console.log('Server running on port 3000'));
```

#### **Example Input & Output**

✅ **Valid Request:**

```json
{
  "name": "Alice",
  "email": "alice@example.com",
  "age": 22
}
```

🔄 **Response:** `"User registered successfully!"`

❌ **Invalid Request:**

```json
{
  "name": "A",
  "email": "wrong-email",
  "age": 15
}
```

🔄 **Response:**

```json
{
  "errors": [
    { "msg": "Name must be at least 3 characters long" },
    { "msg": "Invalid email format" },
    { "msg": "Age must be 18 or above" }
  ]
}
```

***

## **5. Comparing Joi and Express-Validator**

| Feature            | Joi                      | Express-Validator           |
| ------------------ | ------------------------ | --------------------------- |
| **Type**           | Schema-based validation  | Middleware-based validation |
| **Ease of Use**    | Easy for structured data | Better for request handling |
| **Error Handling** | Detailed error messages  | List of validation errors   |
| **Best For**       | API data validation      | Form validation in Express  |

***

## **6. Conclusion**

* **Use Joi** when working with structured data (e.g., user profiles, database entries).
* **Use Express-Validator** for handling form data validation directly in Express routes.
* Both libraries **ensure user input is safe and valid**, preventing security vulnerabilities.

🚀 **Which one should you use?**\
✔ If your project requires flexible validation with custom error messages → **Use Joi**\
✔ If you need middleware-based validation inside Express routes → **Use Express-Validator**
