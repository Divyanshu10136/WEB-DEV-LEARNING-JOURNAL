# Day 6 — Authentication, JWT Tokens, and npm Versioning

## What We Covered Today

Today we learned how authentication works in a backend application and how a server knows whether a user is logged in.

We covered:

* Registering a new user
* Securely hashing passwords with `bcrypt`
* Logging in and verifying passwords
* Resetting a password
* Understanding JWT tokens
* Understanding the three parts of a JWT
* Protecting routes with `jwt.verify()`
* Understanding `MAJOR.MINOR.PATCH`
* Understanding `~` and `^` in `package.json`

---

# 6.1 Authentication — The Big Picture

**Authentication** means verifying **who a user is**.

Think of authentication like entering a secure building:

1. **Register** → You create an account and receive an ID card.
2. **Login** → You show your ID card or credentials to prove who you are.
3. **Protected route** → Security checks your ID before allowing you inside.
4. **Reset password** → If you forget your password, you replace it with a new one.

```text
        ┌───────────┐
        │  REGISTER │
        └─────┬─────┘
              │
       Create account
       Hash password
       Issue token
              │
              ▼
        ┌───────────┐
        │   LOGIN   │
        └─────┬─────┘
              │
       Verify password
       Issue new token
              │
              ▼
     ┌─────────────────┐
     │ PROTECTED ROUTE │
     └───────┬─────────┘
             │
        Verify JWT
             │
       ┌─────┴─────┐
       ▼           ▼
     Valid       Invalid
       │           │
     Allow       Reject
```

---

# 6.2 Register — Creating an Account

When a user registers, the backend receives information such as:

```text
name
email
password
```

The backend should **never store the password directly**.

A simplified registration flow looks like this:

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

### Step by step

### 1. Check whether the email already exists

```js
users.find(u => u.email === email)
```

If the email already belongs to another account, registration is rejected.

### 2. Hash the password

```js
const hash = bcrypt.hashSync(password, 10);
```

The original password is never saved.

### 3. Save the user

```js
{
  id: 1,
  name: "Divyanshu",
  email: "divyanshu@example.com",
  password: "hashed-value"
}
```

### 4. Generate a JWT

```js
jwt.sign({ id: user.id }, SECRET, {
  expiresIn: "7d"
});
```

The user receives the token and can immediately use authenticated parts of the application.

---

# 6.3 Password Hashing

A password should **not** be stored like this:

```text
password: "mypassword123"
```

Instead, it should be stored as a hash:

```text
password:
$2b$10$KR0tMiO/l6JH51Bipc3EPuWnSvA9q6QxRx5lR59WI3YuCCvNJ8Smu
```

The important idea is:

```text
Plain password
      │
      ▼
   bcrypt
      │
      ▼
Password hash
```

The database stores the hash, not the original password.

---

## Why Hash Instead of Encrypt?

**Encryption** is generally designed to be reversible with a key.

**Password hashing** is designed to be one-way.

```text
password
   │
   ▼
 hash()
   │
   ▼
hashed password
```

You don't decrypt the hash during login.

Instead:

```text
Login password
      │
      ▼
 bcrypt.compare()
      │
      ├── Match ──► Login successful
      │
      └── No match ► Login rejected
```

### Analogy — A Paper Shredder

Imagine putting a document through a shredder.

You can't reconstruct the original document from the shredded pieces.

Similarly, a properly designed password hash isn't intended to be reversed back into the original password.

---

# 6.4 Why Does Login Work If the Password Is Hashed?

Suppose registration uses:

```text
mypassword123
```

It gets stored as a hash:

```text
$2b$10$....
```

Later, the user logs in with:

```text
mypassword123
```

The application doesn't compare:

```js
"mypassword123" === "$2b$10$...."
```

Instead, `bcrypt.compare()` checks whether the supplied password corresponds to the stored hash:

```js
bcrypt.compareSync(password, user.password);
```

Conceptually:

```text
                  Stored hash
                       │
                       ▼
Login password ──► bcrypt.compare()
                       │
                 ┌─────┴─────┐
                 ▼           ▼
               true        false
                 │           │
              Login       Reject
```

---

# 6.5 Login — Verifying Identity

A simplified login function:

```js
function login(email, password) {
  const user = users.find(u => u.email === email);

  if (
    !user ||
    !bcrypt.compareSync(password, user.password)
  ) {
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

### Flow

```text
Email + password
       │
       ▼
Find user by email
       │
       ▼
Compare password with stored hash
       │
   ┌───┴────┐
   ▼        ▼
Match     No match
   │        │
   ▼        ▼
Create    Reject
JWT
```

Every successful login can issue a **new token**.

---

# 6.6 Reset Password

Resetting a password means replacing the old password hash with a hash for the new password.

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
user.password = bcrypt.hashSync(newPassword, 10);
```

The new password is hashed before being stored.

