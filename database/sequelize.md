# Day 3 — ORM and Sequelize


---

## 3.1 What is an ORM?

**ORM (Object-Relational Mapper)** is a tool that allows a programming language to communicate with a database using **objects and functions instead of writing SQL queries manually**.

In our project, we use **Sequelize** as the ORM, and Sequelize communicates with **MySQL** in the background.

### Simple idea

Without ORM:

```sql
SELECT * FROM users WHERE id = 1;
```

With Sequelize:

```javascript
const user = await User.findByPk(1);
```

Sequelize takes the JavaScript code and generates the appropriate SQL query for MySQL.

### Analogy — Translator

Imagine that MySQL only understands one language: **SQL**.

Your Node.js application speaks **JavaScript**.

Sequelize acts like a **translator**:

```text
JavaScript Application
        ↓
     Sequelize
        ↓
       SQL
        ↓
      MySQL
```

You write JavaScript, and Sequelize translates it into SQL.

---

# 3.2 Why Use Sequelize When SQL Already Works?

Raw SQL works perfectly well. We used SQL directly on Day 1.

So why add Sequelize?

### 1. Write JavaScript instead of SQL

Instead of:

```sql
INSERT INTO users (name, email)
VALUES ('John', 'john@example.com');
```

You can write:

```javascript
await User.create({
  name: 'John',
  email: 'john@example.com'
});
```

Sequelize generates the SQL automatically.

---

### 2. Less repetitive code

Without Sequelize, you may repeatedly write SQL queries:

```javascript
connection.query(
  "SELECT * FROM users WHERE id = ?",
  [userId]
);
```

With Sequelize:

```javascript
User.findByPk(userId);
```

The model already knows what a `User` looks like.

---

### 3. Relationships become easier

On Day 1 we manually wrote:

```sql
SELECT users.name, resumes.tittle
FROM resumes
JOIN users
ON resumes.userId = users.id;
```

With Sequelize, we can define the relationship once:

```javascript
User.hasMany(Resume);
Resume.belongsTo(User);
```

Then Sequelize can handle the relationship when we request related data.

---

### 4. Helps protect against SQL injection

When Sequelize receives values through its query APIs, it handles parameterization/escaping rather than requiring us to manually concatenate user input into SQL strings.

For example, avoid:

```javascript
// Bad practice
const sql = `SELECT * FROM users WHERE email = '${email}'`;
```

Instead:

```javascript
const user = await User.findOne({
  where: {
    email: email
  }
});
```

This is safer because user input isn't manually inserted into the SQL string.

---

### 5. One model definition can be reused

A model describes the structure of a table once.

```javascript
const User = sequelize.define('User', {
  name: DataTypes.STRING,
  email: DataTypes.STRING
});
```

Now the entire application can reuse `User`.

---

### 6. Database portability

Sequelize supports multiple SQL databases, including:

* MySQL
* PostgreSQL
* SQLite
* Microsoft SQL Server

So much of the application-side code can remain the same while the database configuration changes.

---

# 3.3 ORM Architecture

The complete flow looks like this:

```text
┌──────────────────────────────────────────┐
│          JavaScript Application          │
│                                          │
│      Objects, functions, business logic  │
└────────────────────┬─────────────────────┘
                     │
                     │ JavaScript
                     ▼
┌──────────────────────────────────────────┐
│                Sequelize                 │
│                                          │
│     JavaScript objects → SQL queries     │
│     SQL results → JavaScript objects     │
└────────────────────┬─────────────────────┘
                     │
                     │ SQL
                     ▼
┌──────────────────────────────────────────┐
│                  MySQL                   │
│                                          │
│             Tables and rows              │
└──────────────────────────────────────────┘
```

### In simple words

```text
Your Code
   ↓
Sequelize
   ↓
SQL
   ↓
MySQL
```

And when data comes back:

```text
MySQL
   ↓
SQL Result
   ↓
Sequelize
   ↓
JavaScript Object
```

---

# 3.4 What is Sequelize?

**Sequelize** is an **ORM for Node.js** that makes it easier to work with relational databases.

Instead of constantly writing SQL, we can work with JavaScript models and methods.

### Important Sequelize features

* Database models
* Automatic SQL query generation
* Associations/relationships
* Validation
* Constraints
* Transactions
* Migrations
* Support for multiple SQL databases

---

# 3.5 Sequelize Model

