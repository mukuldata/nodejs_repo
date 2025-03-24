# GraphQL Basics

### **Table of Contents**

1. **What is GraphQL?**
2. **GraphQL vs REST API**
3. **GraphQL Schema**
4. **GraphQL Queries**
5. **GraphQL Mutations**
6. **GraphQL Resolvers**
7. **Example Implementation in Node.js with Express**
8. **Summary**

***

### **1. What is GraphQL?**

GraphQL is a **query language** for APIs developed by Facebook. It provides a flexible and efficient way to **fetch, modify, and manage data** compared to traditional REST APIs.

#### **Key Features:**

✔ Fetch **only** the required data (no over-fetching or under-fetching).\
✔ **Single endpoint** (`/graphql`) instead of multiple REST routes.\
✔ Supports **real-time updates** via subscriptions.

***

### **2. GraphQL vs REST API**

| Feature                | REST API                       | GraphQL                              |
| ---------------------- | ------------------------------ | ------------------------------------ |
| **Endpoint Structure** | Multiple (`/users`, `/posts`)  | Single (`/graphql`)                  |
| **Data Fetching**      | Fixed response structure       | Flexible queries                     |
| **Over-fetching**      | Yes (fetches unnecessary data) | No (fetch only requested fields)     |
| **Under-fetching**     | Yes (multiple requests needed) | No (single request fetches all data) |

***

### **3. GraphQL Schema**

A **GraphQL Schema** defines the structure of data that clients can request. It consists of:

* **Types** → Define the shape of data (e.g., `User`, `Post`).
* **Queries** → Read data.
* **Mutations** → Modify data.
* **Resolvers** → Functions that handle GraphQL requests.

#### **Example Schema:**

```graphql
type User {
  id: ID!
  name: String!
  email: String!
}

type Query {
  getUser(id: ID!): User
}

type Mutation {
  createUser(name: String!, email: String!): User
}
```

***

### **4. GraphQL Queries**

#### **What is a Query?**

A **query** is used to **fetch data** from a GraphQL API.

#### **Example Query:**

```graphql
query {
  getUser(id: "1") {
    id
    name
    email
  }
}
```

#### **Response:**

```json
{
  "data": {
    "getUser": {
      "id": "1",
      "name": "Alice",
      "email": "alice@example.com"
    }
  }
}
```

***

### **5. GraphQL Mutations**

#### **What is a Mutation?**

Mutations are used to **create, update, or delete** data in GraphQL.

#### **Example Mutation:**

```graphql
mutation {
  createUser(name: "Bob", email: "bob@example.com") {
    id
    name
    email
  }
}
```

#### **Response:**

```json
{
  "data": {
    "createUser": {
      "id": "2",
      "name": "Bob",
      "email": "bob@example.com"
    }
  }
}
```

***

### **6. GraphQL Resolvers**

#### **What is a Resolver?**

Resolvers are **functions** that handle queries and mutations by fetching or modifying data.

#### **Example Resolver in JavaScript:**

```javascript
const users = [
  { id: "1", name: "Alice", email: "alice@example.com" }
];

const resolvers = {
  Query: {
    getUser: (_, { id }) => users.find(user => user.id === id)
  },
  Mutation: {
    createUser: (_, { name, email }) => {
      const newUser = { id: String(users.length + 1), name, email };
      users.push(newUser);
      return newUser;
    }
  }
};

module.exports = resolvers;
```

***

### **7. Example Implementation in Node.js with Express**

#### **Step 1: Install Required Packages**

Run the following command:

```bash
npm install express graphql express-graphql
```

#### **Step 2: Create a Simple GraphQL Server**

```javascript
const express = require('express');
const { graphqlHTTP } = require('express-graphql');
const { buildSchema } = require('graphql');

const schema = buildSchema(`
  type User {
    id: ID!
    name: String!
    email: String!
  }

  type Query {
    getUser(id: ID!): User
  }

  type Mutation {
    createUser(name: String!, email: String!): User
  }
`);

const users = [{ id: "1", name: "Alice", email: "alice@example.com" }];

const root = {
  getUser: ({ id }) => users.find(user => user.id === id),
  createUser: ({ name, email }) => {
    const newUser = { id: String(users.length + 1), name, email };
    users.push(newUser);
    return newUser;
  }
};

const app = express();
app.use('/graphql', graphqlHTTP({ schema, rootValue: root, graphiql: true }));

app.listen(4000, () => {
  console.log("GraphQL Server running at http://localhost:4000/graphql");
});
```

#### **Step 3: Test Queries in GraphiQL**

* Open **`http://localhost:4000/graphql`**
* Run a query:

```graphql
query {
  getUser(id: "1") {
    name
    email
  }
}
```

***

### **8. Summary**

| Feature       | Description               | Example                                             |
| ------------- | ------------------------- | --------------------------------------------------- |
| **Schema**    | Defines the API structure | `type User { id, name, email }`                     |
| **Query**     | Fetches data              | `getUser(id: "1")`                                  |
| **Mutation**  | Modifies data             | `createUser(name: "Bob", email: "bob@example.com")` |
| **Resolvers** | Handle requests           | `getUser: () => { ... }`                            |