```text
Old password
     │
     ▼
Old hash
     │
     X
     │
     ▼
New password
     │
     ▼
New hash
```

After the reset:

```text
Old password → rejected
New password → accepted
```

---

# 6.7 What Is a JWT?

**JWT** stands for **JSON Web Token**.

A JWT is commonly used to represent authenticated information between a client and server.

A JWT looks approximately like:

```text
xxxxx.yyyyy.zzzzz
```

It has three parts:

```text
HEADER.PAYLOAD.SIGNATURE
```

For example:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJpZCI6NSwicm9sZSI6InN0dWRlbnQifQ
.
QWWHS6sXqBYOHMTtPQm5JJ0imJ95FfNUmsgdfCIQGGg
```

---

# 6.8 The Three Parts of a JWT

## 1. Header

The header describes the token.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

`alg` tells us which signing algorithm is being used.

`typ` tells us that this is a JWT.

---

## 2. Payload

The payload contains claims/data.

Example:

```json
{
  "id": 5,
  "role": "student",
  "iat": 1750000000,
  "exp": 1750604800
}
```

Common claims include:

```text
id
role
iat
exp
```

### Important

The payload is **not encrypted**.

It is encoded.

Therefore:

> **Never put passwords, secrets, or other sensitive information inside a JWT payload.**

Think of the payload as information written inside a transparent envelope.

Anyone holding the token can decode and read it.

---

## 3. Signature

The signature is used to detect tampering.

Conceptually:

```text
Header + Payload + Secret
            │
            ▼
        Signature
```

If someone changes the payload, the signature will no longer match.

For example:

```text
Original:

id = 5
role = student
       │
       ▼
   valid signature


Tampered:

id = 999
role = admin
       │
       ▼
signature doesn't match
```

---

# 6.9 JWT Analogy — Sealed Envelope

Think about a JWT like a document inside an envelope.

```text
┌───────────────────────────────┐
│ Header                         │
│ Payload                        │
│                               │
│        SIGNATURE               │
└───────────────────────────────┘
```

Anyone may be able to read the information inside.

But the signature provides a way for the server to detect whether the contents were modified.

So:

```text
Encoded ≠ Encrypted
```

This is one of the most important things to remember about JWTs.

---

# 6.10 Creating a JWT

A JWT can be created using:

```js
const token = jwt.sign(
  { id: user.id },
  SECRET,
  { expiresIn: "7d" }
);
```

Here:

```js
{ id: user.id }
```

is the payload.

```js
SECRET
```

is the signing secret.

```js
{ expiresIn: "7d" }
```

sets the expiration time.

---

# 6.11 `jwt.verify()` — Protecting Routes

When a user requests a protected route, the server needs to verify the token.

```js
jwt.verify(token, SECRET);
```

Conceptually:

```text
Client sends JWT
       │
       ▼
Server receives token
       │
       ▼
jwt.verify(token, SECRET)
       │
   ┌───┴────┐
   ▼        ▼
 Valid    Invalid
   │        │
   ▼        ▼
Allow     Reject
request   request
```

If the token was modified, verification fails.

If the token is expired, verification also fails.

---

# 6.12 Example Protected Route

A simplified middleware might look like:

```js
function authenticate(req, res, next) {
  const token = req.headers.authorization;

  try {
    const user = jwt.verify(token, SECRET);

    req.user = user;

    next();
  } catch (error) {
    res.status(401).json({
      message: "Unauthorized"
    });
  }
}
```

Then a protected route can use the authenticated user:

```js
app.get("/profile", authenticate, (req, res) => {
  res.json({
    message: "Welcome!",
    userId: req.user.id
  });
});
```

The important idea is:

```text
Request
   │
   ▼
Authentication middleware
   │
   ▼
jwt.verify()
   │
   ├── Valid ──► Route
   │
   └── Invalid ► 401 Unauthorized
```

---

# 6.13 JWT Expiration

When we create a token like:

```js
jwt.sign(
  { id: user.id },
  SECRET,
  { expiresIn: "7d" }
);
```

the token expires after seven days.

That means:

```text
Day 1  → valid ✅
Day 3  → valid ✅
Day 6  → valid ✅
Day 7+ → expired ❌
```

The user can log in again and receive a fresh token.

---

# 6.14 Authentication Flow — Complete Picture

```text
                REGISTER
                   │
                   ▼
             Create account
                   │
                   ▼
             Hash password
                   │
                   ▼
             Save user
                   │
                   ▼
              Create JWT
                   │
                   ▼
                LOGIN
                   │
                   ▼
            Find user/email
                   │
                   ▼
          Compare password
                   │
             ┌─────┴─────┐
             ▼           ▼
           Match       Wrong
             │           │
             ▼           ▼
        Create JWT     Reject
             │
             ▼
       Protected Route
             │
             ▼
        jwt.verify()
             │
       ┌─────┴─────┐
       ▼           ▼
     Valid       Invalid
       │           │
       ▼           ▼
    Allow       Reject
