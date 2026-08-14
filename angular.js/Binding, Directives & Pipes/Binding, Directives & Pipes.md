# Angular Services, Dependency Injection & Pipes — Learning Notes

**Name:** Divyanshu  
**Topic:** Angular Services, Dependency Injection & Pipes  


---

## 📖 What is an Angular Service?

An Angular **service** is a TypeScript class where I can keep logic or
data that needs to be reused by different parts of an application.

Instead of placing all the functionality inside components, I can move
common operations into a service.

A simple flow is:

```text
Component
    ↓
Service
    ↓
API / Database / Shared Data
````

Services are commonly useful for:

* API communication
* Authentication
* User-related data
* Business rules
* Shared information
* Application state
* Reusable operations

---

# 🤔 Why Use Services?

Imagine an application containing:

```text
UserComponent
ProductComponent
OrderComponent
```

If each component needs the same API logic, copying the same code into
all three components would make the project difficult to maintain.

Instead, I can centralize that logic:

```text
             UserComponent
                  │
             ProductComponent
                  │
             OrderComponent
                  │
                  ↓
             UserService
                  │
                  ↓
              API Server
```

This gives me:

* Less duplicate code
* Better organization
* Easier maintenance
* Better reusability
* Easier testing

---

# 🛠️ Creating a Service

Angular CLI can generate a service automatically.

```bash
ng generate service user
```

Short version:

```bash
ng g s user
```

The generated files can include:

```text
user.service.ts
user.service.spec.ts
```

The `.ts` file contains the service logic, while the `.spec.ts` file is
used for testing.

---

# 🧱 Basic Service Example

A simple service can look like this:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  getUserName() {
    return 'Divyanshu';
  }

}
```

The `@Injectable()` decorator tells Angular that this class can
participate in the Dependency Injection system.

---

# 💉 What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern in which a class
receives the objects it requires instead of creating those objects by
itself.

For example, without DI I could create a service manually:

```ts
const userService = new UserService();
```

With Angular DI, Angular manages the dependency for me.

```text
Component
    ↓
"I need UserService"
    ↓
Angular Dependency Injection
    ↓
UserService
```

This reduces direct dependency creation and makes application code
easier to manage.

---

# 🔌 Injecting a Service into a Component

Suppose I have this service:

```ts
@Injectable({
  providedIn: 'root'
})
export class UserService {

  getUserName() {
    return 'Divyanshu';
  }

}
```

I can inject it into a component through the constructor:

```ts
import { Component } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user',
  template: '<h2>{{ name }}</h2>'
})
export class UserComponent {

  name = '';

  constructor(private userService: UserService) {
    this.name = this.userService.getUserName();
  }

}
```

The important part is:

```ts
constructor(private userService: UserService) {}
```

Angular recognizes that the component requires `UserService` and
provides the dependency.

---

# ✨ Using `inject()`

Modern Angular also provides the `inject()` function as another way to
obtain a dependency.

Example:

```ts
import { Component, inject } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user',
  template: '<h2>{{ name }}</h2>'
})
export class UserComponent {

  private userService = inject(UserService);

  name = this.userService.getUserName();

}
```

So there are two common styles:

```text
Constructor Injection
        OR
inject()
```

The main idea remains the same:

```text
Component
    ↓
Requests Dependency
    ↓
Angular DI
    ↓
Dependency Provided
```

---

# 🌍 Understanding `providedIn: 'root'`

A service often contains:

```ts
@Injectable({
  providedIn: 'root'
})
```

This registers the service with the application's root injector.

Example:

```ts
@Injectable({
  providedIn: 'root'
})
export class UserService {

}
```

This makes the service available for injection throughout the
application.

It also allows Angular to manage the service efficiently based on its
usage.

---

# 🔁 Root-Level Service Instance

A service provided at the root level is generally shared as one service
instance within the application.

For example:

```text
UserComponent ───┐
                 │
ProductComponent ├──→ UserService
                 │
OrderComponent ──┘
```

This is useful when multiple components need access to common data or
application-wide functionality.

---

# 🔄 Sharing Data Between Components

Services can also act as a common place for data used by components that
do not have a direct parent-child relationship.

