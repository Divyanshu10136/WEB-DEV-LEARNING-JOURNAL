# Angular Services, Singletons & Pipes

**Author:** Divyanshu
**Topic:** Services, Dependency Injection, Singleton Services and Pipes

---

## 1. Angular Services

An Angular **service** is a TypeScript class where I can keep logic or data that needs to be reused by different components.

Instead of putting the same logic inside multiple components, I can move it into a service and access it wherever required.

```text
Component
    ↓
Service
    ↓
Shared Logic / Data / API
```

### Common Uses

Services can be useful for:

* API communication
* User information
* Authentication-related logic
* Shared application data
* Business logic
* Reusable functionality

The basic idea is:

> **Components handle the view, while services handle reusable work.**

The class notes describe a service as a class for sharing logic or data between components. 

---

# 2. Creating a Service

Angular CLI can generate a service for me.

```bash
ng generate service logo
```

Short version:

```bash
ng g s logo
```

A simple service can look like this:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LogoService {

  companyName = 'ResumeFlow';

  getCompanyName() {
    return this.companyName;
  }

  setCompanyName(name: string) {
    this.companyName = name;
  }
}
```

`@Injectable()` tells Angular that the class can participate in dependency injection, while `providedIn: 'root'` makes it available throughout the application. 

---

# 3. Dependency Injection

**Dependency Injection (DI)** is the mechanism Angular uses to provide a service to a component instead of the component creating that service itself.

I don't need to do this:

```ts
const service = new LogoService();
```

Instead, I can request the service through the constructor:

```ts
constructor(public logoService: LogoService) {}
```

Angular sees the required dependency and provides it automatically. 

The basic flow is:

```text
Component
    ↓
Requests Service
    ↓
Angular DI
    ↓
Service Provided
```

---

# 4. Using a Service in a Component

For example:

```ts
import { Component } from '@angular/core';
import { LogoService } from './logo.service';

@Component({
  selector: 'app-header',
  template: `
    <h1>{{ logoService.getCompanyName() }}</h1>

    <button (click)="logoService.setCompanyName('Divyanshu')">
      Change Name
    </button>
  `
})
export class HeaderComponent {

  constructor(public logoService: LogoService) {}

}
```

The constructor tells Angular:

```text
"I need LogoService."
```

Angular then supplies the service.

I don't manually create a new instance of the service. 

---

# 5. Singleton Service

When a service is configured with:

```ts
providedIn: 'root'
```

Angular normally creates one shared instance for the application.

This is called a **singleton**.

```text
             LogoService
                  ↑
        ┌─────────┼─────────┐
        │         │         │
     Header     Footer    Profile
```

All of these components can work with the same service instance.

If one component changes a value in the service, another component using that same instance can see the updated value. 

---

# 6. Understanding Singleton with an Example

A simple real-world example is a **shared shopping cart**.

Imagine adding products while moving through different sections of an online store:

```text
Product Page
     ↓
Add Shirt
     ↓
Shared Cart
     ↑
Add Shoes
     ↑
Add Bag
```

There is one cart containing all the selected products.

A singleton service works in a similar way:

```text
Component A ──┐
Component B ──┼──→ Shared Service
Component C ──┘
```

The class notes use examples such as a shared TV remote, washing machine and shopping cart to explain the idea of one shared instance. 

---

# 7. Sharing Data Between Components

A major benefit of a singleton service is that components that aren't directly related can still share information.

For example:

```text
Header Component
       ↓
   LogoService
       ↑
       ↓