A **model** is a JavaScript representation of a database table.

For example, our MySQL `users` table contains:

```text
users
────────────────────
id
name
email
```

We can represent it in Sequelize like this:

```javascript
const User = sequelize.define('User', {
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },

  name: {
    type: DataTypes.STRING,
    allowNull: false
  },

  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  }
});
```

Now `User` represents the `users` table.

We can use it to perform database operations.

---

# 3.6 Raw SQL vs Sequelize

## Without ORM

Suppose we want to find a user by ID.

We might write:

```javascript
const result = await connection.query(
  "SELECT * FROM users WHERE id = ?",
  [userId]
);
```

Then we work with the returned database result.

This means our application contains SQL strings directly.

---

## With Sequelize

We can simply write:

```javascript
const user = await User.findByPk(userId);
```

Sequelize generates the appropriate SQL internally.

Conceptually, it performs something similar to:

```sql
SELECT *
FROM users
WHERE id = ?;
```

The application doesn't have to manually construct that SQL.

---

# 3.7 Reading Data With Sequelize

### Find one user by primary key

```javascript
const user = await User.findByPk(1);

console.log(user.name);
console.log(user.email);
```

`findByPk()` means:

> Find a record using its Primary Key.

---

### Find one user using conditions

```javascript
const user = await User.findOne({
  where: {
    email: 'john@example.com'
  }
});
```

This is conceptually similar to:

```sql
SELECT *
FROM users
WHERE email = 'john@example.com'
LIMIT 1;
```

---

### Find multiple users

```javascript
const users = await User.findAll();
```

With a condition:

```javascript
const users = await User.findAll({
  where: {
    isActive: true
  }
});
```

Conceptually:

```sql
SELECT *
FROM users
WHERE isActive = true;
```

---

# 3.8 CRUD Operations

CRUD means:

| Operation | Meaning | Sequelize                              |
| --------- | ------- | -------------------------------------- |
| **C**     | Create  | `create()`                             |
| **R**     | Read    | `findByPk()`, `findOne()`, `findAll()` |
| **U**     | Update  | `update()`                             |
| **D**     | Delete  | `destroy()`                            |

---

## CREATE

```javascript
const user = await User.create({
  name: 'John Doe',
  email: 'john@example.com'
});
```

Similar SQL:

```sql
INSERT INTO users (name, email)
VALUES ('John Doe', 'john@example.com');
```

---

## READ

```javascript
const user = await User.findByPk(1);
```

Similar SQL:

```sql
SELECT *
FROM users
WHERE id = 1;
```

---

## UPDATE

```javascript
await user.update({
  name: 'Jane Doe'
});
```

Similar SQL:

```sql
UPDATE users
SET name = 'Jane Doe'
WHERE id = 1;
```

---

## DELETE

```javascript
await user.destroy();
```

Similar SQL:

```sql
DELETE FROM users
WHERE id = 1;
```

---

# 3.9 Associations — Relationships Between Models

On Day 1, we learned:

```text
users
   │
   │  userId
   ▼
resumes
```

One user can have multiple resumes.

This is called a **One-to-Many relationship**.

In Sequelize:

```javascript
User.hasMany(Resume);
Resume.belongsTo(User);
```

This tells Sequelize:

> One User can have many Resumes.

and:

> Every Resume belongs to one User.

---

## Example

```javascript
User.hasMany(Resume, {
  foreignKey: 'userId'
});

Resume.belongsTo(User, {
  foreignKey: 'userId'
});
```

The `userId` connects the two tables.

```text
users
──────────────
id
name
email
   │
   │
   │ userId
   ▼
resumes
──────────────
id
userId
title
```

---

# 3.10 Getting Related Data

Once the association is defined, Sequelize can load related records.

```javascript
const userWithResumes = await User.findOne({
  where: {
    id: userId
  },
  include: [Resume]
});
```

The result can contain:

```javascript
{
  id: 1,
  name: "Unishka",
  email: "unishka@gmail.com",
  Resumes: [
    {
      id: 1,
      userId: 1,
      title: "Software Engineer"
    },
    {
      id: 2,
      userId: 1,
      title: "Sales Manager"
    }
  ]
}
```

Instead of manually writing a `JOIN`, Sequelize uses the association to generate the required SQL.

---

# 3.11 Many-to-Many Relationships

Sometimes one record can be connected to many records on both sides.

Example:

```text
User ←→ Project
```

