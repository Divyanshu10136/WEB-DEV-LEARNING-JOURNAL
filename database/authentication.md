# Day 5 — Register API, Password Security, and Async JavaScript

> 

---

## 5.1 The Register API

The **Register API** is responsible for creating a new user account.

When a user registers, the backend receives:

* `name`
* `email`
* `password`

The backend then:

1. Checks whether the email is already registered.
2. Hashes the password.
3. Saves the user to the database.
4. Creates an authentication token.
5. Returns the user's information and token.

### Basic Flow

```text
User submits registration
          ↓
Check email already exists?
          ↓
       No ───────────────► Yes
        ↓                   ↓
 Hash password          Reject request
        ↓
 Save user
        ↓
 Generate token
        ↓
 Return response
```

### Example

```js
function register(name, email, password) {
  if (users.find(u => u.email === email)) {
    throw new Error("Email already registered");
  }

  const hash = bcrypt.hashSync(password, 10);

  const user = {
    id: nextId(),
    name,
    email,
    password: hash
  };

  users.push(user);

  const token = jwt.sign(
    { id: user.id },
    SECRET,
    { expiresIn: "7d" }
  );

  return {
    id: user.id,
    name,
    email,
    token
  };
}
```

### Step-by-step

#### Step 1 — Check whether the email already exists

```js
if (users.find(u => u.email === email)) {
  throw new Error("Email already registered");
}
```

We don't want two accounts using the same email address.

This is also enforced at the database level with:

```sql
email VARCHAR(45) NOT NULL UNIQUE
```

The application check gives the user a friendly error, while the database constraint provides another layer of protection.

---

## 5.2 Password Security

A password should **never be stored as plain text**.

Bad:

```text
email: divyanshu@gmail.com
password: mypassword123
```

If someone gets access to the database, they immediately know the user's password.

Instead, the password is passed through a **hashing algorithm**.

```text
Plain password
      ↓
   bcrypt
      ↓
Password hash
      ↓
Database
```

Example:

```js
const hash = bcrypt.hashSync(password, 10);
```

The database stores something similar to:

```text
$2b$10$KR0tMiO/l6JH51Bipc3EPu...
```

It does **not** store:

```text
mypassword123
```

---

## 5.3 Hashing vs Encryption

These two concepts are different.

### Encryption

Encryption is generally **reversible** when you have the correct key.

```text
Original data
     ↓
 Encryption
     ↓
Encrypted data
     ↓
 Decryption + key
     ↓
Original data
```

### Hashing

Hashing is designed to be **one-way**.

```text
Password
   ↓
 Hash
   ↓
Hash value
```

You don't decrypt the hash to get the original password.

### Simple Analogy

Think of hashing like putting a document through a special one-way shredder.

```text
Original document
       ↓
    Shredder
       ↓
 Shredded result
```

You cannot reconstruct the original document from the shredded result.

For password authentication, the application hashes the password attempt and uses the password-hashing algorithm's verification function to determine whether it matches the stored hash.

---

## 5.4 Why We Use `bcrypt`

`bcrypt` is a password-hashing algorithm commonly used for storing passwords securely.

Example:

```js
const hash = bcrypt.hashSync(password, 10);
```

The `10` is the **cost factor** (work factor).

A higher cost generally means more computational work is required to calculate the hash.

For login, we don't manually hash and compare strings. Instead, we use:

```js
bcrypt.compareSync(password, user.password)
```

This is important because password hashes such as bcrypt include a salt and are intentionally designed so that the same password does not simply produce one universal hash.

---

# 5.5 Login — Verifying the Password

During login, the user sends:

```text
email
password
```

The backend:

1. Finds the user by email.
2. Checks the supplied password against the stored hash.
3. Rejects the request if the credentials are invalid.
4. Creates a new token if authentication succeeds.

```js
function login(email, password) {
  const user = users.find(u => u.email === email);

  if (!user || !bcrypt.compareSync(password, user.password)) {
    throw new Error("Invalid credentials");
  }

  const token = jwt.sign(
    { id: user.id },
    SECRET,
    { expiresIn: "7d" }
  );

  return { token };
}
```

### Login Flow

```text
User enters email + password
             ↓
       Find user by email
             ↓
      User exists?
       /          \
     No            Yes
     ↓              ↓
  Reject       Compare password
                    ↓
              Password correct?
               /          \
             No            Yes
             ↓              ↓
          Reject       Generate JWT
                            ↓
                       Return token
```

---

## 5.6 Password Reset

When a user changes their password, the new password must also be hashed.

