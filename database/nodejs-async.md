# Day 7 — Callbacks in Node.js and How to Escape Callback Hell



* Synchronous vs asynchronous code
* Blocking vs non-blocking behavior
* What a callback is
* Where callbacks are used
* Error-first callbacks in Node.js
* Callback hell
* Fixing callback hell with named functions
* Promises and `.then()`
* `async` / `await`
* Which approach is preferred in modern Node.js

---

# 7.1 Synchronous vs Asynchronous

Before understanding callbacks, we need to understand **synchronous** and **asynchronous** execution.

---

## Synchronous — Blocking

**Synchronous code** runs one operation at a time.

The next operation waits until the previous operation finishes.

Think of it like standing in a queue:

```text
Person 1
   ↓
Person 2
   ↓
Person 3
```

Person 2 cannot move forward until Person 1 is finished.

Example:

```js
console.log("Ordering Food");
console.log("Eat");
```

The output is:

```text
Ordering Food
Eat
```

The second line runs after the first line has completed.

Conceptually:

```text
"Ordering Food"
       │
       ▼
     done
       │
       ▼
     "Eat"
```

This is called **blocking** because the next operation cannot continue until the current operation finishes.

---

# 7.2 Asynchronous — Non-Blocking

Now imagine ordering a pizza.

You place the order:

```text
Order Pizza
     │
     ▼
Pizza is being prepared
```

You don't have to stand in the kitchen waiting.

You can:

* Watch TV
* Reply to messages
* Study
* Work on something else

When the pizza is ready, you get notified.

```text
Order Pizza
     │
     ├──────► Do other work
     │
     ├──────► Watch TV
     │
     ▼
Pizza Ready
     │
     ▼
Eat
```

This is the basic idea behind **asynchronous programming**.

The program starts a task that takes time and continues doing other work instead of freezing while waiting.

---

# 7.3 Synchronous vs Asynchronous

| Synchronous                      | Asynchronous                |
| -------------------------------- | --------------------------- |
| Blocking                         | Non-blocking                |
| Waits for operation to finish    | Doesn't block while waiting |
| Executes sequentially            | Can continue other work     |
| Queue analogy                    | Pizza-order analogy         |
| Simpler for immediate operations | Useful for slow operations  |

Common asynchronous operations in Node.js include:

* Reading files
* Writing files
* Database queries
* API requests
* Timers
* Network operations

---

# 7.4 Why Do We Need Callbacks?

If a task takes time, something needs to tell our program:

> "The task is finished. Here's the result."

That's where a **callback** comes in.

---

# 7.5 What Is a Callback?

**Callback — Definition:**
A callback is a function passed into another function so that the second function can call it later, usually after completing some work.

Simple example:

```js
function orderFood(callback) {
  console.log("Ordered food");

  callback();
}

orderFood(function() {
  console.log("Eat!");
});
```

Output:

```text
Ordered food
Eat!
```

The function:

```js
function() {
  console.log("Eat!");
}
```

is passed into:

```js
orderFood()
```

and then called by:

```js
callback();
```

---

# 7.6 Another Callback Example

```js
function setAlarm(callback) {
  console.log("Alarm ringing");

  callback();
}

setAlarm(function() {
  console.log("Wake up!");
});
```

Flow:

```text
setAlarm()
    │
    ▼
Alarm ringing
    │
    ▼
callback()
    │
    ▼
Wake up!
```

The callback represents:

> "After you're done, run this function."

---

# 7.7 Callback Example — Sending a Message

```js
function sendMsg(callback) {
  console.log("Message sent");

  callback();
}

sendMsg(function() {
  console.log("Got a reply!");
});
```

The pattern is:

```text
Start task
    │
    ▼
Task finishes
    │
    ▼
Callback executes
```

---

# 7.8 Where Are Callbacks Used?

Callbacks are especially common when working with operations that take time.

### File system

```js
fs.readFile("data.txt", callback);
```

### Database

```js
findUser(id, callback);
```

### API request

```js
request(url, callback);
```

### Timer

```js
setTimeout(callback, 1000);
```

### User interaction

```js
button.addEventListener("click", callback);
```

The general idea is:

```text
Start something
      │
      ▼
Wait for result
      │
      ▼
Run callback
```

---

# 7.9 Error-First Callbacks

Node.js traditionally follows a convention called the **error-first callback pattern**.

