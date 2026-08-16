# 🚀 Full Stack Development Notes

These are my personal **Full Stack Development revision notes**. I’ve organized the concepts in simple language so I can quickly revisit them while building backend and full-stack projects.

---

# 🏗️ MVC Architecture

**MVC (Model-View-Controller)** is an architecture pattern that helps organize backend code by separating different responsibilities.

## 🛣️ Route

The route receives an incoming request and determines which controller should handle it.

Think of a route as the **entry point or gate** of the application.

### Responsibilities

* Receives the request.
* Matches the URL and HTTP method.
* Sends the request to the appropriate controller.

---

## 🎮 Controller

The controller contains the main request-handling logic.

### Responsibilities

* Receives data from the request.
* Processes the request.
* Communicates with the model.
* Sends the response back to the client.

---

## 🗃️ Model

The model is responsible for working with application data.

### Responsibilities

* Reads data.
* Creates data.
* Updates data.
* Deletes data.
* Communicates with the database or other data sources.

The model should focus on **data operations**, rather than sending HTTP responses.

---

## 🔄 MVC Request Flow

```text
Client
   ↓
Route
   ↓
Controller
   ↓
Model
   ↓
Database / JSON
   ↓
Controller
   ↓
Response
```

### 🧠 Easy Way to Remember

```text
Route      → Where should the request go?
Controller → What should happen?
Model      → How do we work with the data?
```

---

# 🌐 Frontend, Backend & API

## 🖥️ Frontend

The frontend is the part of an application that users can see and interact with.

Examples:

* Buttons
* Forms
* Images
* Navigation
* Text
* Input fields

---

## ⚙️ Backend

The backend works behind the scenes and handles the application's server-side operations.

It can:

* Receive requests.
* Validate input.
* Process business logic.
* Work with databases.
* Authenticate users.
* Send responses.

---

## 🔌 API

An **API (Application Programming Interface)** provides a way for different parts of an application to communicate.

For example:

```text
Frontend
    ↓
   API
    ↓
Backend
    ↓
Database
```

The frontend sends a request through an API, and the backend processes it and returns a response.

---

# 📩 Request & Response

## Request

A request is the information sent from the client to the server.

Example:

```http
GET /users
```

A request can contain:

* HTTP method
* URL
* Headers
* Query parameters
* Route parameters
* Request body

---

## Response

A response is the information returned by the server.

Example:

```json
{
  "name": "Divyanshu",
  "age": 19
}
```

The response can also contain an appropriate HTTP status code.

---

# 📊 HTTP Status Codes

Status codes tell the client what happened when the server processed a request.

| Code  | Meaning                              |
| ----- | ------------------------------------ |
| `200` | Request completed successfully       |
| `201` | Resource created successfully        |
| `400` | Invalid or malformed request         |
| `401` | Authentication is required or failed |
| `404` | Requested resource was not found     |
| `500` | Unexpected server-side error         |

### 💡 Important

Using meaningful status codes makes APIs easier for frontend developers and other clients to understand.

---

# 📦 JSON

**JSON (JavaScript Object Notation)** is a lightweight data format commonly used for exchanging information between frontend applications and backend APIs.

Example:

```json
{
  "name": "Divyanshu",
  "age": 19
}
```

JSON is commonly used in API request and response bodies.

---

# 🧪 Why Use Postman?

Postman is useful for testing APIs without having to build a frontend first.

It can be used to test HTTP methods such as:

* `GET`
* `POST`
* `PUT`
* `PATCH`
* `DELETE`

For example, I can test a user API directly from Postman and check:

* Request body
* Headers
* Status code
* Response data
* Error messages

---

# 🔄 Typical Backend Flow

A common backend request can follow this pattern:

```text
Browser / Client
       ↓
     Route
       ↓
   Controller
       ↓
      Model
       ↓
    Database
       ↓
      Model
       ↓
   Controller
       ↓
    Response
       ↓
     Client
```

Keeping these layers separate makes the application easier to maintain.

---