Footer Component
```

The header can update the company name:

```ts
this.logoService.setCompanyName('Divyanshu');
```

The footer can read the updated value:

```html
<p>{{ logoService.getCompanyName() }}</p>
```

This works because both components are using the same service instance. 

---

# 8. Important Point About Shared Values

If I copy a service value into a component property only once, the component may continue showing the old value.

For example, reading the value once during initialization is different from directly reading the current service value in the template.

A better approach for this example is:

```html
<p>{{ logoService.getCompanyName() }}</p>
```

This keeps the template connected to the service's current value. 

---

# 9. Why Learn Angular 13?

The session also explains why Angular 13 can still be useful for learning even though newer Angular versions exist.

The important reasons are:

* Core Angular concepts remain transferable.
* Many existing applications use older Angular versions.
* Angular 13 has extensive documentation and learning resources.
* Starting with the fundamentals can make newer features easier to understand later.

The key point from the notes is that learning the fundamentals first gives me a strong base that can be carried into newer Angular versions. 

---

# 10. What is a Pipe?

An Angular **pipe** changes the way data is displayed in a template without modifying the original value.

The syntax is:

```html
{{ value | pipe }}
```

For example:

```html
<p>{{ name | uppercase }}</p>
```

If the value is:

```text
resumeflow
```

the displayed result becomes:

```text
RESUMEFLOW
```

Pipes are mainly used for presentation and formatting. 

---

# 11. Common Built-in Pipes

Angular provides several ready-made pipes.

Some useful examples are:

```text
uppercase
lowercase
titlecase
date
currency
number
percent
json
slice
```

Examples:

```html
<p>{{ name | uppercase }}</p>

<p>{{ price | currency:'INR' }}</p>

<p>{{ today | date:'longDate' }}</p>
```

The session also demonstrates chaining multiple pipes:

```html
{{ name | slice:0:5 | uppercase }}
```



---

# 12. Custom Pipes

Sometimes the built-in pipes don't perform the transformation I need.

In that situation, I can create a custom pipe.

Using Angular CLI:

```bash
ng generate pipe double
```

Short form:

```bash
ng g p double
```

A custom pipe uses:

* `@Pipe`
* `PipeTransform`
* `transform()`

Example:

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

I can then use it like this:

```html
<p>{{ 10 | double }}</p>
```

Output:

```text
20
```

The `transform()` method receives the input value and returns the transformed result. 

---

# 13. Passing Arguments to a Pipe

A pipe can also receive an additional value.

The argument is written after a colon:

```html
{{ value | pipe:argument }}
```

For example:

```ts
@Pipe({
  name: 'multiply'
})
export class MultiplyPipe implements PipeTransform {

  transform(value: number, times: number): number {
    return value * times;
  }
}
```

Usage:

```html
<p>{{ 10 | multiply:3 }}</p>
```

Result:

```text
30
```



---

# 14. Service vs Pipe

| Service                      | Pipe                             |          |
| ---------------------------- | -------------------------------- | -------- |
| Contains reusable logic/data | Changes displayed values         |          |
| Mainly used from TypeScript  | Mainly used in templates         |          |
| Can share information        | Formats presentation             |          |
| Can handle API-related logic | Performs display transformations |          |
| Uses Angular DI              | Uses `                           | ` syntax |

Simple difference:

```text
Service
   ↓
Shared Logic / Data

Pipe
   ↓
Display Formatting
```

---

# 15. Component + Service + Pipe

These concepts can work together in a real Angular application.

```text
             Component
                 │
        ┌────────┴────────┐
        ↓                 ↓
     Service            Template
        ↓                 ↓
   API / Data            Pipe
                          ↓
                    Formatted UI
```

For example, in ResumeFlow:

```text
Resume Component
       ↓
Resume Service
       ↓
Backend API
       ↓
Resume Data
       ↓
Angular Template
       ↓
Date / Currency / Text Pipe
       ↓
User Interface
```

This keeps responsibilities separated.

---

# 16. Practical ResumeFlow Example

Suppose ResumeFlow has a company name displayed in multiple components.

I can create:

```text
LogoService
     ↓
Header
     ↓
Footer
     ↓
Profile
```

The service stores the shared value:

```ts
companyName = 'ResumeFlow';
```

The header can modify it:

```ts
this.logoService.setCompanyName('Divyanshu');
```

The footer can display the current value:

```html
<footer>
  {{ logoService.getCompanyName() }}
</footer>
```

Because the service is shared, both components work with the same data.

---

# 17. Quick Revision

```text
SERVICE
   ↓
Reusable logic and shared data

DEPENDENCY INJECTION
   ↓
Angular supplies required services

providedIn: 'root'
   ↓
Application-level service provider

SINGLETON
   ↓
One shared service instance

PIPE
   ↓
Formats/transforms template values

{{ value | pipe }}
   ↓
Pipe syntax
```

---

