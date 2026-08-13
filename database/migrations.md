# Day 4 — Building the Full Database Schema With Sequelize

---

> **Migration**

A migration is a set of instructions that tells Sequelize **how to create, change, or remove the actual database structure**.

So we have two different things:

```text
Model
  ↓
Used by application code
  ↓
Read / create / update / delete data
```

and:

```text
Migration
  ↓
Changes database structure
  ↓
Creates / modifies / removes tables
```

### Simple analogy — Building and Using a Shelf

Imagine you want to build a shelf.

**Migration** = instructions for building the shelf.

```text
"Create a shelf with 4 sections."
```

You follow those instructions once, and the physical shelf exists.

**Model** = the way your application works with that shelf afterward.

```text
"Put this item on the shelf."
"Find this item."
"Remove this item."
```

So:

> **Migration builds the structure. Model interacts with the structure.**

---

# 4.2 Generating a Model and Migration

Instead of manually creating both files, Sequelize CLI can generate them together.

Example:

```bash
npx sequelize-cli model:generate \
  --name User \
  --attributes name:string,email:string
```

This command creates two files.

### Model

Usually created inside:

```text
models/
```

For example:

```text
models/user.js
```

### Migration

Usually created inside:

```text
migrations/
```

For example:

```text
migrations/20260813000000-create-user.js
```

The exact timestamp in the migration filename will be different.

---

# 4.3 What Does `model:generate` Actually Do?

Consider:

```bash
npx sequelize-cli model:generate \
  --name User \
  --attributes name:string,email:string
```

The command is basically saying:

```text
Create a model called User
        +
Create a migration for User
        +
Give it name and email columns
```

The generated files are connected conceptually.

```text
              model:generate
                    │
             ┌──────┴──────┐
             ▼             ▼
          Model         Migration
             │             │
             ▼             ▼
       Application      Database
          code          structure
```

---

# 4.4 Model vs Migration — Quick Comparison

| Model                              | Migration                       |
| ---------------------------------- | ------------------------------- |
| Used by application                | Used to modify database         |
| Represents a table                 | Creates/changes a table         |
| Used every day by application code | Usually run when schema changes |
| Handles queries and relationships  | Handles database structure      |
| Lives in `models/`                 | Lives in `migrations/`          |

### One-line difference

> **Model = how the application talks to the table.**

> **Migration = how the table gets created or changed.**

---

# 4.5 The Nine Tables in Our Project

Our resume application contains **nine main tables**.

```text
users
templates
documents
sections
items
versions
applications
shares
exports
```

Each table has a specific responsibility.

---

## 1. `users`

Stores information about user accounts.

Examples:

```text
id
name
email
password
tier
aiCredits
```

The `users` table is at the top of our relationship structure.

```text
users
  │
  ├── documents
  ├── applications
  └── exports
```

---

## 2. `templates`

Stores resume designs/templates.

For example:

```text
id
name
description
design
```

A template can be used by multiple documents.

```text
templates
    │
    ▼
documents
```

---

## 3. `documents`

This is the **central table** of our resume system.

A document represents something like:

* Resume
* Cover letter

It connects to:

```text
users
templates
```

Conceptually:

```text
users ──────────┐
                ▼
            documents
                ▲
                │
templates ──────┘
```

---

## 4. `sections`

A document contains multiple sections.

Examples:

```text
Experience
Education
Skills
Projects
Summary
```

Relationship:

```text
document
   │
   ├── section
   ├── section
   └── section
```

So:

> One document can have many sections.

---

## 5. `items`

An item represents one individual piece of information inside a section.

For example:

```text
Experience
    │
    ├── Software Engineer at Google
    ├── Developer at Microsoft
    └── Intern at XYZ
```

So:

```text
document
   ↓
sections
   ↓
items
```

Each level becomes more specific.

---

## 6. `versions`

Stores saved versions/snapshots of a document.

Imagine editing a resume:

```text
Resume
 │
 ├── Version 1
 ├── Version 2
 └── Version 3
```

This allows the application to keep previous versions instead of losing them when the document changes.

Relationship:

```text
documents
    │
    ├── version
    ├── version
    └── version
```

---

## 7. `applications`

Stores job applications tracked by the user.

For example:

```text
Google
Software Engineer
Applied
2026-08-10
```

An application can be connected to:

```text
users
documents
```

This allows the system to know:

> Which user applied for the job, and which resume/document was used?

---

## 8. `shares`

Stores public sharing information for a document.

For example:

```text
documentId
shareToken
isActive
```

Conceptually:

```text
document
   │
   └── share link
```

A user can create a public link to their resume.

---

## 9. `exports`

Stores information about generated files.

For example:

```text
PDF
DOCX
```

The table can track:

```text
which document was exported
which user requested it
what format was generated
```

