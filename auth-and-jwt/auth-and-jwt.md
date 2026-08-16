
# 🔐 Authentication & JWT (JSON Web Token)

In this section, I learned how authentication works in a Node.js application using **JWT (JSON Web Tokens)**. I also explored secure password storage with hashing and learned how npm dependency versioning works with `^` and `~`.

---

## 📚 Topics Covered

* 🔒 Password Hashing
* 🔑 JWT Authentication
* 🧩 JWT Structure
* 🛡️ JWT Security
* ✅ JWT Verification
* 📦 npm Versioning
* `^` Caret vs `~` Tilde

---

# 🔒 Password Hashing

Passwords should **never be stored directly** in a database.

### ❌ Plain Text Password

```text
password123
```

If an attacker gains access to the database, storing passwords in plain text would expose every user's credentials.

Instead, applications store a **hashed version** of the password.

### ✅ Hashed Password

```text
password123
        ↓
$2b$10$KR0tMiO/l6JH51Bipc3...
```

Libraries such as **bcrypt** can be used to generate password hashes.

### 🔄 Hashing is One-Way

A password hash is designed to work in one direction:

```text
Password → Hash ✅
Hash → Original Password ❌
```

The original password cannot simply be recovered from the hash.

---

# 🔑 How Login Verification Works

When **Divyanshu** logs into the application:

1. The user enters their email and password.
2. The server retrieves the stored password hash.
3. `bcrypt` checks the entered password against the stored hash.
4. If the password is correct, authentication succeeds.
5. The server can then generate a JWT for the user.

> 💡 A password does not need to produce the exact same hash every time. Password-hashing libraries use a salt and provide a comparison function to safely verify it.

---

# 🔐 What is JWT?

**JWT** stands for **JSON Web Token**.

A JWT is a compact token that a server can issue after successful authentication. The client can then send the token with subsequent requests so the server can identify and authenticate the user.

Instead of sending the user's password with every request, the application uses the token.

---

# 🏗️ JWT Structure

A JWT contains **three sections**, separated by dots:

```text
Header.Payload.Signature
```

For example:

```text
xxxxx.yyyyy.zzzzz
```

---

## 1️⃣ Header

The header contains information about the token type and signing algorithm.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

* `alg` → Algorithm used for signing
* `typ` → Token type

---

## 2️⃣ Payload

The payload contains **claims** about the user or token.

Example:

```json
{
  "id": 1,
  "name": "Divyanshu",
  "email": "divyanshu@example.com"
}
```

The server can use these claims to identify the authenticated user.

### ⚠️ Important

The JWT payload is **encoded, not encrypted**.

Anyone who possesses the token can decode its payload.

Therefore, never put sensitive information inside the payload.

### ❌ Avoid storing

* Passwords
* OTPs
* Bank account information
* API secrets
* Private credentials
* Other confidential data

A JWT payload should contain only the information necessary for authentication or authorization.

---

# 3️⃣ Signature

The signature helps the server determine whether the token has been modified.

For an HMAC-based JWT, the signature is generated using information such as:

```text
Header + Payload + Secret Key
```

The server keeps the secret key private.

For example:

```env
JWT_SECRET=your-super-secret-key
```

If someone modifies the payload, they cannot create a valid signature without knowing the secret key.

---

# 🛡️ JWT Security

Imagine a valid token contains:

```json
{
  "id": 5
}
```

An attacker might decode the token and change it to:

```json
{
  "id": 1
}
```

However, changing the payload also makes the original signature invalid.

When the server verifies the token:

```javascript
jwt.verify(token, process.env.JWT_SECRET);
```

the verification will fail if the signature does not match.

The server can then reject the request.

### 🔐 Key Idea

> JWT signatures provide **integrity and authenticity** for the token. They do not make the payload secret.

---

# 🔄 JWT Authentication Flow

```text
                User Login
                    │
                    ▼
            Email + Password
                    │
                    ▼
          Verify Password Hash
                    │
                    ▼
             Authentication
                 Success
                    │
                    ▼
             Generate JWT
                    │
                    ▼
          Send Token to Client
                    │
                    ▼
       Client Sends Token with Request
                    │
                    ▼
            Server Verifies JWT
                    │
              ┌─────┴─────┐
              ▼           ▼
            Valid       Invalid
              │           │
              ▼           ▼
        Access Granted   Reject
             ✅            ❌
```

---

# 📦 npm Versioning

When installing dependencies in a Node.js project, versions are usually specified inside `package.json`.

Example:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

A package version generally follows:

```text
MAJOR.MINOR.PATCH
```

For example:

```text
4.17.21
```

Where:

* `4` → Major version
* `17` → Minor version
* `21` → Patch version

---

# 📌 npm Version Symbols

## 1️⃣ Exact Version

```json
"express": "4.17.21"
```

This specifies exactly version `4.17.21`.

It does not request newer versions through the version range.

---

## 2️⃣ Tilde (`~`)

```json
"express": "~4.17.21"
```

The tilde generally allows **patch-level updates** within the same minor version.

Examples:

```text
✅ 4.17.22
✅ 4.17.25
❌ 4.18.0
❌ 5.0.0
```

---

## 3️⃣ Caret (`^`)

```json
"express": "^4.17.21"
```

