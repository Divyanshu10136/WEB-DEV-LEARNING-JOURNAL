
> **Topic:** Express.js, npm, Middleware, Project Structure & API Testing

---

## 📚 Topics Covered

* Express.js Basics
* Request (`req`) and Response (`res`)
* Routing
* Middleware
* npm Fundamentals
* PowerShell
* Backend Project Structure
* Models
* Controllers
* Postman
* API Request Flow

---

# 1. What is Express.js?

**Express.js** is a web framework built on top of **Node.js** that makes backend development easier and faster.

Node.js can create a web server by itself, but Express provides convenient tools for:

* Handling URLs
* Managing requests
* Sending responses
* Creating routes
* Using middleware
* Building APIs

### Without Express

Node.js requires more manual work to:

* Read the URL
* Check the HTTP method
* Parse incoming data
* Create responses

### With Express

Express handles many common backend tasks so developers can focus on application logic.

---

# 2. What is a Framework?

A **framework** is a collection of tools, conventions, and predefined functionality that helps developers build applications faster.

Instead of building everything from scratch, a framework provides a foundation.

### Popular Node.js Frameworks

* Express.js
* Fastify
* Koa
* NestJS

---

# 3. Basic Express Route

```js
app.get("/hello", (req, res) => {
    res.json({
        message: "Hello"
    });
});
```

### Explanation

| Part         | Meaning                                 |
| ------------ | --------------------------------------- |
| `app.get()`  | Creates a GET route                     |
| `"/hello"`   | URL/path                                |
| `req`        | Contains information sent by the client |
| `res`        | Used to send data back to the client    |
| `res.json()` | Sends a JSON response                   |

---

# 4. Understanding the Request (`req`)

The **request object** contains information sent by the client to the server.

Common properties include:

* `req.params`
* `req.query`
* `req.body`
* `req.method`
* `req.url`

---

## `req.params`

Used for dynamic values inside a URL.

Example:

```text
/users/5
```

Route:

```js
app.get("/users/:id", (req, res) => {
    console.log(req.params.id);
});
```

Result:

```text
5
```

So:

```js
req.params.id
```

returns:

```text
5
```

---

## `req.query`

Used for query parameters.

Example:

```text
/users?limit=10
```

Access it using:

```js
req.query.limit
```

Result:

```text
10
```

Example:

```js
app.get("/users", (req, res) => {
    console.log(req.query.limit);
});
```

---

## `req.body`

Contains data sent inside requests such as `POST` or `PUT`.

Example JSON:

```json
{
    "name": "John"
}
```

To access JSON request bodies, use:

```js
app.use(express.json());
```

Then:

```js
req.body
```

can be used inside your route/controller.

> ⚠️ `express.json()` should be registered **before the routes that need `req.body`**.

---

## `req.method`

Returns the HTTP method used by the client.

Examples:

```text
GET
POST
PUT
DELETE
```

Example:

```js
console.log(req.method);
```

---

## `req.url`

Returns the requested URL.

Example:

```text
/documents
```

Usage:

```js
console.log(req.url);
```

---

# 5. Understanding the Response (`res`)

The **response object** is used to send data from the server back to the client.

---

## `res.send()`

Sends text, HTML, or other supported response data.

```js
res.send("Hello World");
```

---

## `res.json()`

Sends JSON data.

```js
res.json({
    message: "Hello World"
});
```

---

## `res.status()`

Sets the HTTP status code.

```js
res.status(404).json({
    error: "Not Found"
});
```

A common pattern is:

```js
res.status(200).json({
    message: "Success"
});
```

---

# 6. Routing

**Routing** determines which code should execute when a client requests a particular URL and HTTP method.

Example:

```text
GET /hello
```

↓

```text
Route matches
```

↓

```text
Controller runs
```

↓

```text
Response is returned
```

---

## Routing Flow

```text
Client
   │
   ▼
 Route
   │
   ▼
Controller
   │
   ▼
Response
```

---

# 7. Middleware

**Middleware** is a function that runs during the request-response cycle, usually before the request reaches the controller.

Middleware can:

* Check authentication
* Validate data
* Log requests
* Modify request data
* Stop invalid requests
* Handle errors
* Parse request bodies

---

## Middleware Structure

```js
function myMiddleware(req, res, next) {

    // Work here

    next();
}
```