The callback's first argument is reserved for an error.

The second argument contains the successful result.

The basic structure is:

```js
callback(error, result);
```

If there is an error:

```js
callback(error);
```

If everything succeeds:

```js
callback(null, result);
```

---

# 7.10 Example — Reading a File

A simplified example:

```js
function readFile(name, callback) {
  if (!name) {
    return callback("No file name given");
  }

  callback(null, "File contents here");
}
```

Using it:

```js
readFile("notes.txt", function(error, data) {
  if (error) {
    console.log("Error:", error);
  } else {
    console.log("Got:", data);
  }
});
```

Notice the order:

```text
callback(error, data)
         │       │
         │       └── successful result
         └────────── error
```

---

# 7.11 Why Check the Error First?

Suppose:

```js
readFile("notes.txt", function(error, data) {
  if (error) {
    console.log("Error:", error);
    return;
  }

  console.log("Data:", data);
});
```

The flow becomes:

```text
Callback
   │
   ▼
Is there an error?
   │
 ┌─┴─┐
 ▼   ▼
Yes  No
 │    │
 ▼    ▼
Error Result
```

This makes it clear that the result should only be used when the operation succeeded.

---

# 7.12 Example — Division

Callbacks aren't only for databases or files.

```js
function divide(a, b, callback) {
  if (b === 0) {
    return callback("Cannot divide by zero");
  }

  callback(null, a / b);
}
```

Use it:

```js
divide(10, 2, function(error, result) {
  if (error) {
    console.log("Error:", error);
  } else {
    console.log("Answer:", result);
  }
});
```

Output:

```text
Answer: 5
```

If we do:

```js
divide(10, 0, function(error, result) {
  if (error) {
    console.log("Error:", error);
  }
});
```

we get:

```text
Error: Cannot divide by zero
```

---

# 7.13 Example — Finding a User

Imagine a database operation:

```js
function findUser(id, callback) {
  if (!id) {
    return callback("No id given");
  }

  callback(null, {
    name: "Divyanshu"
  });
}
```

Usage:

```js
findUser(5, function(error, user) {
  if (error) {
    console.log("Error:", error);
  } else {
    console.log("User:", user.name);
  }
});
```

The important pattern is:

```js
function(error, result) {
  if (error) {
    // handle error
  } else {
    // use result
  }
}
```

---

# 7.14 Callback Hell

Callbacks are useful.

But when many asynchronous operations depend on one another, callbacks can become difficult to manage.

This creates **Callback Hell**.

Imagine this morning routine:

```text
Wake up
   ↓
Brush
   ↓
Bath
   ↓
Get ready
   ↓
Leave
```

If every step depends on the previous step, callbacks can become nested.

```js
wake(function() {
  brush(function() {
    bath(function() {
      getReady(function() {
        leave(function() {
          console.log("Done!");
        });
      });
    });
  });
});
```

Visually:

```text
wake()
  └── brush()
       └── bath()
            └── getReady()
                 └── leave()
                      └── Done
```

The code keeps moving farther to the right.

---

# 7.15 Why Does Callback Hell Happen?

Suppose we have:

```text
Step 1
  ↓
Step 2
  ↓
Step 3
  ↓
Step 4
```

If each operation is asynchronous and requires the previous operation's result, we might write:

```js
step1(function() {
  step2(function() {
    step3(function() {
      step4(function() {
        // done
      });
    });
  });
});
```

Every callback gets placed inside the previous callback.

More steps mean more nesting.

That's why it starts looking like a triangle:

```text
┌────────────────────────────
│ step1(
│   └── step2(
│        └── step3(
│             └── step4(
│                  └── done
```

This is **callback hell**.

---

# 7.16 Callback Hell Example — Making Tea

Imagine making tea:

1. Boil water
2. Add tea
3. Pour tea
4. Serve tea

Using callbacks:

```js
boil(function() {
  addTea(function() {
    pour(function() {
      serve(function() {
        console.log("Tea ready!");
      });
    });
  });
});
```

The code works.

The problem is readability.

As the application becomes more complicated, error handling also becomes difficult.

---

# 7.17 Fix 1 — Named Functions

One simple solution is to move each callback into a named function.

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

we can write:

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

There is no deep triangle.

---

# 7.18 Named Functions — Tea Example

