# Angular Route Guards — Learning Notes

**Name:** Divyanshu
**Topic:** Route Guards


---

## What is a Route Guard?

A **route guard** is a piece of Angular logic that runs before a user enters a route. It checks whether the user is allowed to access that page.

I can think of it as a security gate:

```text
User tries to open a route
          ↓
     Route Guard
          ↓
   ┌──────┴──────┐
   ↓             ↓
Allowed        Blocked
   ↓             ↓
Page opens    Redirect
```

For example, in ResumeForge, the profile page should only be accessible after login. Without a guard, someone could directly type `/profile` into the browser and access the page. 

---

# Why Do We Need Route Guards?

Suppose my application contains:

```text
/login
/signup
/profile
/dashboard
```

The desired behavior is:

```text
Logged Out User
      ↓
 /profile
      ↓
   Blocked
      ↓
  /login
```

After login:

```text
Logged In User
      ↓
 /profile
      ↓
   Allowed
```

So route guards help control navigation based on authentication or other conditions.

---

# AuthGuard

The most common guard for protecting routes is **CanActivate**.

I can create a guard using Angular CLI:

```bash
ng generate guard auth
```

Short form:

```bash
ng g guard auth
```

When prompted, I can select `CanActivate`. 

---

## Example AuthGuard

```ts
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor(
    private auth: AuthService,
    private router: Router
  ) {}

  canActivate(): boolean {

    if (this.auth.getToken()) {
      return true;
    }

    this.router.navigate(['/login']);
    return false;
  }
}
```

The important method is:

```ts
canActivate()
```

It returns:

```text
true  → allow the route
false → stop the route
```

The guard checks whether a token exists. If there is no token, the user
is redirected to the login page. 

---

# Protecting a Route

After creating `AuthGuard`, I can attach it to a route.

Example:

```ts
const routes: Routes = [
  {
    path: 'login',
    component: LoginComponent
  },
  {
    path: 'profile',
    component: ProfileComponent,
    canActivate: [AuthGuard]
  },
  {
    path: '',
    redirectTo: 'login',
    pathMatch: 'full'
  }
];
```

Now Angular checks `AuthGuard` before opening `/profile`.

```text
/profile
    ↓
AuthGuard
    ↓
Token exists?
 ┌──┴──┐
Yes    No
 ↓      ↓
Open   /login
```

The same guard can be applied to multiple protected routes. 

---

# NoAuthGuard

There is also an opposite situation.

If a user is already logged in, I don't want them to see the login or
signup page again.

For this purpose, I can create a **NoAuthGuard**.

Its logic is the reverse of `AuthGuard`:

```text
AuthGuard
Token exists → Allow
No token     → Login

NoAuthGuard
No token     → Allow
Token exists → Profile
```

---

## Example NoAuthGuard

```ts
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class NoAuthGuard implements CanActivate {

  constructor(
    private auth: AuthService,
    private router: Router
  ) {}

  canActivate(): boolean {

    if (!this.auth.getToken()) {
      return true;
    }

    this.router.navigate(['/profile']);
    return false;
  }
}
```

This allows users without a token to access authentication pages. If a
token already exists, the user is sent to the profile page. 

---

# Applying Both Guards

My routing configuration can look like this:

```ts
const routes: Routes = [
  {
    path: 'login',
    component: LoginComponent,
    canActivate: [NoAuthGuard]
  },
  {
    path: 'signup',
    component: SignupComponent,
    canActivate: [NoAuthGuard]
  },
  {
    path: 'profile',
    component: ProfileComponent,
    canActivate: [AuthGuard]
  },
  {
    path: '',
    redirectTo: 'login',
    pathMatch: 'full'
  }
];
```

This creates a clean authentication flow:

```text
             Application
                  │
       ┌──────────┴──────────┐
       ↓                     ↓
  Login / Signup           Profile
       ↓                     ↓
 NoAuthGuard             AuthGuard
       ↓                     ↓
No token allowed       Token required
```



---

# Complete Authentication Flow

Route guards are one part of a larger authentication system.

The complete flow is:

```text
1. User logs in
        ↓
2. Server returns token
        ↓
3. Token is stored
        ↓
4. Interceptor attaches token to requests
        ↓
5. AuthGuard protects private routes
        ↓
6. User can access protected pages
```

So the three important pieces work together:

```text
Login
  ↓
Gets Token
  ↓
Interceptor
  ↓
Sends Token
  ↓
Route Guard
  ↓
Protects Pages
```

The class notes describe the guard as the final part of this
authentication flow. 

---

# Types of Angular Guards

There are several guard types worth knowing:

| Guard              | Purpose                                   |
| ------------------ | ----------------------------------------- |
| `CanActivate`      | Controls whether a route can be entered   |
| `CanDeactivate`    | Controls whether a user can leave a route |
| `CanActivateChild` | Protects child routes                     |
| `Resolve`          | Gets data before a route loads            |

For example, `CanDeactivate` can be useful in ResumeForge when a user
has entered information into a resume form but has not saved it yet. It
can help warn the user before leaving the page. 

---

# AuthGuard vs NoAuthGuard

| AuthGuard                  | NoAuthGuard                 |
| -------------------------- | --------------------------- |
| Protects private pages     | Protects login/signup pages |
| Requires a token           | Requires no token           |
| Used for profile/dashboard | Used for login/register     |
| No token → login           | Token exists → profile      |
| `true` when authenticated  | `true` when unauthenticated |

Easy way to remember:

```text
AuthGuard
     ↓
"Are you logged in?"
     ↓
Yes → Enter
No  → Login

NoAuthGuard
     ↓
"Are you already logged in?"
     ↓
No  → Enter
Yes → Profile
```

---

# ResumeForge Example

For my ResumeForge project, I can protect routes such as:

```text
/login
/signup
/profile
/resumes
/dashboard
/applications
```

A possible structure is:

```text
                    ResumeForge
                        │
          ┌─────────────┴─────────────┐
          ↓                           ↓
    Public Routes              Protected Routes
          ↓                           ↓
   Login / Signup            Profile / Resume
          ↓                           ↓
     NoAuthGuard               AuthGuard
```

This prevents unauthenticated users from directly opening private
ResumeForge pages.

---

# Testing the Guards

I should test both situations.

### Test 1 — Logged Out

```text
Log out
   ↓
Open /profile
   ↓
AuthGuard checks token
   ↓
No token
   ↓
Redirect to /login
```

### Test 2 — Logged In

```text
Log in
   ↓
Token exists
   ↓
Open /profile
   ↓
AuthGuard allows access
   ↓
Profile opens
```

### Test 3 — Logged In User Opens Login

```text
Already logged in
       ↓
Open /login
       ↓
NoAuthGuard checks token
       ↓
Token exists
       ↓
Redirect to /profile
```

These are the main checks from the session homework. 

---

# Quick Revision

```text
Route Guard
    ↓
Runs before route opens
    ↓
Checks a condition
    ↓
true  → Route allowed
false → Route blocked
```

### AuthGuard

```text
Token present
     ↓
Allow private page

Token missing
     ↓
Redirect to login
```

### NoAuthGuard

```text
Token missing
     ↓
Allow login/signup

Token present
     ↓
Redirect to profile
```

---