# 🟨 JavaScript Fundamentals

## 📦 Objects

Objects store related information as **key-value pairs**.

Example:

```javascript
const student = {
  name: "Divyanshu",
  marks: 90
};
```

Accessing a property:

```javascript
student.name;
```

Output:

```text
Divyanshu
```

---

# 📚 Arrays

Arrays are used to store multiple values in a single variable.

Example:

```javascript
const students = [
  "Info",
  "Divyanshu",
  "Alpha"
];
```

Arrays are especially useful when working with collections of data returned from APIs or databases.

---

# 🛠️ Useful JavaScript Array Methods

## `forEach()`

Runs a function for each item in an array.

```javascript
students.forEach(student => {
  console.log(student);
});
```

It is mainly used when you want to perform an action for every item.

---

## `map()`

Creates a **new array** by transforming every element.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);
```

Result:

```javascript
[2, 4, 6]
```

---

## `filter()`

Creates a new array containing only the elements that satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4];

const evenNumbers = numbers.filter(number => number % 2 === 0);
```

Result:

```javascript
[2, 4]
```

---

## `find()`

Returns the **first element** that matches a condition.

```javascript
const users = [
  { id: 1, name: "Divyanshu" },
  { id: 2, name: "Alex" }
];

const user = users.find(user => user.id === 2);
```

---

## `some()`

Returns `true` if **at least one** element satisfies the condition.

```javascript
const numbers = [1, 3, 5, 6];

numbers.some(number => number % 2 === 0);
```

Result:

```text
true
```

---

## `every()`

Returns `true` only when **all elements** satisfy the condition.

```javascript
const numbers = [2, 4, 6];

numbers.every(number => number % 2 === 0);
```

Result:

```text
true
```

---

## `sort()`

Sorts the elements of an array.

For numbers, provide a comparison function:

```javascript
const numbers = [10, 2, 5];

numbers.sort((a, b) => a - b);
```

Result:

```javascript
[2, 5, 10]
```

---

## `reduce()`

Reduces an array into a single accumulated value.

For example, calculating a total:

```javascript
const marks = [80, 90, 70];

const total = marks.reduce(
  (sum, mark) => sum + mark,
  0
);
```

Result:

```text
240
```

---

# 🚨 Error Handling

Errors can occur during almost any backend operation.

Common examples include:

* Invalid user input.
* Missing files.
* Database failures.
* Invalid API requests.
* Unexpected server errors.

JavaScript provides `try...catch` for handling exceptions.

```javascript
try {
  // Code that may throw an error
} catch (error) {
  // Handle the error
}
```

### `try`

Contains the code that might produce an error.

### `catch`

Runs when an exception occurs and gives us an opportunity to handle it.

### 💡 Backend Rule

Do not allow an unexpected error to produce a confusing response.

Instead, handle errors properly and return an appropriate HTTP status and message.

---

# 📁 Backend Project Structure

A simple backend project can be organized like this:

```text
project/
│
├── routes/
├── controllers/
├── models/
├── middleware/
├── app.js
└── package.json
```

---

## 📍 `routes/`

Contains API route definitions.

Example:

```text
GET    /users
POST   /users
GET    /users/:id
```

Routes decide which controller should handle each request.

---

## 🎮 `controllers/`

Contains request-handling and business logic.

Controllers typically:

* Read request data.
* Call models or services.
* Handle application logic.
* Return responses.

---

## 🗃️ `models/`

Responsible for working with application data.

Models can communicate with:

* Databases
* JSON files
* Other data sources

---

## 🛡️ `middleware/`

Middleware functions run during the request-response cycle.

Common examples include:

* Authentication
* Authorization
* Logging
* Validation
* Error handling

---

# 🧩 Express Middleware

In Express, middleware can be registered using `app.use()`.

A common example is:

```javascript
app.use(express.json());
```

This allows Express to parse incoming JSON request bodies.

For example, if the client sends:

```json
{
  "name": "Divyanshu"
}
```

the parsed data can be accessed through:

```javascript
req.body
```

### ⚠️ Middleware Order Matters

Middleware that needs to process request data should generally be registered **before the routes that depend on it**.

Example:

```javascript
const express = require("express");

const app = express();

app.use(express.json());

app.use("/api/users", userRoutes);
```

---

# 📌 Important Backend Terms

| Term       | Meaning                                          |
| ---------- | ------------------------------------------------ |
| `req`      | Contains information about the incoming request  |
| `res`      | Used to send a response to the client            |
| Route      | Determines which endpoint handles a request      |
| Controller | Handles request processing and application logic |
| Model      | Works with application data                      |
| Middleware | Runs during the request-response cycle           |
| API        | Interface through which applications communicate |
| JSON       | Common format for exchanging structured data     |

---

# 🌿 Git vs GitHub

## Git

**Git** is a distributed version control system.

It runs on your computer and tracks changes made to your project.

Git allows you to:

* Save versions of your code.
* Compare changes.
* Create branches.
* Revert changes.
* Collaborate with others.

---

## GitHub

**GitHub** is an online platform for hosting and collaborating on Git repositories.

It provides features such as:

* Remote repositories
* Pull requests
* Issues
* Code reviews
* Collaboration

### 🧠 Easy Difference

```text
Git     → Version control tool
GitHub  → Online platform for Git repositories
```

---

# 🔄 Basic Git Workflow

## 1️⃣ Initialize a Repository

```bash
git init
```

Creates a Git repository in the current project.

---

## 2️⃣ Check Repository Status

```bash
git status
```

Shows modified, staged, and untracked files.

---

## 3️⃣ Stage Files

```bash
git add .
```

Stages the current changes.

---

## 4️⃣ Create a Commit

```bash
git commit -m "Added Login Page"
```

Creates a snapshot of the staged changes.

---

## 5️⃣ Push Changes

```bash
git push origin main
```

Uploads local commits to the remote `main` branch.

---

## 6️⃣ Clone a Repository

```bash
git clone <repository-url>
```

Downloads a remote Git repository to your computer.

---

# ✍️ Writing Better Commit Messages

Good commit messages clearly describe what changed.

### ✅ Good Examples

```text
Added Login Page
```

```text
Fixed Navbar Bug
```

```text
Created User API
```

```text
Added JWT Authentication
```

### ❌ Avoid Vague Messages

```text
update
```

```text
final
```

```text
changes
```

A useful commit message should make it easier to understand the project's history later.

---

# 🌍 CORS

**CORS** stands for **Cross-Origin Resource Sharing**.

Browsers apply security rules when a frontend tries to communicate with a server from a different origin.

For example:

```text
Frontend → http://localhost:3000
Backend  → http://localhost:5000
```

These are different origins because their ports are different.

The backend can configure CORS to allow requests from specific origins.

---

## Enable CORS in Express

First install the package:

```bash
npm install cors
```

Then:

```javascript
const cors = require("cors");

app.use(cors());
```

This is convenient during development, but production applications should generally restrict allowed origins.

---

## Production Example

```javascript
app.use(
  cors({
    origin: "https://yourwebsite.com"
  })
);
```

This tells the server which frontend origin is allowed to make browser-based cross-origin requests.

### 🔐 Security Tip

Avoid blindly allowing every origin in production unless there is a specific reason to do so.

---

# ⚡ Quick Revision

```text
Route       → Decides where the request goes
Controller  → Handles the request logic
Model       → Works with application data
API         → Allows applications to communicate
JSON        → Common data-exchange format
Postman     → Used for API testing
req         → Incoming request information
res         → Sends a response
Middleware  → Runs during the request-response cycle
Git         → Version control
GitHub      → Online Git collaboration platform
CORS        → Controls cross-origin browser requests
```

---

# 🔄 Full Backend Request Flow

Whenever I work on a backend project, I can remember the basic flow like this:

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
                 Model
                   │
                   ▼
               Database
                   │
                   ▼
                 Model
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

---

