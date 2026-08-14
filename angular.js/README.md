
# Angular.js Learning Journal

**Name:** Divyanshu Shah  
**Repository:** WEB-DEV-LEARNING-JOURNAL  
**Topic:** Angular.js  
**Purpose:** Learning and practising Angular concepts

---


## 📚 Topics Covered

### Day 1 — Angular Introduction

In the first session, I learned the basic idea of Angular and why it is
used for building web applications.

Topics covered:

- Introduction to Angular
- Angular features
- Angular application structure
- TypeScript
- Angular CLI
- Basic Angular workflow

---

### Day 2 — Components

I learned how Angular applications are divided into reusable
components.

Topics covered:

- Components
- Component structure
- Creating components
- Component communication
- Modules
- Passing data between components

Basic structure:

```text
Component
├── TypeScript
├── HTML
├── CSS
└── Test file
````

---

### Day 3 — Lifecycle Hooks

I learned how Angular components work during different stages of their
lifecycle.

Topics covered:

* Component lifecycle
* `ngOnInit`
* `ngOnChanges`
* `ngAfterViewInit`
* `ngOnDestroy`

These hooks help me run code at the appropriate stage of a component's
life.

---

### Day 3 — Data Binding, Directives and Pipes

I learned how Angular connects component data with the HTML template.

#### Data Binding

The main types I practised were:

```text
Interpolation
Property Binding
Event Binding
Two-Way Binding
```

Example:

```html
<h2>{{ username }}</h2>
```

```html
<button (click)="login()">Login</button>
```

```html
<input [(ngModel)]="username">
```

#### Directives

I learned that directives can be used to control or change elements in
an Angular template.

Examples:

```text
*ngIf
*ngFor
ngClass
ngStyle
```

#### Pipes

Pipes help transform data before displaying it.

Examples:

```text
uppercase
lowercase
date
currency
json
```

---

### Day 3 — Services, Singletons and Pipes

I learned how services can keep reusable logic outside components.

Services can be useful for:

* API communication
* Authentication
* Shared data
* Reusable application logic

I also learned about dependency injection and how Angular provides
services to the components that need them.

---

### Day 3 — Forms

Forms are used to collect information from users.

Examples include:

* Login forms
* Registration forms
* Profile forms
* Resume forms

I learned that form data should be handled and validated before it is
submitted.

---

### Day 4 — Login and Profile

I practised the basic flow of a login and profile application.

```text
Login
  ↓
Enter Details
  ↓
Validate
  ↓
Successful Login
  ↓
Profile
```

This helped me understand how authentication-related pages can be
organized in an Angular application.

---

### Day 5 — Route Guards

I learned how route guards can control access to Angular routes.

For example:

```text
User
 ↓
Open Protected Page
 ↓
Authentication Check
 ↓
 ┌──────────────────┐
 │                  │
Logged In        Not Logged In
 │                  │
 ↓                  ↓
Page              Login
```

Route guards are useful when some pages should only be available to
authenticated users.

---

# 📁 Files in This Folder

```text
angular.js/
│
├── Angular_Day2_Components.pdf
├── Angular_Day2_Modules_and_Communication (1).pdf
├── Angular_Day3_1_Lifecycle_Hooks.pdf
├── Angular_Day3_2_Binding_Directives_Pipes.pdf
├── Angular_Day3_3_Services_and_Forms.pdf
├── Angular_Day3_Services_Singletons_Pipes.pdf
├── Angular_Day4_Login_Profile_Example.pdf
├── Angular_Day5_Route_Guards.pdf
├── Day1_of-Angular-intro.pdf
└── README.md