### What does `next()` do?

`next()` passes control to the next middleware or route handler.

```text
Middleware
    │
    ▼
 next()
    │
    ▼
Next Middleware / Controller
```

If `next()` is not called and the middleware doesn't send a response, the request can remain stuck.

---

# Middleware Flow

```text
Client
   │
   ▼
express.json()
   │
   ▼
Logger Middleware
   │
   ▼
Controller
   │
   ▼
Response
```

---

# Why Middleware Order Matters

### Correct

```js
app.use(express.json());

app.use(logger);

app.use(routes);
```

### Potentially Incorrect

```js
app.use(routes);

app.use(express.json());
```

If a route needs JSON body parsing, placing `express.json()` after the route means that middleware has not run yet.

```text
req.body
   ↓
Not parsed yet
```

### Rule

> Register middleware **before** the routes that depend on it.

---

# 8. Logger Middleware

A logger middleware prints information about incoming requests.

```js
function logger(req, res, next) {

    console.log(req.method);
    console.log(req.url);

    next();
}
```

Example output:

```text
GET
/api/documents
```

This is useful for:

* Debugging
* Monitoring requests
* Understanding application flow

---

# 9. npm

**npm** stands for **Node Package Manager**.

The term npm commonly refers to both:

1. The npm package registry/ecosystem
2. The npm command-line tool

npm is used to:

* Install packages
* Remove packages
* Manage dependencies
* Run project scripts
* Manage package versions

---

# `package.json`

Every typical Node.js project contains:

```text
package.json
```

It stores information such as:

* Project name
* Version
* Scripts
* Dependencies
* DevDependencies
* Project metadata

Create it using:

```bash
npm init -y
```

---

# Installing Packages

## Install Express

```bash
npm install express
```

## Install Nodemon as a Development Dependency

```bash
npm install --save-dev nodemon
```

## Install a Global Package

```bash
npm install -g nodemon
```

## Remove a Package

```bash
npm uninstall express
```

---

# Local vs Global Installation

## Local Installation

The package is installed inside the current project.

```bash
npm install express
```

It is recorded in the project's `package.json`.

---

## Global Installation

The package is installed globally and can generally be used across projects.

```bash
npm install -g nodemon
```

> 💡 For project dependencies, prefer **local installation**. Global installation is mainly useful for tools you want available system-wide.

---

# Dependencies vs DevDependencies

## Dependencies

Packages required by the application during normal operation.

Examples:

* Express
* Mongoose

Installed with:

```bash
npm install express
```

---

## DevDependencies

Packages primarily needed during development.

Examples:

* Nodemon
* Testing tools
* Development utilities

Installed with:

```bash
npm install --save-dev nodemon
```

---

# Semantic Versioning — SemVer

Package versions generally follow:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
4.22.2
```

Meaning:

```text
4  → Major
22 → Minor
2  → Patch
```

---

## Major Version

Usually contains breaking changes.

```text
4.x.x → 5.x.x
```

Existing code may require changes.

---

## Minor Version

Adds new features while maintaining backward compatibility under normal SemVer conventions.

```text
4.18.0 → 4.19.0
```

---

## Patch Version

Usually contains bug fixes.

```text
4.18.0 → 4.18.1
```

---

# Version Symbols

## Caret `^`

Example:

```text
^4.18.0
```

Generally allows compatible updates within major version `4`.

For example:

```text
4.22.2
```

may satisfy the range.

---

## Tilde `~`

Example:

```text
~4.18.0
```

Generally allows patch-level updates within the `4.18` minor version.

---

## Exact Version

```text
4.18.0
```

Specifies exactly that version.

---

# `package-lock.json`

npm creates:

```text
package-lock.json
```

It records the resolved versions of installed dependencies and their dependency tree.

### Benefits

* More reproducible installations
* Consistent dependency versions
* Reduced "works on my machine" problems
* Better team collaboration

> ✅ For applications, `package-lock.json` is normally committed to Git.

---

# Scoped Packages

Packages can belong to an organization or scope.

Examples:

```text
@types/node
@babel/core
```

The part before `/` represents the scope.

For example:

```text
@types/node
│     │
│     └── Package
└──────── Scope
```

---

# Package Visibility

## Public Package

Can generally be installed by anyone.

## Private Package

Restricted to authorized users or organizations.

---

# Alternatives to npm

Other JavaScript package managers include:

* Yarn
* pnpm
* Bun

---

# Useful npm Commands

| Command                  | Purpose                             |
| ------------------------ | ----------------------------------- |
| `npm init -y`            | Create `package.json`               |
| `npm install`            | Install project dependencies        |
| `npm install express`    | Install Express                     |
| `npm install -D nodemon` | Install Nodemon as a dev dependency |
| `npm install -g nodemon` | Install Nodemon globally            |
| `npm uninstall express`  | Remove Express                      |
| `npm outdated`           | Check outdated packages             |

---

# 10. Why PowerShell?

**PowerShell** is a command-line shell and scripting environment commonly available on Windows.

It can be used to:

* Navigate directories
* Create files and folders
* Run Node.js commands
* Run npm commands
* Execute scripts
* Manage development environments

Example:

```powershell
mkdir backend
cd backend
npm init -y
```

---

# 11. Standard Backend Project Structure

A simple Express project can be organized like this:

```text
project/
│
├── app.js
│
├── routes/
│   └── documentRoutes.js
│
├── controllers/
│   └── documentController.js
│
├── models/
│   └── documentModel.js
│
├── middleware/
│   └── logger.js
│
├── package.json
├── package-lock.json
└── node_modules/
```

> 💡 The exact structure can vary between projects. The goal is to keep different responsibilities separated.

---

# `app.js`

The main application entry point.

Typical responsibilities:

* Create the Express application
* Register middleware
* Register routes
* Start the server

Example:

```js
const express = require("express");