```text
Component A
     │
     ↓
Shared Service
     ↑
     │
Component B
```

For example:

```ts
export class UserService {

  username = 'Divyanshu';

}
```

Different components can access the same service according to the
service's injection scope.

---

# 🌐 Services and API Calls

A major use of Angular services is communicating with backend APIs.

The architecture can look like:

```text
Component
    ↓
UserService
    ↓
HttpClient
    ↓
Backend API
    ↓
Database
```

Instead of putting HTTP logic directly into every component, I can keep
it inside a service.

Example:

```ts
getUsers() {
  return this.http.get('/api/users');
}
```

The component can then use:

```ts
this.userService.getUsers();
```

This keeps components focused more on UI-related work.

---

# 🎨 What is a Pipe?

An Angular **pipe** is used to transform data before displaying it in a
template.

The basic syntax is:

```text
value | pipe
```

For example:

```html
{{ name | uppercase }}
```

If:

```ts
name = 'divyanshu';
```

the displayed result becomes:

```text
DIVYANSHU
```

The original variable does not need to be changed just to format its
display.

---

# 💡 Why Use Pipes?

Pipes are helpful when I want to change the presentation of data.

For example:

```text
Original Value
      ↓
     Pipe
      ↓
Formatted Display
```

Common uses include:

* Changing text case
* Formatting dates
* Displaying currency
* Formatting numbers
* Showing percentages

---

# 🧰 Common Built-in Pipes

Angular provides several ready-to-use pipes.

Some commonly used examples are:

```text
uppercase
lowercase
titlecase
date
currency
percent
number
json
```

---

# 🔠 Uppercase Pipe

```html
{{ name | uppercase }}
```

Input:

```text
divyanshu
```

Output:

```text
DIVYANSHU
```

---

# 🔡 Lowercase Pipe

```html
{{ name | lowercase }}
```

Input:

```text
DIVYANSHU
```

Output:

```text
divyanshu
```

---

# 📝 Titlecase Pipe

The `titlecase` pipe changes text into title-style capitalization.

```html
{{ name | titlecase }}
```

For example:

```text
hello angular
```

can become:

```text
Hello Angular
```

---

# 📅 Date Pipe

The `date` pipe can format date values.

Basic example:

```html
{{ today | date }}
```

A specific format can also be supplied:

```html
{{ today | date:'dd/MM/yyyy' }}
```

For example, a date may be displayed as:

```text
13/08/2026
```

---

# 💰 Currency Pipe

The `currency` pipe is useful for displaying monetary values.

Example:

```html
{{ price | currency }}
```

For Indian currency:

```html
{{ price | currency:'INR' }}
```

If:

```ts
price = 5000;
```

the pipe formats the number as a currency value.

---

# 📊 Percent Pipe

The `percent` pipe converts a decimal value into a percentage display.

Example:

```html
{{ progress | percent }}
```

If:

```ts
progress = 0.75;
```

the displayed result is:

```text
75%
```

---

# ⛓️ Using Multiple Pipes

Pipes can be combined in one expression.

For example:

```html
{{ name | lowercase | titlecase }}
```

The value moves through the pipes from left to right:

```text
name
 ↓
lowercase
 ↓
titlecase
 ↓
Displayed Result
```

This is called **pipe chaining**.

---

# 🎛️ Passing Parameters to Pipes

Some pipes allow additional values to control the formatting.

For example:

```html
{{ price | currency:'INR' }}
```

Here:

```text
currency
    ↓
Parameter
    ↓
INR
```

Another example:

```html
{{ today | date:'dd/MM/yyyy' }}
```

The format string tells the date pipe how the value should be displayed.

---

# 🧪 Creating a Custom Pipe

If the built-in pipes do not provide the transformation I need, I can
create a custom pipe.

Angular CLI command:

```bash
ng generate pipe double
```

Short form:

```bash
ng g p double
```

Angular can generate files such as:

```text
double.pipe.ts
double.pipe.spec.ts
```

---

# 🔢 Custom Pipe Example

Suppose I want to create a pipe that multiplies a number by two.

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'double'
})
export class DoublePipe implements PipeTransform {

