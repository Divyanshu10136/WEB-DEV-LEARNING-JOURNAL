
# Day 1 — Why Databases Exist & MySQL Basics

**Author:** Divyanshu Shah

## Table of Contents

* [1. Why Databases Exist](#1-why-databases-exist)
* [2. Why We Moved Away From `data.json`](#2-why-we-moved-away-from-datajson)
* [3. MySQL Server vs MySQL Workbench](#3-mysql-server-vs-mysql-workbench)
* [4. Database Hierarchy](#4-database-hierarchy)
* [5. Creating a Database](#5-creating-a-database)
* [6. Creating the `users` Table](#6-creating-the-users-table)
* [7. INSERT and SELECT](#7-insert-and-select)
* [8. Creating the `resumes` Table](#8-creating-the-resumes-table)
* [9. Primary Key vs Foreign Key](#9-primary-key-vs-foreign-key)
* [10. ON UPDATE and ON DELETE](#10-on-update-and-on-delete)
* [11. JOIN](#11-join)
* [12. Normalization](#12-normalization)
* [13. Indexing](#13-indexing)
* [14. Quick Recap](#14-quick-recap)
* [15. SQL Practiced Today](#15-sql-practiced-today)

---

## 1. Why Databases Exist

Before using a real database, an application can store its data inside a simple flat file such as:

```text
data.json
```

Every record is stored as plain text. Whenever something changes, the application has to:

1. Open the file.
2. Read the data.
3. Change the required record.
4. Save the entire file again.

This may work for a small project, but it creates serious problems as the application grows.

### Problem 1 — Race Conditions

Imagine two people open the same Word document at the same time.

Both make different changes:

```text
Person A → changes document → saves
Person B → changes document → saves
```

If Person B saves last, Person A's changes may be overwritten.

This is a **race condition**.

A plain file does not provide the sophisticated concurrency controls that a database provides.

### Analogy — Two People Editing the Same Word File

Think of a database more like a shared Google Doc.

Multiple users can work with the same underlying data, while the database manages concurrent operations so changes don't simply overwrite each other because one file was saved later.

---

### Problem 2 — Searching Becomes Slow

Suppose `data.json` contains thousands of records.

To find one user, the application may need to:

```text
Load the entire file
        ↓
Read every record
        ↓
Check each record
        ↓
Find the matching user
```

This becomes inefficient as the amount of data grows.

Databases are specifically designed to store and retrieve large amounts of data efficiently. One important feature that helps with this is **indexing**.

---

# 2. Why We Moved Away From `data.json`

Our resume project initially stored its data in:

```text
data.json
```

This worked for a classroom project, but it wasn't suitable for a real application.

The two major problems were:

### 1. Concurrent Updates

Multiple operations could try to modify the same file at the same time.

```text
Request A ──┐
            ├──> data.json
Request B ──┘
```

Without proper concurrency control, one update could overwrite another.

### 2. Searching Large Data

The application might need to load and scan the entire file to find a particular record.

A database provides structures such as **indexes** that can make common searches much faster.

Therefore, the resume project was moved from:

```text
data.json
```

to:

```text
MySQL Database
```

---

# 3. MySQL Server vs MySQL Workbench

Two important tools were installed:

### MySQL Server

**MySQL Server** is the actual database management system that stores and manages the data.

It runs in the background as a service.

### MySQL Workbench

**MySQL Workbench** is a graphical interface used to interact with MySQL.

It allows us to:

* Write SQL queries
* Create databases
* Create tables
* View data
* Manage database structures
* Inspect relationships

### Analogy

Think of a bank.

```text
MySQL Server
    ↓
Secure bank server room
where the actual data is stored
```

```text
MySQL Workbench
    ↓
ATM / interface
used to give instructions
```

The Workbench is the interface, while MySQL Server is the system actually storing and managing the data.

The **root password** protects administrative access to MySQL.

---

# 4. Database Hierarchy

A simple way to understand the structure is:

```text
Server
  │
  ├── Database
  │      │
  │      ├── Table
  │      │     ├── Row
  │      │     ├── Row
  │      │     └── Row
  │      │
  │      └── Table
  │
  └── Database
```

### Server

A MySQL server can contain multiple databases.

### Database

A database is a logical container for related tables.

Example:

```text
resume_db
```

### Table

A table stores related data in rows and columns.

Examples:

```text
users
resumes
```

### Row

A row represents one record.

For example:

```text
1 | Divyanshu | divyanshu@gmail.com
```

### Analogy

Think about folders on a computer:

```text
Server
└── Project Folder
    ├── users.xlsx
    └── resumes.xlsx
```

The database is like the project folder, while tables are like individual spreadsheets inside it.

---

# 5. Creating a Database

We created the database using:

```sql
CREATE DATABASE resume_db;
USE resume_db;
```

### `CREATE DATABASE`

Creates a new database.

```sql
CREATE DATABASE resume_db;
```

### `USE`

Selects the database we want to work with.

```sql
USE resume_db;
```

After running this, SQL commands will operate on `resume_db`.

---

# 6. Creating the `users` Table

We created a `users` table:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(45) NOT NULL,
    email VARCHAR(45) NOT NULL UNIQUE
);
```

Let's understand each column.

### `id`

```sql
id INT AUTO_INCREMENT PRIMARY KEY
```

* `INT` → stores an integer
* `AUTO_INCREMENT` → MySQL automatically generates the next number
* `PRIMARY KEY` → uniquely identifies each row

Example:

```text
1
2
3
4
5
```

### `name`

```sql
name VARCHAR(45) NOT NULL
```

* `VARCHAR(45)` → variable-length text up to 45 characters
* `NOT NULL` → the value cannot be missing

### `email`

```sql
email VARCHAR(45) NOT NULL UNIQUE
```

The email:

* Cannot be `NULL`
* Must be unique

Therefore, two users cannot have the same email address.

---

## Analogy — College Roll Numbers

The `id` is like a college roll number.

The college assigns:

```text
Student 1 → Roll No. 1
Student 2 → Roll No. 2
Student 3 → Roll No. 3
```

Students don't choose their own roll numbers.

Similarly:

```sql
AUTO_INCREMENT
```

automatically generates IDs.

The `UNIQUE` email constraint ensures that the same email cannot be registered twice.

---

# 7. INSERT and SELECT

We inserted users into the table.

```sql
INSERT INTO users (name, email)
VALUES ('unishka', 'unishka@gmail.com');
```

We can read all users using:

```sql
SELECT * FROM users;
```

We can search for a specific user:

```sql
SELECT *
FROM users
WHERE name = 'unishka';
```

### SQL Concepts

| SQL      | Meaning     |
| -------- | ----------- |
| `INSERT` | Add data    |
| `SELECT` | Read data   |
| `WHERE`  | Filter data |

### Analogy — Phone Contacts

```text
INSERT → Save a new contact

SELECT → Open your contacts

WHERE → Search for a specific contact
```

---

## Users Table

The final table contained:

| id | name    | email                                         |
| -: | ------- | --------------------------------------------- |
|  1 | unishka | [unishka@gmail.com](mailto:unishka@gmail.com) |
|  2 | divyani | [divyani@gmail.com](mailto:divyani@gmail.com) |
|  3 | vijay   | [vijay@gmail.com](mailto:vijay@gmail.com)     |
|  4 | manish  | [manish@gmail.com](mailto:manish@gmail.com)   |
|  5 | ishita  | [ishita@gmail.com](mailto:ishita@gmail.com)   |

---

# 8. Creating the `resumes` Table

We then created a second table:

```sql
CREATE TABLE resumes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    userId INT NOT NULL,
    tittle VARCHAR(150) NOT NULL,
    FOREIGN KEY (userId) REFERENCES users(id)
);
```

> **Note:** `tittle` was used in the original project schema. In a new project, `title` would normally be the preferred spelling.

The table contains:

```text
id
userId
tittle
```

### `id`

The resume's own unique identifier.

### `userId`

Identifies the user who owns the resume.

### `tittle`

Stores the resume title.

---

# 9. Primary Key vs Foreign Key

This is one of the most important database concepts.

## Primary Key

A **Primary Key** uniquely identifies a row in its own table.

Example:

```sql
users.id
```

The IDs are:

```text
1
2
3
4
5
```

Each user has a unique ID.

### Simple Definition

> **Primary Key = Who am I?**

---

## Foreign Key

A **Foreign Key** creates a relationship with another table.

In our example:

```sql
resumes.userId
```

references:

```sql
users.id
```

```sql
FOREIGN KEY (userId)
REFERENCES users(id)
```

### Simple Definition

> **Foreign Key = Who do I belong to?**

---

## Analogy — Swiggy Order

Imagine a food delivery application.

An order might have:

```text
Order ID = 501
Customer ID = 10
```

The order's own ID identifies the order.

```text
Order ID → Primary Key
```

The customer ID identifies its owner.

```text
Customer ID → Foreign Key
```

Our resume system works in the same way:

```text
resume.id
    ↓
"What resume am I?"

resume.userId
    ↓
"Which user owns me?"
```

---

## Relationship

Our tables are connected like this:

```text
users
  │
  │ id
  ▼
resumes
  │
  └── userId
```

One user can have multiple resumes.

```text
User 1
 ├── Resume 1
 └── Resume 2

User 2
 ├── Resume 3
 └── Resume 4
```

This is a **one-to-many relationship**.

---

# 10. ON UPDATE and ON DELETE

Foreign keys can define what should happen when a referenced row is updated or deleted.

Two important options are:

```text
RESTRICT
CASCADE
```

## RESTRICT

Prevents the parent row from being deleted or changed when related child rows exist.

For example:

```text
User 1
 ├── Resume 1
 └── Resume 2
```

Trying to delete User 1 may fail because resumes still reference that user.

The resumes must be handled first.

### Analogy

It's like a system refusing to delete a customer account while dependent records still exist.

---

## CASCADE

With `CASCADE`, changes to the parent can automatically affect related rows.

For example:

```sql
FOREIGN KEY (userId)
REFERENCES users(id)
ON DELETE CASCADE
```

If User 1 is deleted:

```text
User 1
 ├── Resume 1
 └── Resume 2
```

the related resumes are automatically deleted.

### Analogy

Deleting a parent account automatically cleans up its dependent records.

---

# 11. JOIN

A **JOIN** allows us to retrieve related information from multiple tables in a single query.

Our tables contain:

```text
users
 ├── id
 ├── name
 └── email

resumes
 ├── id
 ├── userId
 └── tittle
```

The connection is:

```text
resumes.userId = users.id
```

We can join them using:

```sql
SELECT
    users.name,
    users.email,
    resumes.tittle
FROM resumes
JOIN users
    ON resumes.userId = users.id;
```

---

## How the JOIN Works

Suppose we have:

### `users`

| id | name    | email                                         |
| -: | ------- | --------------------------------------------- |
|  1 | unishka | [unishka@gmail.com](mailto:unishka@gmail.com) |
|  2 | divyani | [divyani@gmail.com](mailto:divyani@gmail.com) |
|  3 | vijay   | [vijay@gmail.com](mailto:vijay@gmail.com)     |
|  4 | manish  | [manish@gmail.com](mailto:manish@gmail.com)   |

### `resumes`

| id | userId | tittle                |
| -: | -----: | --------------------- |
|  1 |      1 | software engineer     |
|  2 |      1 | sales manager         |
|  3 |      2 | frontend developer    |
|  4 |      2 | software developer    |
|  5 |      3 | hr                    |
|  6 |      4 | application developer |
|  7 |      4 | full stack developer  |

The JOIN produces:

| name    | email                                         | tittle                |
| ------- | --------------------------------------------- | --------------------- |
| unishka | [unishka@gmail.com](mailto:unishka@gmail.com) | software engineer     |
| unishka | [unishka@gmail.com](mailto:unishka@gmail.com) | sales manager         |
| divyani | [divyani@gmail.com](mailto:divyani@gmail.com) | frontend developer    |
| divyani | [divyani@gmail.com](mailto:divyani@gmail.com) | software developer    |
| vijay   | [vijay@gmail.com](mailto:vijay@gmail.com)     | hr                    |
| manish  | [manish@gmail.com](mailto:manish@gmail.com)   | application developer |
| manish  | [manish@gmail.com](mailto:manish@gmail.com)   | full stack developer  |

---

## Analogy — Matching a Parcel to Its Owner

Imagine one table contains:

```text
Parcel ID
Item
```

and another contains:

```text
Parcel ID
Owner
Address
```

A JOIN matches the common ID:

```text
Parcel ID
   ↓
Match
   ↓
Owner + Parcel Information
```

The original data remains in separate tables, but JOIN allows us to view related information together.

---

## `ORDER BY`

Without an `ORDER BY` clause, SQL does not guarantee a particular result order.

If we want a specific order:

```sql
SELECT *
FROM users
ORDER BY id;
```

Descending order:

```sql
SELECT *
FROM users
ORDER BY id DESC;
```

---

# 12. Normalization

**Normalization** means organizing data so that the same fact is not unnecessarily stored in multiple places.

Imagine storing this in every resume:

```text
resume_id
user_name
user_email
resume_title
```

If a user changes their email, we would have to update every resume belonging to that user.

That creates the possibility of inconsistent data.

---

## Better Design

Store user information once:

```text
users
 ├── id
 ├── name
 └── email
```

Then the resume only stores:

```text
resumes
 ├── id
 ├── userId
 └── tittle
```

The relationship connects them.

### One-Line Definition

> **Normalization = Store each fact once and use IDs to connect related data.**

---

## Analogy — Aadhaar Number

Instead of repeatedly copying a person's complete information onto every form, a form can store a unique identifier and refer back to the original record.

Similarly:

```text
users
   ↓
userId
   ↓
resumes
```

---

# 13. Indexing

An **index** is a data structure that helps MySQL find rows efficiently.

For example:

```sql
CREATE INDEX idx_email
ON users(email);
```

Now MySQL has an index associated with the `email` column.

---

## Analogy — Textbook Index

Imagine a 1,000-page textbook.

You want to find:

```text
"Normalization"
```

Without an index, you might have to search page by page.

With the textbook's index:

```text
Normalization → Page 214
```

You can jump directly toward the relevant location.

A database index serves a similar purpose.

---

## Where Indexes Can Help

Indexes are commonly useful for queries involving:

```text
WHERE
JOIN
ORDER BY
GROUP BY
```

However, indexes aren't free. They consume storage and can make writes somewhat more expensive because the index also needs to be maintained.

---

# 14. Quick Recap

| Term              | Meaning                                                         |
| ----------------- | --------------------------------------------------------------- |
| **Database**      | Organized collection of related data                            |
| **Table**         | Collection of rows and columns                                  |
| **Row**           | One record                                                      |
| **Primary Key**   | Unique identity of a row                                        |
| **Foreign Key**   | Reference to a row in another table                             |
| **JOIN**          | Combines related data from multiple tables                      |
| **Normalization** | Avoids unnecessary duplication of facts                         |
| **Index**         | Data structure that can speed up data retrieval                 |
| **RESTRICT**      | Prevents certain parent changes when dependent rows exist       |
| **CASCADE**       | Automatically propagates certain parent changes to related rows |

---

# 15. SQL Practiced Today

```sql
CREATE DATABASE resume_db;

USE resume_db;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(45) NOT NULL,
    email VARCHAR(45) NOT NULL UNIQUE
);

INSERT INTO users (name, email)
VALUES ('unishka', 'unishka@gmail.com');

SELECT *
FROM users;

SELECT *
FROM users
WHERE name = 'unishka';

CREATE TABLE resumes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    userId INT NOT NULL,
    tittle VARCHAR(150) NOT NULL,
    FOREIGN KEY (userId) REFERENCES users(id)
);

SELECT
    users.name,
    users.email,
    resumes.tittle
FROM resumes
JOIN users
    ON resumes.userId = users.id;
```

---

# 🎯 Day 1 Learning Summary

Today I learned:

* Why applications need databases instead of simple files
* What a **race condition** is
* Why databases are better for large-scale data retrieval
* Difference between **MySQL Server** and **MySQL Workbench**
* Server → Database → Table → Row hierarchy
* How to create a database using SQL
* How to create tables
* `PRIMARY KEY`
* `FOREIGN KEY`
* `AUTO_INCREMENT`
* `NOT NULL`
* `UNIQUE`
* `INSERT`
* `SELECT`
* `WHERE`
* `ORDER BY`
* `JOIN`
* `RESTRICT`
* `CASCADE`
* Normalization
* Indexing
* One-to-many relationships

### The Core Idea

```text
                    MySQL Server
                         │
                    resume_db
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
            users                 resumes
              │                     │
              │ id ◄──────── userId │
              │                     │
              └───────── JOIN ──────┘
                         │
                         ▼
                  Combined Result
```

**Day 1 complete — Divyanshu Shah 🚀**
