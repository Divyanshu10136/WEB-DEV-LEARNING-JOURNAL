
# API, HTTP & REST Notes


---

# 📚 Table of Contents

- [Day 1 – API Fundamentals](#day-1--api-fundamentals)
  - [What is an API?](#what-is-an-api)
  - [Client–Server Architecture](#clientserver-architecture)
  - [HTTP Request & Response](#http-request--response)
  - [JSON](#json)
  - [HTTP Methods](#http-methods-get-post-put-patch-delete)
  - [GET vs POST](#get-vs-post)
  - [HTTP Status Codes](#http-status-codes-basics)
  - [REST URL Design](#rest-url-design)
  - [fetch() API](#fetch-api-then-asyncawait-sending-requests-parsing-responses)
  - [Error Handling](#error-handling-with-fetch)
  - [Authentication](#authentication-basics-api-keys-bearer-tokens-env)
  - [Express](#introduction-to-express-creating-basic-apis)
  - [PokeAPI Demo](#live-api-demo-pokeapi)
- [Day 2 – REST APIs & Backend](#day-2--rest-apis--backend)
  - [REST Architecture](#rest-api-architecture--request-flow)
  - [Resources & CRUD](#resources--crud-operations)
  - [HTTP Methods Deep Dive](#deep-dive-into-http-methods-safe--idempotent)
  - [HTTP Headers](#http-headers)
  - [Detailed Status Codes](#http-status-codes-detailed)
  - [REST Naming & Versioning](#rest-naming-conventions--api-versioning)
  - [API Best Practices](#api-design-best-practices-consistency)
  - [Request Lifecycle](#complete-request-lifecycle)
- [Day 3 – Advanced REST & Browser Storage](#day-3--advanced-rest--browser-storage)
  - [REST Concepts](#rest-concepts-revision)
  - [Resources vs Action Endpoints](#resources-vs-action-endpoints)
  - [Nested REST URLs](#nested-rest-urls)
  - [Browser Storage](#browser-storage)
  - [Storage Comparison](#browser-storage-comparison)
  - [Real-World Use Cases](#real-world-browser-storage-use-cases)
- [Day 4 – Frontend, Backend & Server](#day-4--frontend-backend--the-server)
  - [Frontend](#frontend)
  - [Backend](#backend)
  - [Application Server](#the-application-server)
  - [Complete Web Flow](#the-complete-web-flow)
  - [Frontend vs Backend](#frontend-vs-backend-at-a-glance)
  - [Library vs Framework](#library-vs-framework)

---

# Day 1 – API Fundamentals

## What is an API?

An **API (Application Programming Interface)** is a set of rules that allows one piece of software to communicate with another.

It defines:

- How a request should be made
- What data can be requested
- What response will be returned

The client does **not** directly access the server's database. The API sits between the client and server and handles communication.

### Real-Life Example

When you use a weather app:

```text
Weather App
    ↓
Weather API
    ↓
Server
    ↓
Weather Database
    ↓
Server
    ↓
Weather API
    ↓
Weather App
````

### Roles Involved

| Role     | Description                                                          |
| -------- | -------------------------------------------------------------------- |
| Client   | Makes the request, such as a browser, mobile app, or JavaScript code |
| API      | Interface responsible for communication                              |
| Server   | Processes the request and prepares the response                      |
| Database | Stores the actual data                                               |

---

# Client–Server Architecture

Client-server architecture is a model where the client sends requests and the server processes them and sends back responses.

### Rules

* The client initiates communication.
* The server responds to requests.
* HTTP is stateless.
* Each request must contain the information required to process it.
* Authentication information can be provided using cookies or tokens.

### Example

When Divyanshu Shah logs into an email application:

```text
Browser
   ↓
POST /login
   ↓
Server
   ↓
Authentication
   ↓
Response + Token/Cookie
```

On future requests, the browser sends the authentication information again.

---

# HTTP Request & Response

**HTTP (HyperText Transfer Protocol)** defines how clients and servers exchange messages.

Every interaction consists of:

```text
Client → Request → Server
Client ← Response ← Server
```

## Request Structure

| Part    | Description                                      |
| ------- | ------------------------------------------------ |
| Method  | Action to perform: GET, POST, PUT, PATCH, DELETE |
| URL     | Address of the requested resource                |
| Headers | Additional request information                   |
| Body    | Data being sent to the server                    |

## Response Structure

| Part        | Description                  |
| ----------- | ---------------------------- |
| Status Code | Indicates success or failure |
| Headers     | Response metadata            |
| Body        | Actual response data         |

### Example

A login request might look like:

```http
POST /login
Content-Type: application/json

{
  "email": "divyanshu@example.com",
  "password": "mypassword123"
}
```

The server might respond:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "Login successful"
}
```

---

# JSON

**JSON (JavaScript Object Notation)** is a lightweight text format commonly used to exchange data between clients and servers.

```json
{
  "name": "iPhone 16 Pro",
  "price": 134900,
  "inStock": true
}
```

## Important JavaScript Methods

### `response.json()`

Converts JSON received from the server into a JavaScript object.

```javascript
const data = await response.json();
```

### `JSON.stringify()`

Converts a JavaScript object into JSON text.

```javascript
const body = JSON.stringify({
  name: "Divyanshu Shah",
  age: 22
});
```

---

# HTTP Methods: GET, POST, PUT, PATCH, DELETE

HTTP methods describe what action should be performed on a resource.

| Method | Action  | Meaning                    |
| ------ | ------- | -------------------------- |
| GET    | Read    | Retrieve existing data     |
| POST   | Create  | Create new data            |
| PUT    | Replace | Replace an entire resource |
| PATCH  | Update  | Update part of a resource  |
| DELETE | Remove  | Delete a resource          |

### Example

For a mobile API:

```http
GET    /mobiles
GET    /mobiles/5
POST   /mobiles
PUT    /mobiles/5
PATCH  /mobiles/5
DELETE /mobiles/5
```

---

# GET vs POST

| Feature             | GET                  | POST             |
| ------------------- | -------------------- | ---------------- |
| Purpose             | Read data            | Create/send data |
| Data location       | URL/query parameters | Request body     |
| Changes server data | Usually no           | Usually yes      |
| Safe                | Yes                  | No               |
| Typical response    | 200                  | 201              |

### GET

```http
GET /mobiles?search=iphone
```

### POST

```http
POST /mobiles

{
  "name": "iPhone 16 Pro",
  "price": 134900
}
```

---

# HTTP Status Codes Basics

HTTP status codes are three-digit numbers describing the result of a request.

| Family | Meaning       |
| ------ | ------------- |
| 1xx    | Informational |
| 2xx    | Success       |
| 3xx    | Redirection   |
| 4xx    | Client Error  |
| 5xx    | Server Error  |

## Common Status Codes

| Code | Name                  | Meaning                                 |
| ---- | --------------------- | --------------------------------------- |
| 200  | OK                    | Request succeeded                       |
| 201  | Created               | Resource was created                    |
| 204  | No Content            | Request succeeded without response data |
| 400  | Bad Request           | Invalid or malformed request            |
| 401  | Unauthorized          | Authentication required/invalid         |
| 403  | Forbidden             | Authenticated but not permitted         |
| 404  | Not Found             | Resource does not exist                 |
| 429  | Too Many Requests     | Rate limit exceeded                     |
| 500  | Internal Server Error | Server-side failure                     |

> **Important:** Always check the status code before assuming the response was successful.

---

# REST URL Design

REST uses:

* **Nouns** in URLs to identify resources
* **HTTP methods** to describe actions

### Correct

```http
GET    /mobiles
GET    /mobiles/5
POST   /mobiles
PUT    /mobiles/5
PATCH  /mobiles/5
DELETE /mobiles/5
```

### Incorrect

```http
GET  /getAllMobiles
POST /createNewMobile
POST /deleteMobile5
```

The URL should identify **what** we are working with, while the HTTP method identifies **what we want to do**.

---

# fetch() API: `.then()`, async/await, Sending Requests & Parsing Responses

`fetch()` is a built-in JavaScript API used to make HTTP requests.

## Using `.then()`

```javascript
fetch("https://example.com/login", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    email: "divyanshu@example.com",
    password: "mypassword123"
  })
})
  .then(res => res.json())
  .then(data => console.log("Logged in:", data))
  .catch(err => console.log("Error:", err));
```

## Using async/await

```javascript
async function loginUser() {
  const res = await fetch("https://example.com/login", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      email: "divyanshu@example.com",
      password: "mypassword123"
    })
  });

  const data = await res.json();

  console.log("Logged in:", data);
}
```

## Why Are There Two `await`s?

Both `fetch()` and `res.json()` are asynchronous.

```javascript
const res = await fetch(url);
const data = await res.json();
```

### First `await`

Waits for the HTTP request and response.

### Second `await`

Reads and parses the response body as JSON.

---

# Sending Data with fetch()

```javascript
async function createAccount() {
  const res = await fetch("https://example.com/signup", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      name: "Divyanshu Shah",
      email: "divyanshu@example.com",
      password: "mypassword123"
    })
  });

  const data = await res.json();

  console.log(data);
}
```

| Field     | Meaning                    |
| --------- | -------------------------- |
| `method`  | HTTP method                |
| `headers` | Metadata about the request |
| `body`    | Data being sent            |

---

# Error Handling with fetch()

A very important point:

> `fetch()` does **not** reject automatically for HTTP errors such as `404` or `500`.

It rejects mainly when a network-level failure occurs.

Therefore, check `res.ok`.

```javascript
async function loginUser() {
  try {
    const res = await fetch("https://example.com/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        email: "divyanshu@example.com",
        password: "wrongpassword"
      })
    });

    if (!res.ok) {
      throw new Error(`Login failed with status ${res.status}`);
    }

    const data = await res.json();

    console.log("Welcome:", data);

  } catch (err) {
    console.log("Something went wrong:", err.message);
  }
}
```

### `res.ok`

```javascript
true
```

for status codes:

```text
200–299
```

and:

```javascript
false
```

for other status codes.

---

# Authentication Basics: API Keys, Bearer Tokens & `.env`

Authentication proves the identity of a client.

## API Keys

An API key is usually sent through a request header.

```javascript
fetch("https://api.example.com/data", {
  headers: {
    "x-api-key": "your-secret-key"
  }
});
```

## Bearer Tokens

Bearer tokens are commonly sent through the `Authorization` header.

```javascript
fetch("https://api.example.com/profile", {
  headers: {
    "Authorization": "Bearer YOUR_TOKEN"
  }
});
```

## `.env`

Secret values should be stored outside source code.

```env
API_KEY=abc123secretkey
```

Backend:

```javascript
const apiKey = process.env.API_KEY;
```

### Important Security Rules

Never put secret keys:

* In frontend JavaScript
* In public GitHub repositories
* Directly into source code
* Into committed `.env` files

Add `.env` to `.gitignore`:

```gitignore
.env
.env.*
```

> Anything shipped to the browser can potentially be inspected by users.

---

# Introduction to Express: Creating Basic APIs

Express is a Node.js framework used to create HTTP servers and APIs.

```javascript
const express = require("express");

const app = express();

app.use(express.json());

app.get("/mobiles", (req, res) => {
  res.status(200).json({
    mobiles: ["iPhone 16", "Galaxy S25"]
  });
});

app.post("/login", (req, res) => {
  const { email, password } = req.body;

  if (password !== "mypassword123") {
    return res.status(403).json({
      message: "Incorrect email or password"
    });
  }

  res.status(200).json({
    message: "Login successful"
  });
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

# Live API Demo: PokeAPI

A real public API can be accessed directly from JavaScript.

```javascript
async function getPokemon(name) {
  const res = await fetch(
    `https://pokeapi.co/api/v2/pokemon/${name}`
  );

  if (!res.ok) {
    console.log("Pokemon not found, status:", res.status);
    return;
  }

  const data = await res.json();

  console.log(data.name, data.height, data.weight);
}

getPokemon("pikachu");
```

Example output:

```text
pikachu 4 60
```

---

# Day 2 – REST APIs & Backend

# REST API Architecture & Request Flow

REST (**Representational State Transfer**) is an architectural style for designing APIs.

Each piece of data is treated as a **resource** and can be represented using a URL.

### Request Flow

```text
Client
  ↓
API
  ↓
Server
  ↓
Database
  ↓
Server
  ↓
API
  ↓
Client
```

Usually the response is returned as JSON.

---

# Resources & CRUD Operations

A **resource** is a noun representing something the application manages.

Examples:

```text
Student
Product
Order
Document
Course
```

### Resource URLs

```http
/students
/students/99
/students/99/courses
```

## CRUD

CRUD means:

* Create
* Read
* Update
* Delete

| CRUD   | HTTP      | SQL    | Example               |
| ------ | --------- | ------ | --------------------- |
| Create | POST      | INSERT | `POST /students`      |
| Read   | GET       | SELECT | `GET /students/99`    |
| Update | PUT/PATCH | UPDATE | `PATCH /students/99`  |
| Delete | DELETE    | DELETE | `DELETE /students/99` |

---

# Deep Dive into HTTP Methods: Safe & Idempotent

## Safe

A method is **safe** when it does not change server data.

Example:

```http
GET /students
```

## Idempotent

A method is **idempotent** when making the same request multiple times has the same intended effect as making it once.

### Method Cheat Sheet

| Method | Safe | Idempotent | Body       |
| ------ | ---- | ---------- | ---------- |
| GET    | Yes  | Yes        | Usually No |
| POST   | No   | No         | Yes        |
| PUT    | No   | Yes        | Yes        |
| PATCH  | No   | Usually    | Yes        |
| DELETE | No   | Yes        | Rarely     |

---

# GET

GET retrieves data.

```http
GET /students
GET /students/99
```

### Characteristics

* Safe
* Idempotent
* Does not normally modify data

---

# POST

POST is generally used to create a new resource.

```http
POST /students
```

Example body:

```json
{
  "name": "Divyanshu Shah",
  "age": 22
}
```

Typical response:

```http
201 Created
```

POST is generally **not idempotent** because repeating it may create multiple resources.

---

# PUT

PUT replaces an entire resource.

```http
PUT /students/99
```

Example:

```json
{
  "name": "Divyanshu Shah",
  "age": 22,
  "course": "B.Tech"
}
```

If fields are omitted, depending on the API implementation, they may be replaced or reset.

PUT is idempotent.

---

# PATCH

PATCH updates only selected fields.

```http
PATCH /students/99
```

Example:

```json
{
  "age": 23
}
```

The remaining student information stays unchanged.

---

# DELETE

DELETE removes a resource.

```http
DELETE /students/99
```

DELETE is idempotent in the HTTP sense: after the resource has been deleted, repeating the request does not recreate it.

A second request may return:

```http
404 Not Found
```

---

# HTTP Headers

Headers are key-value pairs containing metadata.

| Header          | Purpose                      |
| --------------- | ---------------------------- |
| `Content-Type`  | Format of data being sent    |
| `Accept`        | Format the client wants back |
| `Authorization` | Authentication information   |
| `Cookie`        | Browser cookie data          |
| `Host`          | Requested host               |
| `User-Agent`    | Identifies the client        |

### Content-Type

```http
Content-Type: application/json
```

means:

> "The body I am sending is JSON."

### Accept

```http
Accept: application/json
```

means:

> "I want the response in JSON format."

---

# HTTP Status Codes – Detailed

## 1xx

| Code | Meaning             |
| ---- | ------------------- |
| 100  | Continue            |
| 101  | Switching Protocols |

## 2xx

| Code | Meaning    |
| ---- | ---------- |
| 200  | OK         |
| 201  | Created    |
| 202  | Accepted   |
| 204  | No Content |

## 3xx

| Code | Meaning            |
| ---- | ------------------ |
| 301  | Moved Permanently  |
| 302  | Found              |
| 304  | Not Modified       |
| 307  | Temporary Redirect |
| 308  | Permanent Redirect |

## 4xx

| Code | Meaning                |
| ---- | ---------------------- |
| 400  | Bad Request            |
| 401  | Unauthorized           |
| 403  | Forbidden              |
| 404  | Not Found              |
| 405  | Method Not Allowed     |
| 406  | Not Acceptable         |
| 408  | Request Timeout        |
| 409  | Conflict               |
| 410  | Gone                   |
| 415  | Unsupported Media Type |
| 422  | Unprocessable Entity   |
| 429  | Too Many Requests      |

## 5xx

| Code | Meaning               |
| ---- | --------------------- |
| 500  | Internal Server Error |
| 501  | Not Implemented       |
| 502  | Bad Gateway           |
| 503  | Service Unavailable   |
| 504  | Gateway Timeout       |

### 401 vs 403

This is an important interview question.

**401:**

> "I don't know who you are."

The client is not properly authenticated.

**403:**

> "I know who you are, but you aren't allowed to do this."

The client is authenticated but does not have permission.

---

# REST Naming Conventions & API Versioning

Good REST URLs should be:

* Nouns
* Plural
* Lowercase
* Predictable
* Consistent

### Incorrect vs Correct

| Incorrect                      | Correct                    |
| ------------------------------ | -------------------------- |
| `GET /getStudents`             | `GET /students`            |
| `POST /createNewStudent`       | `POST /students`           |
| `GET /student`                 | `GET /students`            |
| `GET /Students_List`           | `GET /students`            |
| `POST /students/delete/99`     | `DELETE /students/99`      |
| `GET /students/99/get-courses` | `GET /students/99/courses` |

---

# API Versioning

API versioning allows APIs to evolve without breaking existing clients.

```http
/api/v1/students
/api/v2/students
```

Old applications can continue using:

```text
v1
```

while newer applications use:

```text
v2
```

---

# API Design Best Practices

A good API should be consistent.

### Checklist

* Use plural nouns.
* Use lowercase URLs.
* Use kebab-case when needed.
* Use query parameters for filtering.
* Use API versioning.
* Use appropriate HTTP status codes.
* Return consistent JSON error structures.

### Example

```http
GET /api/v1/students?year=2&sort=name
```

---

# Complete Request Lifecycle

A typical web request can follow this path:

```text
Browser
   ↓
DNS
   ↓
TCP/TLS
   ↓
API Gateway / Server
   ↓
Application Logic
   ↓
Database
   ↓
Application Server
   ↓
JSON Response
   ↓
Browser
```

### Possible Failures

| Stage             | Possible Error   |
| ----------------- | ---------------- |
| DNS               | Server not found |
| API Gateway       | 401 / 403 / 429  |
| Controller        | 400 / 422        |
| Service           | 500              |
| Database/Upstream | 504              |

---

# Day 3 – Advanced REST & Browser Storage

# REST Concepts Revision

## Resource

A resource is a noun managed by the application.

Examples:

```text
Student
Document
Order
Product
```

## Endpoint

An endpoint is a specific combination of:

```text
HTTP Method + URL
```

Example:

```http
POST /api/documents
```

This means:

> Create a new document.

## Stateless API

A stateless API does not depend on memory of previous requests.

Each request must contain the information required to process it.

For example:

```http
Authorization: Bearer TOKEN
```

---

# Resources vs Action Endpoints

Most REST endpoints represent resources.

However, some operations are actions or computations.

| Type     | Endpoint                 | Purpose                   |
| -------- | ------------------------ | ------------------------- |
| Resource | `GET /api/documents/:id` | Get saved document        |
| Resource | `POST /api/documents`    | Create document           |
| Action   | `POST /api/ai/bullets`   | Generate AI bullet points |
| Action   | `POST /api/ats/check`    | Calculate ATS score       |
| Action   | `POST /api/exports/pdf`  | Generate PDF              |

For operations such as generating, calculating, or processing, a POST action endpoint can be appropriate.

---

# Nested REST URLs

Nested URLs represent relationships between resources.

```http
GET    /students/99/courses
POST   /api/documents/:id/sections
PATCH  /api/documents/:id/sections/:sectionId
DELETE /api/documents/:id/sections/:sectionId
POST   /api/documents/:id/sections/:sectionId/items
```

### Example

A section belongs to a specific document:

```text
/api/documents/10/sections
```

A specific section:

```text
/api/documents/10/sections/4
```

### Public Share URLs

Some applications expose public resources without requiring normal authentication.

Example:

```http
GET /api/share/:slug
```

An unpredictable slug is preferable to exposing sequential IDs.

---

# Browser Storage

Browser storage allows websites to store data on the user's device.

The main options are:

1. Cookies
2. Local Storage
3. Session Storage
4. IndexedDB

---

# Cookies

Cookies are small pieces of data associated with a website.

The browser can automatically send cookies to the relevant server.

### Characteristics

* Small size
* Approximately 4 KB per cookie
* Can expire
* Sent with requests when applicable
* Commonly used for authentication/session management

### Common Uses

* Login sessions
* Remember me
* Shopping carts
* Preferences

---

# Local Storage

Local Storage stores data in the browser.

```javascript
localStorage.setItem("theme", "dark");

localStorage.getItem("theme");

localStorage.removeItem("theme");
```

### Characteristics

* Around 5–10 MB depending on browser
* Does not automatically expire
* Not automatically sent to the server
* Stores strings

### Object Example

```javascript
const user = {
  name: "Divyanshu Shah",
  age: 22
};

localStorage.setItem("user", JSON.stringify(user));

const savedUser = JSON.parse(
  localStorage.getItem("user")
);
```

---

# Session Storage

Session Storage stores data for the lifetime of a browser tab.

```javascript
sessionStorage.setItem("name", "Divyanshu Shah");

sessionStorage.getItem("name");
```

### Characteristics

* Temporary
* Cleared when the tab is closed
* Not sent automatically to the server
* Separate for each browser tab
* Stores strings

---

# IndexedDB

IndexedDB is a browser database designed for larger amounts of structured data.

It can store:

* Objects
* Files
* Images
* Binary data
* Large datasets

It is particularly useful for offline applications.

---

# Browser Storage Comparison

| Feature          | Cookies       | Local Storage | Session Storage | IndexedDB               |
| ---------------- | ------------- | ------------- | --------------- | ----------------------- |
| Approx. Size     | 4 KB          | 5–10 MB       | ~5 MB           | Hundreds of MB+         |
| Sent to Server   | Yes           | No            | No              | No                      |
| Expiration       | Yes           | No            | Tab close       | No automatic expiration |
| Large Data       | No            | Limited       | Limited         | Yes                     |
| Objects Directly | No            | No            | No              | Yes                     |
| Best Use         | Sessions/auth | Preferences   | Temporary data  | Offline/large data      |

> Local Storage and Session Storage store strings. Use `JSON.stringify()` and `JSON.parse()` for objects.

---

# Real-World Browser Storage Use Cases

| Application | Storage                 | Typical Use            |
| ----------- | ----------------------- | ---------------------- |
| Gmail       | Cookies                 | Authentication/session |
| YouTube     | Local Storage           | Preferences            |
| Amazon      | Cookies + Local Storage | Sessions/preferences   |
| Google Docs | IndexedDB               | Offline/local data     |
| Spotify     | IndexedDB               | Offline/cache data     |
| Google Maps | IndexedDB               | Offline map data       |

---

# Common Interview Questions

### Which storage is automatically sent to the server?

**Cookies**

### Which storage survives browser restart?

**Local Storage and IndexedDB**

### Which storage is cleared when the tab closes?

**Session Storage**

### Which storage is suitable for large offline data?

**IndexedDB**

### Can Local Storage directly store JavaScript objects?

No.

Objects need to be converted to strings:

```javascript
JSON.stringify(object)
```

and converted back:

```javascript
JSON.parse(string)
```

---

# Day 4 – Frontend, Backend & the Server

# The Two Halves of Every Web App

Every modern web application generally has two major sides:

```text
Frontend
   ↕
Backend
```

### Frontend

What the user sees and interacts with.

### Backend

The logic, data, authentication, and processing behind the application.

> **Frontend asks. Backend answers.**

---

# Frontend

The frontend runs inside the user's browser.

## Core Technologies

### HTML

Provides structure.

### CSS

Provides styling.

### JavaScript

Provides behavior and interaction.

## Popular Frontend Technologies

* React
* Angular
* Vue.js
* jQuery
* Backbone.js

### Frontend Responsibilities

* User interface
* Forms
* User interactions
* Animations
* Rendering data
* Calling APIs

---

# Backend

The backend runs on a server.

The user does not directly see the backend code.

## Popular Backend Technologies

* Node.js + Express
* Python
* Java
* PHP
* ASP.NET
* JSP

### Backend Responsibilities

* Business logic
* Database communication
* Authentication
* Authorization
* API development
* Data processing
* Sending responses

---

# The Application Server

An application server receives HTTP requests, executes backend logic, and returns responses.

For example:

```text
Node.js
   +
Express.js
   =
Application Server
```

Example:

```javascript
app.get("/students", (req, res) => {
  res.json({
    students: []
  });
});
```

---

# The Complete Web Flow

A typical web application works like this:

```text
Browser
   ↓
Frontend
   ↓
HTTP Request
   ↓
Application Server
   ↓
Business Logic
   ↓
Database
   ↓
Response
   ↓
JSON
   ↓
Frontend
   ↓
UI Update
```

---

# Frontend vs Backend

| Feature         | Frontend            | Backend                 |
| --------------- | ------------------- | ----------------------- |
| Runs in         | Browser             | Server                  |
| Visible to user | Yes                 | No                      |
| Main job        | UI & interaction    | Logic & data            |
| Technologies    | HTML, CSS, JS       | Node, Python, Java, PHP |
| Frameworks      | React, Angular, Vue | Express, Django, Spring |
| Talks to        | User + API          | API + Database          |

---

# Interview Definitions

### Frontend

> Frontend is the part of an application that users see and interact with.

### Backend

> Backend processes requests, runs business logic, manages data, and handles authentication and authorization.

### Application Server

> An application server such as Node.js + Express receives HTTP requests, executes backend logic, communicates with databases, and sends responses to clients.

### Complete Explanation

> The frontend sends an HTTP request to the application server. The server processes the request, executes business logic, communicates with the database when necessary, and returns a response, usually JSON. The frontend then uses that response to update the UI.

---

# Library vs Framework

One of the most important differences is:

> **Who controls the flow?**

---

# What is a Library?

A library is a collection of reusable code that your application calls when needed.

### Simple Explanation

A library is like a **toolbox**.

You decide:

* When to use it
* Which function to call
* How to structure your application

### Examples

* jQuery
* React
* Lodash
* Axios

### Example

```javascript
axios.get("/students");
```

Your code decides when Axios is used.

### When to Use a Library

Use a library when:

* You need a specific functionality
* You want flexibility
* You want to control application structure
* The project is small or medium-sized

---

# What is a Framework?

A framework provides a predefined structure for an application.

The framework controls the overall flow and calls your code when necessary.

This concept is known as:

> **Inversion of Control**

### Simple Explanation

A framework is like a house whose basic structure has already been built.

You decide what goes inside, but the basic structure is already defined.

### Examples

* Angular
* Express.js
* Django
* Spring

### When to Use a Framework

Use a framework when:

* Building a large application
* Working with a large team
* You need a consistent structure
* You want built-in patterns and conventions
* You want faster project setup

---

# Library vs Framework: Core Difference

> **In a library, you call the code.**
>
> **In a framework, the framework calls your code.**

---

# Quick Comparison

| Feature     | Library                      | Framework                        |
| ----------- | ---------------------------- | -------------------------------- |
| Control     | Your code controls the flow  | Framework controls the flow      |
| Flexibility | High                         | More structured                  |
| Learning    | Usually easier               | Usually takes longer             |
| Structure   | You decide                   | Framework provides structure     |
| Examples    | Axios, Lodash, jQuery, React | Angular, Express, Django, Spring |

---

# 🧠 Quick Revision Cheat Sheet

## API

```text
API = Interface between software systems
```

## HTTP

```text
HTTP = Protocol for client-server communication
```

## REST

```text
REST = Architectural style for designing APIs around resources
```

## CRUD

```text
Create → POST
Read   → GET
Update → PUT/PATCH
Delete → DELETE
```

## Common Status Codes

```text
200 → OK
201 → Created
204 → No Content
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
409 → Conflict
422 → Invalid Data
429 → Too Many Requests
500 → Server Error
503 → Service Unavailable
```

## Authentication

```text
API Key
Bearer Token
Cookie / Session
```

## Browser Storage

```text
Cookies       → Server communication / sessions
LocalStorage  → Persistent browser preferences
SessionStorage → Temporary tab data
IndexedDB     → Large/offline structured data
```

## Web Architecture

```text
Frontend
   ↓
HTTP
   ↓
Backend / Application Server
   ↓
Database
   ↓
JSON Response
   ↓
Frontend
```

## Library vs Framework

```text
Library:
You call it.

Framework:
It calls your code.
```

---

# 🎯 Important Interview Questions

### 1. What is an API?

An API is an interface that allows different software systems to communicate using predefined rules.

### 2. What is REST?

REST is an architectural style where resources are identified using URLs and HTTP methods describe operations on those resources.

### 3. What is the difference between PUT and PATCH?

**PUT** generally replaces the complete resource.

**PATCH** updates only selected fields.

### 4. What is the difference between 401 and 403?

**401:** Authentication is missing or invalid.

**403:** Authentication exists, but the client does not have permission.

### 5. Does fetch throw an error for 404?

No. `fetch()` normally resolves successfully at the network level. You should check:

```javascript
if (!res.ok) {
  // handle HTTP error
}
```

### 6. Why do we use JSON.stringify()?

To convert a JavaScript object into JSON text.

### 7. Why do we use response.json()?

To parse JSON response data into a JavaScript object.

### 8. What is a stateless API?

An API where each request contains all the information necessary for the server to process it.

### 9. What is CRUD?

```text
Create
Read
Update
Delete
```

### 10. What is the difference between frontend and backend?

Frontend handles the user interface and interaction, while backend handles business logic, data, authentication, and server-side processing.

---

# 👨‍💻 Author

**Divyanshu Shah**