const app = express();

app.use(express.json());

app.get("/hello", (req, res) => {
    res.json({
        message: "Hello World"
    });
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

---

# `routes/`

Routes determine:

> **Which URL and HTTP method should call which handler?**

Example:

```text
GET /documents
```

↓

```text
documentController
```

Example:

```js
const express = require("express");
const router = express.Router();

router.get("/documents", documentController.getDocuments);

module.exports = router;
```

---

# `controllers/`

Controllers handle HTTP requests and responses.

Typical responsibilities:

* Receive `req`
* Call the appropriate model/service
* Process the result
* Send `res`

Example:

```js
const getDocuments = (req, res) => {
    res.json({
        message: "Documents retrieved"
    });
};

module.exports = {
    getDocuments
};
```

Controllers generally know about:

```text
req
res
```

They should avoid containing large amounts of database/data-access logic.

---

# `models/`

Models are responsible for the application's data layer.

Typical responsibilities:

* Read data
* Create data
* Update data
* Delete data
* Find records

A model should generally not:

* Use `req`
* Use `res`
* Send HTTP responses
* Decide HTTP status codes

---

# `middleware/`

Contains reusable middleware functions.

Examples:

* Logger
* Authentication
* Authorization
* Validation
* Error handling

Example:

```js
function logger(req, res, next) {
    console.log(`${req.method} ${req.url}`);
    next();
}

module.exports = logger;
```

---

# Backend Request Flow

A common architecture looks like:

```text
Client
   │
   ▼
Express
   │
   ▼
Middleware
   │
   ▼
Route
   │
   ▼
Controller
   │
   ▼
Model / Service
   │
   ▼
Database
   │
   ▼
Controller
   │
   ▼
Response
   │
   ▼
Client
```

### Simple Responsibility Breakdown

```text
Route       → Where should the request go?
Middleware  → What should happen before the handler?
Controller  → How should the HTTP request/response be handled?
Model       → How should data be accessed?
Database    → Where is the data stored?
```

---

# 12. First Hello Route

Suppose we want:

```http
GET /api/documents/hello
```

The project could follow:

```text
app.js
   ↓
routes/index.js
   ↓
documentRoutes.js
   ↓
documentController.js
   ↓
Response
```

Expected response:

```json
{
    "message": "Hello from documents"
}
```

---

# Example Implementation

### `app.js`

```js
const express = require("express");
const documentRoutes = require("./routes/documentRoutes");

const app = express();

app.use(express.json());

app.use("/api/documents", documentRoutes);

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

### `routes/documentRoutes.js`

```js
const express = require("express");
const {
    hello
} = require("../controllers/documentController");

const router = express.Router();

router.get("/hello", hello);

module.exports = router;
```

### `controllers/documentController.js`

```js
const hello = (req, res) => {
    res.json({
        message: "Hello from documents"
    });
};

module.exports = {
    hello
};
```

Now:

```http
GET http://localhost:3000/api/documents/hello
```

returns:

```json
{
    "message": "Hello from documents"
}
```

---

# 13. Models

Models are responsible for the **data layer**.

They should handle operations such as:

* Reading data
* Writing data
* Finding records
* Updating records
* Deleting records

### Models Should Not

```text
❌ Use req
❌ Use res
❌ Send HTTP responses
❌ Return HTTP status codes
```

Instead, they should return data or errors to the layer that called them.

---

# 14. Controllers

Controllers connect the **HTTP layer** with the **data/business layer**.

Typical flow:

```text
Request
   ↓
Controller
   ↓
Model / Service
   ↓
Data
   ↓
Controller
   ↓
Response
```

Controllers are responsible for:

* Receiving the request
* Calling the appropriate logic
* Checking the result
* Sending the response

---

# Model vs Controller

| Model                            | Controller                       |
| -------------------------------- | -------------------------------- |
| Handles data                     | Handles HTTP requests            |
| Reads/writes data                | Sends responses                  |
| Doesn't need `req` or `res`      | Works with `req` and `res`       |
| Doesn't decide HTTP status codes | Can return HTTP status codes     |
| Focuses on data operations       | Focuses on request/response flow |

---

# 15. Postman

**Postman** is a tool used to test APIs.

With Postman, you can:

* Send GET requests
* Send POST requests
* Send PUT requests
* Send DELETE requests
* Send JSON request bodies
* Add headers
* Test authentication
* Inspect responses
* Save API collections

---

# Browser vs Postman

A browser address bar is primarily convenient for making **GET** requests.

For APIs involving:

```text
POST
PUT
PATCH
DELETE
```

Postman or another API client is much more useful.

Example POST request:

```http
POST /api/documents
```

Request body:

```json
{
    "title": "JavaScript Notes"
}
```

---

# 🔄 Complete Backend Architecture

```text
                    ┌──────────────┐
                    │    Client    │
                    │   Browser /  │
                    │   Postman    │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   Express    │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │  Middleware  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Route     │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ Controller   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ Model/Service│
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   Database   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ Controller   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │  Response    │
                    └──────────────┘
```

---

# 🧠 Important Points to Remember

* Express makes Node.js backend development easier.
* `req` contains information sent by the client.
* `res` is used to send data back to the client.
* Routes determine where requests should go.
* Middleware runs during the request-response cycle.
* Middleware order matters.
* `express.json()` should be registered before routes that need JSON request bodies.
* `next()` passes control to the next middleware/handler.
* `package.json` stores project metadata and dependency information.
* `package-lock.json` records resolved dependency versions.
* `dependencies` are used by the application.
* `devDependencies` are primarily used during development.
* Controllers handle HTTP request/response logic.
* Models handle data operations.
* Keep controllers and models focused on their respective responsibilities.
* Postman is useful for testing APIs.

---

# ⚡ Quick Revision

```text
Client
   ↓
Express
   ↓
Middleware
   ↓
Route
   ↓
Controller
   ↓
Model / Service
   ↓
Database
   ↓
Controller
   ↓
Response
   ↓
Client
```

---

# 🎯 Day 1 Cheat Sheet

| Concept             | Remember                                  |
| ------------------- | ----------------------------------------- |
| Express.js          | Node.js web framework                     |
| `req`               | Incoming request information              |
| `res`               | Outgoing response                         |
| `req.params`        | Dynamic URL values                        |
| `req.query`         | Query-string values                       |
| `req.body`          | Request body data                         |
| `req.method`        | HTTP method                               |
| `req.url`           | Requested URL                             |
| `res.send()`        | Send response                             |
| `res.json()`        | Send JSON                                 |
| `res.status()`      | Set HTTP status                           |
| Middleware          | Runs during request/response flow         |
| `next()`            | Pass control forward                      |
| npm                 | Node.js package management ecosystem/tool |
| `package.json`      | Project/dependency configuration          |
| `package-lock.json` | Resolved dependency versions              |
| Routes              | Map requests to handlers                  |
| Controllers         | Handle HTTP logic                         |
| Models              | Handle data                               |
| Postman             | API testing tool                          |

---