```js
function resetPassword(email, newPassword) {
  const user = users.find(u => u.email === email);

  if (!user) {
    throw new Error("User not found");
  }

  user.password = bcrypt.hashSync(newPassword, 10);

  return {
    message: "Password updated"
  };
}
```

The important part is:

```js
bcrypt.hashSync(newPassword, 10)
```

We **never** do:

```js
user.password = newPassword;
```

### What happens after reset?

```text
Old password
     ↓
Old hash
     ↓
Replaced

New password
     ↓
New hash
     ↓
Stored in database
```

Therefore:

```text
Old password → rejected
New password → accepted
```

---

# 5.7 Promises

A **Promise** represents the eventual result of an asynchronous operation.

For example:

* Database query
* API request
* Reading a file
* Timer
* Network request

A Promise can be in three states:

### 1. Pending

The operation is still running.

```text
Promise
  ↓
Pending
```

### 2. Fulfilled / Resolved

The operation completed successfully.

```text
Promise
  ↓
Resolved
  ↓
Result
```

### 3. Rejected

The operation failed.

```text
Promise
  ↓
Rejected
  ↓
Error
```

---

## 5.8 `.then()` and `.catch()`

`.then()` handles a successful Promise.

`.catch()` handles a rejected Promise.

```js
someTask()
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.log(error);
  });
```

Think of it as:

```text
        Promise
           ↓
      ┌────┴────┐
   Success     Failure
      ↓           ↓
   .then()     .catch()
```

---

# 5.9 Callback Hell

A **callback** is a function passed into another function to be executed later.

Callbacks are useful for asynchronous operations, but too many nested callbacks can make code difficult to read.

Example:

```js
wake(function() {
  brush(function() {
    bath(function() {
      getReady(function() {
        console.log("Ready!");
      });
    });
  });
});
```

The code starts moving farther and farther to the right:

```text
wake()
  └── brush()
        └── bath()
              └── getReady()
                    └── Ready!
```

This is commonly called **Callback Hell**.

---

## 5.10 Why Callback Hell Is a Problem

Callback-heavy code can become difficult because:

* ❌ Code becomes deeply nested.
* ❌ Reading the execution flow becomes harder.
* ❌ Error handling becomes complicated.
* ❌ Adding more steps makes the nesting worse.
* ❌ Maintaining the code becomes difficult.

For example:

```js
getUser(function(user) {
  getResume(user.id, function(resume) {
    getTemplate(resume.templateId, function(template) {
      generatePDF(template, resume, function(pdf) {
        uploadPDF(pdf, function(result) {
          console.log(result);
        });
      });
    });
  });
});
```

The business logic may be simple, but the structure becomes difficult to follow.

---

# 5.11 Solving Callback Hell — Named Functions

One basic solution is to move callbacks into named functions.

Instead of:

```js
wake(function() {
  brush(function() {
    bath(function() {
      console.log("Ready!");
    });
  });
});
```

We can write:

```js
function wake() {
  console.log("Wake");
  brush();
}

function brush() {
  console.log("Brush");
  bath();
}

function bath() {
  console.log("Bath");
  done();
}

function done() {
  console.log("Ready!");
}

wake();
```

Now the flow is:

```text
wake()
  ↓
brush()
  ↓
bath()
  ↓
done()
```

This removes the visual nesting.

However, this approach isn't always ideal for complex asynchronous operations.

---

# 5.12 Promises — A Better Solution

Instead of nesting callbacks, asynchronous operations can return Promises.

Example:

```js
function cook() {
  return new Promise(function(resolve) {
    resolve("Food ready");
  });
}

cook()
  .then(function(result) {
    console.log(result);
  });
```

The Promise represents the future result.

```text
cook()
  ↓
Promise
  ↓
resolve("Food ready")
  ↓
.then()
  ↓
"Food ready"
```

---

## 5.13 Promise Chaining

Promises can be chained.

```js
stepOne()
  .then(result => {
    return stepTwo(result);
  })
  .then(result => {
    return stepThree(result);
  })
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.log(error);
  });
```

Instead of:

```text
stepOne()
   └── stepTwo()
          └── stepThree()
                 └── result
```

We get:

```text
stepOne()
   ↓
stepTwo()
   ↓
stepThree()
   ↓
result
```

This is much easier to follow.

---

# 5.14 Async/Await

`async/await` is built on top of Promises.

It allows asynchronous code to be written in a style that looks more like normal sequential code.

```js
function makeTea() {
  return new Promise(function(resolve) {
    resolve("Tea ready");
  });
}

async function serveTea() {
  const result = await makeTea();

  console.log(result);
}

serveTea();
```

The important keywords are:

```js
async
await
```

### `async`

An `async` function always returns a Promise.

```js
async function hello() {
  return "Hello";
}
```