  transform(value: number): number {
    return value * 2;
  }

}
```

I can then use it inside a template:

```html
{{ 10 | double }}
```

Result:

```text
20
```

---

# ⚡ Pure and Impure Pipes

Angular pipes can be categorized as **pure** or **impure**.

By default, a pipe is pure.

A pure pipe is evaluated when Angular detects a relevant change in its
input.

An impure pipe can be evaluated during change-detection cycles.

Example:

```ts
@Pipe({
  name: 'example',
  pure: false
})
```

Because impure pipes may run frequently, I should use them carefully,
especially when the transformation is expensive.

---

# 🆚 Service vs Pipe

Services and pipes have different responsibilities.

| Service                         | Pipe                                    |          |
| ------------------------------- | --------------------------------------- | -------- |
| Contains reusable logic or data | Formats or transforms displayed values  |          |
| Mainly used from TypeScript     | Mainly used inside templates            |          |
| Can communicate with APIs       | Usually handles presentation formatting |          |
| Can provide shared data         | Changes how a value appears             |          |
| Works with Dependency Injection | Uses `                                  | ` syntax |

A simple way to understand the difference:

```text
Service
   ↓
Gets or manages data

Pipe
   ↓
Formats data for presentation
```

---

# 🔗 Component + Service + Pipe

These concepts can work together in the same Angular application.

```text
                Component
                   │
          ┌────────┴────────┐
          ↓                 ↓
       Service             HTML
          ↓                 ↓
      API / Data           Pipe
                            ↓
                       Formatted UI
```

For example:

```text
UserComponent
      ↓
UserService
      ↓
Backend API
      ↓
User Data
      ↓
HTML Template
      ↓
Pipe
      ↓
Formatted Interface
```

---

# 🛒 Real-World Example

Suppose I am building an online shopping application.

### Product Service

The service can request products from the backend:

```ts
getProducts() {
  return this.http.get('/api/products');
}
```

### Product Component

The component can use the service:

```ts
this.productService.getProducts();
```

### Product Template

The template can format product information:

```html
<h2>{{ product.name | titlecase }}</h2>

<p>{{ product.price | currency:'INR' }}</p>
```

The complete flow becomes:

```text
Backend API
     ↓
Product Service
     ↓
Product Component
     ↓
HTML Template
     ↓
Pipe
     ↓
Formatted Product
```

---

# 🧠 Quick Revision

## Service

```text
Reusable logic
       +
Shared data
```

## Dependency Injection

```text
Class needs a dependency
        ↓
Angular DI provides it
```

## `providedIn: 'root'`

```text
Service registered with root injector
```

## Pipe

```text
Transforms data for display
```

## Pipe Syntax

```html
{{ value | pipe }}
```

---

# 🛠️ Useful CLI Commands

### Generate a Service

```bash
ng g s user
```

### Generate a Pipe

```bash
ng g p double
```

---

# 📋 Common Pipes to Remember

```text
uppercase
lowercase
titlecase
date
currency
percent
number
json
```

---

# 🎯 My Learning

From this topic, I learned that services help me keep reusable logic
outside components. This is especially useful when several parts of an
application need the same functionality.

I also understood the basic idea behind Angular's Dependency Injection
system and how Angular can provide services automatically.

Pipes are different from services because their main purpose is to
format or transform values for presentation inside templates.

The overall architecture I understood is:

```text
Component
    ↓
Service
    ↓
API / Data
    ↓
Component Template
    ↓
Pipe
    ↓
Formatted UI
```

---

# ✅ Topics Covered

* [x] Angular Services
* [x] Creating Services with Angular CLI
* [x] `@Injectable()`
* [x] Dependency Injection
* [x] Constructor Injection
* [x] `inject()`
* [x] `providedIn: 'root'`
* [x] Shared Service Data
* [x] Services for API Calls
* [x] Angular Pipes
* [x] Built-in Pipes
* [x] Pipe Parameters
* [x] Pipe Chaining
* [x] Custom Pipes
* [x] Pure and Impure Pipes
* [x] Service vs Pipe
* [x] Component + Service + Pipe Architecture