```

---

# 6.15 npm Versioning

Now we move from authentication to package management.

When we install packages such as:

```bash
npm install express
```

the dependency version is recorded in `package.json`.

A version normally follows:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
4.17.21
```

---

## MAJOR

The first number:

```text
4
```

represents the major version.

A major release can contain breaking changes.

Example:

```text
4.x.x → 5.x.x
```

Your existing code might need changes.

---

## MINOR

The second number:

```text
17
```

represents the minor version.

Minor releases generally add functionality without intentionally breaking the existing API within the same major version.

Example:

```text
4.17.x → 4.18.x
```

---

## PATCH

The third number:

```text
21
```

represents the patch version.

Patch releases generally contain bug fixes and small corrections.

Example:

```text
4.17.21 → 4.17.22
```

---

# 6.16 Understanding `~`

Suppose `package.json` contains:

```json
"express": "~4.17.21"
```

The tilde generally allows patch-level updates within that minor version.

So versions such as:

```text
4.17.21 ✅
4.17.22 ✅
4.17.30 ✅
```

may satisfy the range.

But:

```text
4.18.0 ❌
5.0.0  ❌
```

do not satisfy that range.

### Simple memory trick

```text
~ = patch updates
```

---

# 6.17 Understanding `^`

Suppose:

```json
"express": "^4.17.21"
```

The caret generally allows compatible minor and patch updates while staying within major version `4`.

Examples:

```text
4.17.21 ✅
4.17.22 ✅
4.18.0  ✅
4.99.9  ✅
5.0.0   ❌
```

### Simple memory trick

```text
^ = minor + patch updates
```

For the common case of a package starting at a non-zero major version.

---

# 6.18 No Symbol

If we write:

```json
"express": "4.17.21"
```

npm uses that exact version range:

```text
4.17.21
```

No automatic movement to another version is requested by the range itself.

---

# 6.19 Quick npm Version Table

| Symbol | Example    | General Meaning                        |
| ------ | ---------- | -------------------------------------- |
| none   | `4.17.21`  | Exact version                          |
| `~`    | `~4.17.21` | Patch updates within `4.17.x`          |
| `^`    | `^4.17.21` | Minor + patch updates within major `4` |

### Easy way to remember

```text
4.17.21

4 = MAJOR
17 = MINOR
21 = PATCH


~4.17.21
      └── small movement → PATCH


^4.17.21
   └────── more movement → MINOR + PATCH
```

---

# 6.20 Important Security Ideas

There are several important security lessons from today's topic.

### Never store plain passwords

Bad:

```js
password: "mypassword123"
```

Good:

```js
password: bcrypt.hashSync(password, 10)
```

### Never put passwords inside JWTs

Bad:

```js
jwt.sign({
  id: user.id,
  password: user.password
}, SECRET);
```

Good:

```js
jwt.sign({
  id: user.id
}, SECRET);
```

### Always verify authentication tokens

```js
jwt.verify(token, SECRET);
```

### Give tokens an expiration time

```js
{
  expiresIn: "7d"
}
```

---

# Quick Recap

| Term           | One-Line Meaning                                                |
| -------------- | --------------------------------------------------------------- |
| Authentication | Verifying who a user is                                         |
| Hashing        | One-way transformation used to protect passwords                |
| bcrypt         | A password-hashing algorithm/library                            |
| JWT            | Token commonly used to represent authenticated information      |
| Header         | JWT metadata such as algorithm and token type                   |
| Payload        | JWT claims/data; readable when decoded                          |
| Signature      | Helps detect whether the token was tampered with                |
| `jwt.sign()`   | Creates a JWT                                                   |
| `jwt.verify()` | Verifies a JWT                                                  |
| `expiresIn`    | Controls token expiration                                       |
| MAJOR          | Potentially breaking version changes                            |
| MINOR          | New compatible functionality                                    |
| PATCH          | Bug fixes/corrections                                           |
| `~`            | Generally allows patch updates                                  |
| `^`            | Generally allows minor + patch updates within the major version |

---

# Day 6 — Final Flow

```text
              USER
               │
        ┌──────┴──────┐
        ▼             ▼
     REGISTER       LOGIN
        │             │
        ▼             ▼
  Hash password   Find user
        │             │
        ▼             ▼
   Save user      Compare hash
        │             │
        └──────┬──────┘
               ▼
           JWT TOKEN
               │
               ▼
      header.payload.signature
               │
               ▼
       Protected API Route
               │
               ▼
          jwt.verify()
               │
        ┌──────┴──────┐
        ▼             ▼
      Valid         Invalid
        │             │
        ▼             ▼
      Access        Reject
```

And for npm:

```text
MAJOR.MINOR.PATCH
   │     │     │
   │     │     └── Bug fixes
   │     └──────── New features
   └────────────── Breaking changes


~4.17.21 → patch updates
^4.17.21 → minor + patch updates
4.17.21  → exact version
```