Relationship:

```text
users
   │
   └──────┐
          ▼
       exports
          ▲
          │
      documents
```

---

# 4.6 Complete Database Structure

The overall system can be visualized like this:

```text
                         users
                           │
            ┌──────────────┼──────────────┐
            │              │              │
            ▼              ▼              ▼
       documents     applications      exports
            │
            │
       ┌────┴────┐
       │         │
       ▼         ▼
   sections   versions
       │
       ▼
     items

templates
    │
    ▼
documents

documents
    │
    ▼
  shares
```

The most important table is:

```text
documents
```

because many other parts of the application connect to it.

---

# 4.7 Why `documents` Is the Center

Think about the application from the user's perspective.

A user creates a resume.

```text
User
 ↓
Resume/Document
 ↓
Template
 ↓
Sections
 ↓
Items
```

The same document can also have:

```text
Document
 ├── Versions
 ├── Shares
 └── Exports
```

And job applications can reference the document:

```text
User
  │
  ▼
Application
  │
  ▼
Document
```

That's why `documents` acts as the **central table**.

---

# 4.8 Foreign Keys in Migrations

On Day 1, we learned about Foreign Keys.

For example:

```text
users
──────
id
```

and:

```text
documents
─────────
userId
```

`documents.userId` points to `users.id`.

In a Sequelize migration, we can define that relationship like this:

```javascript
userId: {
  type: Sequelize.INTEGER,
  references: {
    model: 'Users',
    key: 'id'
  },
  onUpdate: 'CASCADE',
  onDelete: 'CASCADE'
}
```

Let's understand each part.

---

## `type`

```javascript
type: Sequelize.INTEGER
```

The foreign key is an integer because it stores the ID of the user.

---

## `references`

```javascript
references: {
  model: 'Users',
  key: 'id'
}
```

This means:

> `userId` references the `id` column of the `Users` table.

In SQL terms:

```text
documents.userId
        ↓
users.id
```

---

# 4.9 `onUpdate: 'CASCADE'`

```javascript
onUpdate: 'CASCADE'
```

This tells the database:

> If the referenced parent ID changes, update the related foreign key values too.

For example:

```text
users.id = 5
     ↓
documents.userId = 5
```

If the parent ID changes:

```text
users.id = 10
```

the related foreign key can automatically change accordingly.

In real applications, primary keys usually don't change, but the rule can still be defined.

---

# 4.10 `onDelete: 'CASCADE'`

```javascript
onDelete: 'CASCADE'
```

This means:

> If the parent record is deleted, automatically delete related records.

Example:

```text
User
 ↓
Document
 ↓
Section
 ↓
Item
```

If the user is deleted and the relationships are configured with cascading deletes, related records can also be removed.

### Analogy — Cleaning an Entire Folder

Imagine deleting a project folder.

Instead of manually deleting:

```text
project
 ├── file 1
 ├── file 2
 ├── file 3
 └── subfolder
```

the system automatically removes everything inside it.

That's the basic idea of `CASCADE`.

---

# 4.11 Model Associations

The migration defines the **database-level relationship**.

The model defines the relationship from the **application's perspective**.

For example:

```javascript
Document.belongsTo(models.User, {
  foreignKey: 'userId'
});
```

This means:

> A document belongs to a user.

And:

```javascript
Document.hasMany(models.Section, {
  foreignKey: 'documentId',
  onDelete: 'CASCADE'
});
```

This means:

> A document has many sections.

So we have:

```text
User
 │
 │ has many
 ▼
Documents
 │
 │ has many
 ▼
Sections
 │
 │ has many
 ▼
Items
```

---

# 4.12 Migration vs Association

It's important not to confuse these.

### Migration

Deals with the **actual database structure**.

```javascript
references: {
  model: 'Users',
  key: 'id'
}
```

### Model association

Deals with how Sequelize understands the relationship.

```javascript
Document.belongsTo(models.User);
```

So:

```text
Migration
   ↓
Database relationship

Model association
   ↓
Application relationship
```

Both are important.

---

# 4.13 Running Migrations

Once migrations are created, we need to execute them.

### Run all pending migrations

```bash
npx sequelize-cli db:migrate
```

This tells Sequelize:

> Run all migrations that haven't been executed yet.

After running them, the corresponding tables are created.

---

# 4.14 Undo the Last Migration

```bash
npx sequelize-cli db:migrate:undo
```

This reverses the most recently executed migration.

For example:

```text
Migration 1 → User
Migration 2 → Template
Migration 3 → Document
```

If we run:

```bash
npx sequelize-cli db:migrate:undo
```

the last migration is reversed:

```text
Migration 3 → undone
```

---

# 4.15 Undo All Migrations