```js
function boil() {
  console.log("Boil");
  addTea();
}

function addTea() {
  console.log("Add tea");
  pour();
}

function pour() {
  console.log("Pour");
  ready();
}

function ready() {
  console.log("Tea done!");
}

boil();
```

The code is easier to follow:

```text
boil()
  ↓
addTea()
  ↓
pour()
  ↓
ready()
```

### Limitation

Named functions can reduce nesting, but they don't completely solve the problems of complex asynchronous code.

For real backend applications, **Promises** provide a much better solution.

---

# 7.19 Fix 2 — Promises

A **Promise** represents the eventual result of an asynchronous operation.

A Promise has three states:

```text
             Promise
                │
        ┌───────┴────────┐
        ▼                ▼
     Pending          Finished
                       │
                ┌──────┴──────┐
                ▼             ▼
             Resolved       Rejected
             Success          Error
```

### Pending

The operation hasn't finished yet.

### Resolved

The operation completed successfully.

### Rejected

Something went wrong.

---

# 7.20 Creating a Promise

```js
function cook() {
  return new Promise(function(resolve) {
    resolve("Food ready");
  });
}
```

The function returns a Promise.

When the work succeeds:

```js
resolve("Food ready");
```

provides the result.

We can receive that result using `.then()`:

```js
cook().then(function(result) {
  console.log(result);
});
```

Output:

```text
Food ready
```

---

# 7.21 Promise Rejection

A Promise can also fail.

```js
function cook() {
  return new Promise(function(resolve, reject) {
    const success = false;

    if (success) {
      resolve("Food ready");
    } else {
      reject("Food burned");
    }
  });
}
```

Then:

```js
cook()
  .then(function(result) {
    console.log(result);
  })
  .catch(function(error) {
    console.log("Error:", error);
  });
```

The two paths are:

```text
             cook()
               │
        ┌──────┴──────┐
        ▼             ▼
      resolve       reject
        │             │
        ▼             ▼
      .then()       .catch()
```

---

# 7.22 Promise Chaining

The biggest advantage is that Promises can be chained.

Instead of:

```js
step1(function() {
  step2(function() {
    step3(function() {
      step4(function() {
        // done
      });
    });
  });
});
```

we can write:

```js
step1()
  .then(step2)
  .then(step3)
  .then(step4)
  .then(function() {
    console.log("Done!");
  })
  .catch(function(error) {
    console.log("Error:", error);
  });
```

The code becomes much flatter.

```text
step1()
  ↓
.then(step2)
  ↓
.then(step3)
  ↓
.then(step4)
  ↓
Done
```

---

# 7.23 Fix 3 — `async` / `await`

Modern JavaScript provides an even cleaner way to work with Promises:

```text
async / await
```

`async` and `await` are built on top of Promises.

Example:

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

Output:

```text
Tea ready
```

---

# 7.24 What Does `await` Mean?

When we write:

```js
const result = await makeTea();
```

we are essentially saying:

> "Wait for this Promise to settle before continuing this async function."

It allows asynchronous code to be written in a style that looks sequential.

Instead of:

```js
makeTea().then(function(result) {
  console.log(result);
});
```

we can write:

```js
const result = await makeTea();

console.log(result);
```

This is generally easier to read.

---

# 7.25 `async` and `await` Example

```js
function getData() {
  return new Promise(function(resolve) {
    resolve("Data loaded");
  });
}

async function show() {
  const result = await getData();

  console.log(result);
}

show();
```

Flow:

```text
show()
  │
  ▼
getData()
  │
  ▼
Promise
  │
  ▼
await
  │
  ▼
result
  │
  ▼
console.log()
```

---

# 7.26 Error Handling with `async` / `await`

Promises use:

```js
.then()
.catch()
```

With `async` / `await`, we commonly use:

```js
try {
  // async operation
} catch (error) {
  // handle error
}
```

Example:

```js
async function serveTea() {
  try {
    const result = await makeTea();

    console.log(result);
  } catch (error) {
    console.log("Error:", error);
  }
}
```

This gives us one clean place for error handling.

---

# 7.27 Callback vs Promise vs Async/Await

| Approach        | Main Idea                                | Readability             |
| --------------- | ---------------------------------------- | ----------------------- |
| Callback        | Function called after operation finishes | Can become difficult    |
| Named functions | Move callbacks into separate functions   | Better for simple flows |
| Promise         | Represents future result                 | Good                    |
| `.then()`       | Handles Promise result                   | Good for chaining       |
| `async/await`   | Cleaner syntax for Promises              | Usually easiest to read |

