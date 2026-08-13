# Day 2 — RDBMS, SQL vs NoSQL & Data Types

## Table of Contents

* [2.1 DBMS vs RDBMS](#21-dbms-vs-rdbms)
* [2.2 SQL vs NoSQL](#22-sql-vs-nosql)
* [2.3 Structured vs Unstructured Data](#23-structured-vs-unstructured-data)
* [2.4 CHAR vs VARCHAR vs STRING](#24-char-vs-varchar-vs-string)
* [2.5 ENUM](#25-enum)
* [2.6 BOOLEAN](#26-boolean)
* [2.7 TINYINT](#27-tinyint)
* [2.8 Quick Comparison](#28-quick-comparison)
* [2.9 Key Takeaways](#29-key-takeaways)

---

# 2.1 DBMS vs RDBMS

## What is a DBMS?

**DBMS (Database Management System)** is software used to store, manage, retrieve, and modify data.

A DBMS provides tools for applications and users to work with stored data.

A DBMS does not necessarily organize data into related tables.

---

## What is an RDBMS?

**RDBMS (Relational Database Management System)** is a type of DBMS that stores data using **tables** and allows those tables to be connected through relationships.

MySQL is an **RDBMS**.

Other examples include:

* MySQL
* PostgreSQL
* Oracle Database
* Microsoft SQL Server

The important idea is that relational databases organize data into tables and use relationships such as **primary keys** and **foreign keys** to connect related records.

---

## DBMS vs RDBMS

| Feature        | DBMS                                                  | RDBMS                               |
| -------------- | ----------------------------------------------------- | ----------------------------------- |
| Structure      | Can have different data models                        | Table-based relational model        |
| Relationships  | May have limited relationships                        | Strong relationships between tables |
| Foreign Keys   | Not necessarily supported                             | Supported                           |
| SQL            | Depends on the system                                 | Commonly uses SQL                   |
| Data Integrity | Depends on implementation                             | Strong integrity constraints        |
| Transactions   | Depends on implementation                             | Strong transaction support          |
| Examples       | File-based database systems, some specialized systems | MySQL, PostgreSQL, Oracle           |

> **Important:** DBMS is the broader category. RDBMS is a specific type of DBMS.

---

## Why MySQL is an RDBMS

On Day 1, we created:

```text
users
   │
   │ userId
   ▼
resumes
```

The `resumes` table contains:

```sql
FOREIGN KEY (userId)
REFERENCES users(id)
```

This relationship between tables is one of the defining features of a relational database.

Therefore:

```text
MySQL
  ↓
DBMS
  ↓
RDBMS
```

---

## Analogy — Library vs Filing Cabinet

Imagine a room containing books.

A general DBMS can be thought of as a system for managing those books without necessarily enforcing a strict relational structure.

An RDBMS is more like an organized filing cabinet:

```text
Users Drawer
    ↓
User records

Resumes Drawer
    ↓
Resume records

Reference Card
    ↓
Connects a resume to its user
```

The relationships are explicitly defined.

---

# 2.2 SQL vs NoSQL

There are two broad approaches commonly discussed when choosing a database:

```text
SQL / Relational
        vs
NoSQL / Non-relational
```

---

## SQL Databases

**SQL databases** generally use the relational model.

Data is stored in tables containing:

* Rows
* Columns
* Relationships

Example:

```text
users
+----+----------+---------------------+
| id | name     | email               |
+----+----------+---------------------+
| 1  | Divyanshu | divyanshu@gmail.com |
| 2  | Vijay     | vijay@gmail.com     |
+----+----------+---------------------+
```

The structure is usually defined before data is inserted.

Examples:

* MySQL
* PostgreSQL
* Oracle
* SQL Server

---

## NoSQL Databases

**NoSQL** is a broad category of non-relational database systems.

Different NoSQL databases can use different data models, including:

* Documents
* Key-value pairs
* Wide-column data
* Graphs

Examples include:

* MongoDB
* Redis
* Cassandra
* DynamoDB

A document database might store something like:

```json
{
  "userId": 123,
  "name": "Divyanshu",
  "email": "divyanshu@gmail.com",
  "skills": [
    "JavaScript",
    "Node.js",
    "MySQL"
  ]
}
```

Another document could contain completely different fields:

```json
{
  "userId": 456,
  "name": "Vijay",
  "profileImage": "profile.jpg",
  "socialLinks": {
    "github": "vijay123"
  }
}
```

This flexibility is one reason document-oriented NoSQL databases can be useful when data structures change frequently.

---

## SQL vs NoSQL Comparison

| Aspect         | SQL                                        | NoSQL                                            |
| -------------- | ------------------------------------------ | ------------------------------------------------ |
| Data model     | Tables                                     | Documents, key-value, graph, etc.                |
| Schema         | Usually predefined                         | Often more flexible                              |
| Relationships  | Foreign keys and JOINs                     | Often embedded or application-managed references |
| Query language | SQL                                        | Depends on database                              |
| Transactions   | Strong transaction support                 | Depends on database                              |
| Scaling        | Can scale vertically and also horizontally | Often designed with horizontal scaling in mind   |
| Best fit       | Structured relational data                 | Flexible or highly variable data                 |
| Examples       | MySQL, PostgreSQL                          | MongoDB, Redis, Cassandra                        |

---

## Important Point About Scaling

A common beginner explanation is:

```text
SQL → vertical scaling
NoSQL → horizontal scaling
```

This is too simplistic.

Both relational and NoSQL databases can support different scaling strategies depending on the specific database and architecture.

A better way to remember it is:

> **SQL databases are especially strong when relationships, consistency, transactions, and structured querying are important. NoSQL databases are often useful when flexible data models or particular large-scale access patterns are important.**

---

# 2.3 Structured vs Unstructured Data

## Structured Data

**Structured data** follows a predefined format.

For example, an employee table:

```text
employees

id
name
email
salary
department
hireDate
```

Every employee record follows the same general structure.

Example:

| id | name      | email                                             | salary | department |
| -: | --------- | ------------------------------------------------- | -----: | ---------- |
|  1 | Divyanshu | [divyanshu@gmail.com](mailto:divyanshu@gmail.com) |  50000 | IT         |
|  2 | Vijay     | [vijay@gmail.com](mailto:vijay@gmail.com)         |  60000 | HR         |

The structure is predictable.

---

## Characteristics of Structured Data

Structured data usually has:

* A predefined schema
* Rows and columns
* Consistent fields
* Predictable data types
* Efficient querying
* Easy indexing
* Strong validation

---

## Example

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    salary DECIMAL(10, 2),
    department VARCHAR(50),
    hireDate DATE
);
```

Every employee is expected to have these columns.

---

## Advantages

Structured data provides:

* Efficient querying
* Strong data consistency
* Validation
* Relationships between records
* Indexing
* Easier reporting
* Easier analytics

This is one of the reasons relational databases are widely used for systems such as:

```text
Banking
E-commerce
Payroll
Orders
User accounts
Inventory
```

---

# Unstructured Data

**Unstructured data** does not naturally fit into a fixed row-and-column structure.

Examples include:

* Images
* Videos
* Audio
* Documents
* Emails
* Social media content
* Free-form text

For example, two social media posts might contain completely different information.

### Post 1

```json
{
  "userId": 123,
  "content": "Just had coffee!",
  "timestamp": "2026-08-13T10:30:00Z",
  "likes": 45
}
```

### Post 2

```json
{
  "userId": 456,
  "content": "Check out this photo",
  "image": "photo.jpg",
  "likes": 200,
  "comments": [
    {
      "userId": 789,
      "text": "Nice!"
    }
  ],
  "hashtags": [
    "photography",
    "nature"
  ]
}
```

The two records don't necessarily have exactly the same fields.

---

## Characteristics of Unstructured Data

Unstructured data may have:

* No fixed schema
* Different fields between records
* Text
* Images
* Videos
* Audio
* Large amounts of information
* High variety

---

## Advantages

Flexible data models can be useful when:

* Requirements change frequently
* Records have different shapes
* The application handles many types of content
* Rapid iteration is important
* Large-scale distributed storage is required

---

## Disadvantages

Unstructured or flexible data can make some tasks more difficult:

* Querying can be more complicated
* Data consistency may require more application logic
* Reporting can be harder
* Validation may be less centralized
* Relationships may require additional design

---

## Analogy — Printed Form vs Diary

### Structured Data

A printed government form:

```text
Name: ___________
Age: ____________
Email: __________
Address: ________
```

Every form has the same fields.

### Unstructured Data

A diary:

```text
Today I went to college...
```

Tomorrow's entry might contain:

```text
A long story
A photo
A drawing
A list
A few sentences
```

The structure can change from entry to entry.

---

# Structured vs Unstructured

| Aspect         | Structured               | Unstructured                         |
| -------------- | ------------------------ | ------------------------------------ |
| Format         | Fixed                    | Flexible                             |
| Schema         | Predefined               | Often no fixed schema                |
| Data shape     | Consistent               | Can vary                             |
| Querying       | Generally easier         | Often more complex                   |
| Validation     | Strong                   | May require more application logic   |
| Examples       | Employee records, orders | Images, videos, emails               |
| Common storage | SQL databases            | Files, object storage, NoSQL systems |

> **Note:** "Unstructured" does not mean "cannot be stored in a database." Modern databases can store many different types of data.

---

# 2.4 CHAR vs VARCHAR vs STRING

Text data can be represented in different ways.

Three terms we need to understand are:

```text
CHAR
VARCHAR
STRING
```

---

## CHAR

**`CHAR(size)`** is a fixed-length character type.

Example:

```sql
code CHAR(10)
```

If the value is:

```text
"Deepesh"
```

the column has a fixed length of 10 characters.

Conceptually:

```text
D | e | e | p | e | s | h | _ | _ | _
```

The important idea is:

> **CHAR is designed for values whose lengths are consistently fixed or nearly fixed.**

Examples:

```text
Country code
Currency code
Fixed-length identifiers
```

For example:

```text
IN
US
UK
```

---

# VARCHAR

**`VARCHAR(size)`** is a variable-length character type.

Example:

```sql
name VARCHAR(50)
```

If we store:

```text
"Divyanshu"
```

the actual value is only as long as necessary, subject to the column's maximum length and the database's character semantics.

Use `VARCHAR` for values whose lengths vary.

Examples:

```text
Names
Emails
Addresses
Usernames
Titles
```

---

## CHAR vs VARCHAR

| Feature       | CHAR              | VARCHAR            |
| ------------- | ----------------- | ------------------ |
| Length        | Fixed             | Variable           |
| Best for      | Fixed-size values | Variable-size text |
| Example       | Country code      | Name               |
| Typical usage | `CHAR(2)`         | `VARCHAR(100)`     |

### Rule of Thumb

Use:

```text
CHAR
```

when the value has a genuinely fixed length.

Use:

```text
VARCHAR
```

when the length varies.

---

# What is STRING?

`STRING` is **not a native MySQL data type**.

For example, Sequelize provides:

```javascript
DataTypes.STRING
```

When using Sequelize with MySQL, this generally maps to a `VARCHAR` column.

For example:

```javascript
const User = sequelize.define('User', {
  name: DataTypes.STRING
});
```

This represents a string/text field on the JavaScript/Sequelize side.

The underlying MySQL column uses a MySQL-supported type such as `VARCHAR`.

### Simple Way to Remember

```text
JavaScript / Sequelize
        ↓
DataTypes.STRING
        ↓
MySQL
        ↓
VARCHAR
```

So:

> **STRING is commonly the application/ORM terminology, while VARCHAR is the MySQL data type.**

---

# 2.5 ENUM

**`ENUM`** allows a column to accept one value from a predefined list.

Example:

```sql
CREATE TABLE posts (
    id INT PRIMARY KEY,
    title VARCHAR(255),
    status ENUM('active', 'pending', 'deleted')
);
```

The `status` column can contain:

```text
active
pending
deleted
```

but not arbitrary values outside the defined set when strict validation is enforced.

---

## Where ENUM Can Be Useful

ENUM can be useful for small, stable sets of values such as:

```text
status
role
category
```

Example:

```text
Role:
admin
user
guest
```

Or:

```text
Status:
active
pending
deleted
```

---

## Sequelize ENUM

Sequelize can represent the same concept:

```javascript
const Post = sequelize.define('Post', {
  title: DataTypes.STRING,

  status: DataTypes.ENUM(
    'active',
    'pending',
    'deleted'
  )
});
```

---

## ENUM Advantages

* Restricts allowed values
* Helps maintain consistency
* Makes the intended values explicit
* Can be useful for small, stable sets of options

---

## ENUM Disadvantages

ENUM is not always the best choice.

If the list of values changes frequently, a separate table can sometimes be a better design.

For example, instead of:

```text
ENUM('admin', 'user', 'guest')
```

a separate roles table could be used:

```text
roles
-----
id
name
```

This makes the available roles data rather than part of the table definition.

### Rule of Thumb

> Use ENUM when the allowed choices are small and relatively stable.

---

# 2.6 BOOLEAN

**BOOLEAN** represents a true/false value.

Examples:

```text
isVerified
isActive
isPremium
termsAccepted
```

Example table:

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100),
    isVerified BOOLEAN,
    isActive BOOLEAN
);
```

The values conceptually are:

```text
true
false
```

---

## Sequelize BOOLEAN

```javascript
const User = sequelize.define('User', {
  email: DataTypes.STRING,

  isVerified: DataTypes.BOOLEAN,

  termsAccepted: DataTypes.BOOLEAN,

  isActive: DataTypes.BOOLEAN
});
```

A default value can also be specified:

```javascript
isActive: {
  type: DataTypes.BOOLEAN,
  defaultValue: true
}
```

---

## MySQL BOOLEAN

In MySQL, `BOOLEAN` is essentially an alias for:

```text
TINYINT(1)
```

So conceptually:

```text
true  → 1
false → 0
```

This is an important detail when inspecting MySQL data.

---

# 2.7 TINYINT

**`TINYINT`** is a small integer data type.

For a signed `TINYINT`, the range is:

```text
-128 to 127
```

For an unsigned `TINYINT`:

```text
0 to 255
```

It requires only **1 byte** of storage.

---

## Example

```sql
CREATE TABLE ratings (
    id INT PRIMARY KEY,
    stars TINYINT,
    helpful TINYINT
);
```

Possible data:

```text
stars   = 5
helpful = 1
```

---

## Common Uses

TINYINT can be useful for:

```text
Small ratings
Small counters
Flags
Small numeric values
```

For example:

```text
1 → Poor
2 → Below average
3 → Average
4 → Good
5 → Excellent
```

A rating does not need a full `INT` if the application only needs values from 1 to 5.

---

## Storage Difference

| Type      | Approximate Storage |
| --------- | ------------------: |
| `TINYINT` |              1 byte |
| `INT`     |             4 bytes |

When a table contains millions of rows, choosing appropriate data types can have meaningful effects on storage.

---

# 2.8 Quick Comparison

## Text Types

| Type      | Meaning                   | Example            |
| --------- | ------------------------- | ------------------ |
| `CHAR`    | Fixed-length text         | Country code       |
| `VARCHAR` | Variable-length text      | Name, email        |
| `STRING`  | Common Sequelize/ORM name | `DataTypes.STRING` |

---

## Specialized Types

| Type      | Purpose                           | Example                        |
| --------- | --------------------------------- | ------------------------------ |
| `ENUM`    | One value from predefined options | `active`, `pending`, `deleted` |
| `BOOLEAN` | True/false                        | `isActive`                     |
| `TINYINT` | Small integer                     | Rating `1–5`                   |

---

# 2.9 Key Takeaways

### DBMS

```text
Software for managing databases
```

### RDBMS

```text
DBMS based on the relational/table model
```

MySQL is an RDBMS.

---

### SQL

Best suited to systems where structured data, relationships, transactions, and consistency are important.

```text
Users
  ↓
Orders
  ↓
Products
```

Relationships can be explicitly represented using foreign keys.

---

### NoSQL

A broad family of non-relational databases that can provide flexible data models and are often chosen for particular scalability or data-access requirements.

---

### Structured Data

```text
Fixed structure
Rows + columns
Predictable schema
```

Example:

```text
Employee
├── id
├── name
├── email
└── salary
```

---

### Unstructured Data

```text
Flexible / non-tabular data
Images
Videos
Audio
Free-form text
```

---

### CHAR

```text
Fixed-length text
```

### VARCHAR

```text
Variable-length text
```

### STRING

```text
Common ORM/JavaScript terminology
```

### ENUM

```text
One value from a predefined set
```

### BOOLEAN

```text
true / false
```

### TINYINT

```text
Small integer
```

---

# 🎯 Day 2 Learning Summary

Today I learned:

* What a **DBMS** is
* What an **RDBMS** is
* Why **MySQL is an RDBMS**
* Difference between SQL and NoSQL
* Structured vs unstructured data
* When relational databases are useful
* When flexible NoSQL data models can be useful
* Difference between `CHAR` and `VARCHAR`
* Why `STRING` is not a MySQL data type
* What `ENUM` is
* What `BOOLEAN` is
* How MySQL represents `BOOLEAN`
* What `TINYINT` is
* Why choosing the right data type matters

## The Big Picture

```text
                    DATABASES
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
        SQL                       NoSQL
          │                         │
       RDBMS                  Non-relational
          │                         │
     ┌────┴────┐              ┌─────┴─────┐
     ▼         ▼              ▼           ▼
   MySQL   PostgreSQL      MongoDB      Redis
     │
     ▼
Structured Data
     │
     ├── Tables
     ├── Rows
     ├── Columns
     ├── Primary Keys
     ├── Foreign Keys
     └── JOINs
```

**Day 2 complete — Divyanshu Shah 🚀**