```bash
npx sequelize-cli db:migrate:undo:all
```

This reverses all migrations.

Conceptually:

```text
Migration 3 → undo
Migration 2 → undo
Migration 1 → undo
```

The database returns to its earlier state.

---

# 4.16 Check Migration Status

```bash
npx sequelize-cli db:migrate:status
```

This shows which migrations have already been executed.

Conceptually, you may see something like:

```text
up     202608130001-create-user.js
up     202608130002-create-template.js
up     202608130003-create-document.js
down   202608130004-create-section.js
```

### Meaning

```text
up   = migration has been executed
down = migration has not been executed
```

---

# 4.17 Why Migration Order Matters

Our tables have dependencies.

For example:

```text
documents.userId
        ↓
users.id
```

Therefore, `users` must exist before `documents` can reference it.

Similarly:

```text
documents
    ↓
sections
    ↓
items
```

The parent tables need to exist before dependent tables are created.

So migrations should run in the correct order.

### Example

Correct:

```text
1. users
2. templates
3. documents
4. sections
5. items
6. versions
7. applications
8. shares
9. exports
```

If we tried to create `documents` before `users`, the foreign key reference could fail because the referenced table does not exist yet.

---

# 4.18 Migration History

One major advantage of migrations is that database changes become trackable.

Imagine the project starts with:

```text
users
```

Later we need:

```text
templates
```

Then:

```text
documents
```

Instead of manually changing everyone's database, we create migrations:

```text
Migration 1
→ Create users

Migration 2
→ Create templates

Migration 3
→ Create documents
```

Every developer can run:

```bash
npx sequelize-cli db:migrate
```

and bring their local database up to the current schema.

---

# 4.19 Model + Migration + Database

The complete relationship is:

```text
                Sequelize CLI
                     │
                     ▼
             model:generate
                     │
              ┌──────┴──────┐
              ▼             ▼
           Model         Migration
              │             │
              │             ▼
              │         MySQL Table
              │             ▲
              │             │
              └─────────────┘
```

The model and migration have different jobs.

```text
MODEL
 ↓
Application interacts with data

MIGRATION
 ↓
Database structure is created/changed
```

---

# 4.20 Example: User Table

A command such as:

```bash
npx sequelize-cli model:generate \
  --name User \
  --attributes name:string,email:string
```

creates the starting point for a User model and its migration.

The migration can ultimately create a table conceptually like:

```sql
CREATE TABLE Users (
  id INTEGER AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255)
);
```

The model allows application code to work with that table:

```javascript
const user = await User.create({
  name: 'Divyanshu Shah',
  email: 'divyanshu@example.com'
});
```

So:

```text
JavaScript
   ↓
User.create()
   ↓
Sequelize
   ↓
SQL
   ↓
Users table
```

---

# 4.21 Complete Day 4 Flow

```text
                    Sequelize CLI
                         │
                         ▼
                  model:generate
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
           MODEL                 MIGRATION
              │                     │
              │                     ▼
              │                  MySQL
              │                     │
              │               Creates tables
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                  Application
                         │
                         ▼
              CRUD + Associations
```

---

# Day 4 — Quick Recap

| Term                      | One-line meaning                                      |
| ------------------------- | ----------------------------------------------------- |
| **Model**                 | JavaScript representation of a database table         |
| **Migration**             | Instructions for creating/changing database structure |
| **`model:generate`**      | Generates a model and migration together              |
| **Foreign Key**           | Connects one table to another                         |
| **`references`**          | Specifies which table/column a foreign key points to  |
| **`CASCADE`**             | Automatically propagates certain parent changes       |
| **`db:migrate`**          | Runs pending migrations                               |
| **`db:migrate:undo`**     | Reverts the latest migration                          |
| **`db:migrate:undo:all`** | Reverts all migrations                                |
| **`db:migrate:status`**   | Shows migration status                                |
| **Association**           | Defines relationships between Sequelize models        |
| **`hasMany()`**           | One record has multiple related records               |
| **`belongsTo()`**         | A record belongs to another record                    |

---

# Day 4 — Final Takeaway

The biggest concept from today is the difference between **building the database** and **using the database**.

```text
MIGRATION
   ↓
Builds / changes database structure
   ↓
MySQL tables


MODEL
   ↓
Application interacts with tables
   ↓
CRUD operations


ASSOCIATIONS
   ↓
Tell Sequelize how models are related
   ↓
User → Documents → Sections → Items
```

### The nine-table structure

```text
                         USERS
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
        DOCUMENTS     APPLICATIONS    EXPORTS
             │
       ┌─────┼─────┐
       ▼     ▼     ▼
  SECTIONS VERSIONS SHARES
       │
       ▼
     ITEMS

TEMPLATES
    │
    └──────────────► DOCUMENTS
```