---

# 7.28 Same Problem — Three Different Solutions

### Callback

```js
getUser(function(user) {
  getResume(user.id, function(resume) {
    getApplications(resume.id, function(applications) {
      console.log(applications);
    });
  });
});
```

Nested structure:

```text
getUser
  └── getResume
       └── getApplications
```

---

### Promise

```js
getUser()
  .then(user => getResume(user.id))
  .then(resume => getApplications(resume.id))
  .then(applications => {
    console.log(applications);
  })
  .catch(error => {
    console.log(error);
  });
```

Much flatter:

```text
getUser()
   ↓
getResume()
   ↓
getApplications()
   ↓
result
```

---

### Async/Await

```js
async function loadApplications() {
  try {
    const user = await getUser();
    const resume = await getResume(user.id);
    const applications = await getApplications(resume.id);

    console.log(applications);
  } catch (error) {
    console.log(error);
  }
}
```

This reads almost like normal step-by-step instructions.

```text
Get user
   ↓
Get resume
   ↓
Get applications
   ↓
Show applications
```

---

# 7.29 Important: `await` Does Not Block the Entire Node.js Server

A common misunderstanding is:

> "`await` means Node.js freezes."

That's not the right mental model.

`await` pauses the execution of the **current async function** while the Promise is settling. Node.js can continue handling other work.

For example:

```js
async function getUser() {
  const user = await database.findUser();

  console.log(user);
}
```

While the database operation is waiting, Node.js can continue processing other events.

That's one reason asynchronous programming is so important in backend development.

---

# 7.30 Best Practices for Callbacks

If you are working with callbacks, remember:

### 1. Follow the error-first convention

```js
callback(error, result);
```

### 2. Check errors first

```js
if (error) {
  return callback(error);
}
```

### 3. Avoid unnecessary nesting

Bad:

```js
step1(function() {
  step2(function() {
    step3(function() {
      step4(function() {
        // ...
      });
    });
  });
});
```

Prefer Promises or `async/await` for complex asynchronous flows.

### 4. Keep functions small

Each function should ideally have one clear responsibility.

### 5. Use `async/await` for readable modern Node.js code

```js
async function getData() {
  try {
    const data = await fetchData();

    return data;
  } catch (error) {
    console.log(error);
  }
}
```

---

# 7.31 Overall Flow

The evolution looks like this:

```text
Synchronous
    │
    │ Blocking
    ▼
Asynchronous
    │
    │ Callbacks
    ▼
Callback Hell
    │
    ├───────────────► Named Functions
    │
    ├───────────────► Promises + .then()
    │
    └───────────────► async / await
                            │
                            ▼
                     Clean async code
```

---

# Quick Recap

| Term                 | One-Line Meaning                                                   |
| -------------------- | ------------------------------------------------------------------ |
| Synchronous          | Code executes step by step and waits for each operation            |
| Blocking             | The current operation prevents the next work from proceeding       |
| Asynchronous         | An operation can continue later without blocking the whole program |
| Callback             | A function passed to another function to be called later           |
| Error-first callback | Node.js convention: `callback(error, result)`                      |
| Callback Hell        | Deeply nested callbacks that become difficult to read and maintain |
| Promise              | Represents a future asynchronous result                            |
| `resolve()`          | Successfully completes a Promise                                   |
| `reject()`           | Fails a Promise                                                    |
| `.then()`            | Handles a successful Promise result                                |
| `.catch()`           | Handles a rejected Promise                                         |
| `async`              | Declares a function that works with Promises                       |
| `await`              | Waits for a Promise inside an async function                       |
| `try/catch`          | Common error-handling pattern with async/await                     |

---

# Day 7 — Final Takeaway

The most important idea from today is:

```text
Callbacks
    ↓
Too much nesting
    ↓
Callback Hell
    ↓
Promises
    ↓
async / await
```

The same asynchronous task can be written as:

```js
// Callback
getData(function(data) {
  console.log(data);
});
```

Then:

```js
// Promise
getData()
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.log(error);
  });
```

And finally:

```js
// async/await
async function showData() {
  try {
    const data = await getData();

    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```