### `await`

`await` waits for a Promise's result **within the async function** before continuing to the next statement.

```js
const result = await makeTea();
console.log(result);
```

---

# 5.15 Error Handling with `try/catch`

One of the biggest advantages of `async/await` is clean error handling.

```js
async function registerUser() {
  try {
    const user = await createUser();

    console.log(user);
  } catch (error) {
    console.log("Something went wrong:", error);
  }
}
```

The flow is:

```text
async function
      ↓
 await operation
      ↓
 ┌────┴─────┐
Success    Error
   ↓          ↓
continue    catch
```

---

# 5.16 Real Backend Example

A registration process may involve several asynchronous operations:

```text
Receive request
      ↓
Check email
      ↓
Hash password
      ↓
Create user
      ↓
Generate token
      ↓
Send response
```

With `async/await`, the code can remain easy to read:

```js
async function register(name, email, password) {
  try {
    const existingUser = await User.findOne({
      where: { email }
    });

    if (existingUser) {
      throw new Error("Email already registered");
    }

    const hash = await bcrypt.hash(password, 10);

    const user = await User.create({
      name,
      email,
      password: hash
    });

    const token = jwt.sign(
      { id: user.id },
      SECRET,
      { expiresIn: "7d" }
    );

    return {
      id: user.id,
      name: user.name,
      email: user.email,
      token
    };
  } catch (error) {
    throw error;
  }
}
```

The exact implementation can vary depending on the libraries being used, but the important idea is the flow:

```text
Database operation
       ↓
      await
       ↓
Next operation
       ↓
      await
       ↓
Next operation
```

---

# 5.17 Callback vs Promise vs Async/Await

| Approach        | Main Idea                              | Readability                       |
| --------------- | -------------------------------------- | --------------------------------- |
| Callback        | Pass a function to execute later       | Can become difficult with nesting |
| Named functions | Move callbacks into separate functions | Better for simple flows           |
| Promise         | Represent a future result              | Good                              |
| `.then()`       | Chain Promise operations               | Good                              |
| `async/await`   | Write Promise-based code sequentially  | Usually easiest to read           |

### Evolution

```text
Callbacks
    ↓
Callback Hell
    ↓
Named Functions
    ↓
Promises
    ↓
.then()
    ↓
async/await
```

---

# 5.18 Important Concepts to Remember

### Password

```text
Plain password
      ↓
   bcrypt
      ↓
Password hash
      ↓
Database
```

Never store the plain password.

### Registration

```text
Register
   ↓
Check email
   ↓
Hash password
   ↓
Save user
   ↓
Generate token
   ↓
Return response
```

### Promise

```text
Pending
   ↓
 ┌─┴─────┐
 ↓       ↓
Resolve Reject
 ↓       ↓
.then   .catch
```

### Async/Await

```js
async function example() {
  try {
    const result = await someOperation();
    console.log(result);
  } catch (error) {
    console.log(error);
  }
}
```

---

# Quick Recap

| Term                   | One-Line Meaning                                                |
| ---------------------- | --------------------------------------------------------------- |
| **Register API**       | Creates a new user account                                      |
| **Hashing**            | One-way transformation used to protect passwords                |
| **bcrypt**             | Password-hashing library/algorithm                              |
| **Password hash**      | Scrambled representation stored instead of the password         |
| **Promise**            | Represents the future result of an asynchronous operation       |
| **Pending**            | Promise is still waiting                                        |
| **Resolved/Fulfilled** | Promise completed successfully                                  |
| **Rejected**           | Promise failed                                                  |
| **Callback**           | Function executed later by another function                     |
| **Callback Hell**      | Excessive nested callbacks that make code difficult to maintain |
| **`.then()`**          | Handles a successful Promise                                    |
| **`.catch()`**         | Handles a rejected Promise                                      |
| **`async`**            | Declares a function that works with Promises                    |
| **`await`**            | Waits for a Promise result inside an async function             |
| **`try/catch`**        | Handles errors cleanly in async code                            |

---

# Day 5 Summary

Today we connected **authentication** with the asynchronous programming concepts needed to build a real backend.

```text
                 DAY 5
                   │
       ┌───────────┴───────────┐
       ↓                       ↓
 Authentication          Async JavaScript
       │                       │
   ┌───┴────┐             ┌────┴─────┐
   ↓        ↓             ↓          ↓
Register   Login       Callbacks   Promises
   │        │             │          │
   ↓        ↓             ↓          ↓
Hash      Compare     Callback     .then()
Password  Hash        Hell            │
   │                      │            ↓
   ↓                      └──────► async/await
Create User
   │
   ↓
Generate Token
```