One user can work on many projects.

One project can have many users.

This is a **Many-to-Many relationship**.

Usually, a third table is needed.

```text
users
  │
  │
  ▼
user_projects
  ▲
  │
  │
projects
```

In Sequelize:

```javascript
User.belongsToMany(Project, {
  through: 'UserProject'
});

Project.belongsToMany(User, {
  through: 'UserProject'
});
```

`UserProject` is the **junction/intermediate table**.

---

# 3.12 Why Associations Matter

Without associations, you would repeatedly have to manually think about:

```text
Which foreign key connects these tables?
Which table should I JOIN?
What is the relationship?
What condition should I use?
```

With Sequelize:

```javascript
User.hasMany(Resume);
Resume.belongsTo(User);
```

The relationship is defined once.

Then Sequelize can use it throughout the application.

---

# 3.13 Complete Example

Let's create a simple `User` model.

```javascript
const User = sequelize.define('User', {
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },

  name: {
    type: DataTypes.STRING,
    allowNull: false
  },

  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  },

  isActive: {
    type: DataTypes.BOOLEAN,
    defaultValue: true
  },

  role: {
    type: DataTypes.ENUM('admin', 'user', 'guest'),
    defaultValue: 'user'
  }
});
```

Now we can perform all four CRUD operations.

### Create

```javascript
const user = await User.create({
  name: 'Divyanshu Shah',
  email: 'divyanshu@example.com',
  role: 'user'
});
```

### Read

```javascript
const user = await User.findByPk(1);
```

### Update

```javascript
await user.update({
  name: 'Divyanshu Shah'
});
```

### Delete

```javascript
await user.destroy();
```

---

# 3.14 Sequelize vs Raw SQL

| Raw SQL                                         | Sequelize                         |
| ----------------------------------------------- | --------------------------------- |
| Write SQL manually                              | Write JavaScript methods          |
| More SQL code                                   | Less repetitive code              |
| Manually manage queries                         | Sequelize generates queries       |
| Manually manage relationships                   | Associations define relationships |
| Database-specific SQL can appear throughout app | ORM provides abstraction          |
| More control over exact SQL                     | Easier application development    |

### Important

Sequelize does **not** mean SQL disappears.

It is still using SQL underneath.

```text
Sequelize
    ↓
generates SQL
    ↓
MySQL executes SQL
    ↓
result
    ↓
Sequelize
    ↓
JavaScript object
```

So understanding SQL is still extremely important even when using Sequelize.

---

# 3.15 Day 3 Key Terms

| Term                  | One-line meaning                                            |
| --------------------- | ----------------------------------------------------------- |
| **ORM**               | Tool that connects programming objects with database tables |
| **Sequelize**         | Node.js ORM for SQL databases                               |
| **Model**             | JavaScript representation of a database table               |
| **Association**       | Relationship between Sequelize models                       |
| **`create()`**        | Inserts a new record                                        |
| **`findByPk()`**      | Finds a record by primary key                               |
| **`findOne()`**       | Finds one matching record                                   |
| **`findAll()`**       | Finds multiple records                                      |
| **`update()`**        | Changes an existing record                                  |
| **`destroy()`**       | Deletes a record                                            |
| **`hasMany()`**       | Defines one-to-many relationship                            |
| **`belongsTo()`**     | Defines that a model belongs to another model               |
| **`belongsToMany()`** | Defines many-to-many relationship                           |
| **`include`**         | Loads related records                                       |

---

# Day 3 — Final Recap

```text
                 JavaScript Application
                          │
                          ▼
                     Sequelize
                          │
                    Generates SQL
                          │
                          ▼
                        MySQL
                          │
                    Stores the data
                          │
                          ▼
                     SQL Result
                          │
                          ▼
                     Sequelize
                          │
                          ▼
                 JavaScript Object
```

### The main things learned today:

* **ORM** allows JavaScript code to communicate with SQL databases.
* **Sequelize** is the ORM used with Node.js.
* A **model represents a database table**.
* Sequelize provides methods for **CRUD operations**.
* **Associations** represent relationships between tables.
* `hasMany()` and `belongsTo()` are commonly used for **one-to-many** relationships.
* `belongsToMany()` is used for **many-to-many** relationships.
* `include` can load related records.
* Sequelize generates SQL behind the scenes.
* Knowing **SQL is still important**, because Sequelize ultimately communicates with the database using SQL.

