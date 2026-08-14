# Angular Components — Learning Notes

**Name:** Divyanshu
**Topic:** Angular Components


---

## 📌 Introduction

Angular applications are built using **components**. A component is a small, reusable part of an application that handles a specific section of the user interface.

Instead of putting the complete application into one large file, I can divide it into separate components.

For example:

```text
Angular Application
│
├── Header
├── Navbar
├── Home
├── Product
├── Login
└── Footer
```

This makes the application easier to organize and manage.

---

# Why Components Are Useful

Using components provides several advantages:

* Makes code easier to understand
* Makes parts of the UI reusable
* Simplifies maintenance
* Makes testing easier
* Allows different developers to work on separate parts

For example, instead of creating a navbar separately for every page, I can create one `NavbarComponent` and use it wherever required.

---

# Component Structure

A component mainly combines three things:

```text
Component
│
├── TypeScript → Logic and data
├── HTML       → Page structure
└── CSS        → Design and styling
```

### TypeScript

The TypeScript file contains the component's variables, functions,
and application logic.

### HTML

The HTML template controls what the user sees.

### CSS

CSS is responsible for the visual appearance of the component.

---

# Creating a Component

Angular CLI can generate a new component.

```bash
ng generate component header
```

Short version:

```bash
ng g c header
```

A generated component normally contains files such as:

```text
header/
│
├── header.component.ts
├── header.component.html
├── header.component.css
└── header.component.spec.ts
```

---

# Component Class

The TypeScript file contains the class used by the component.

Example:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrl: './header.component.css'
})
export class HeaderComponent {

  title = 'My Website';

}
```

Here:

```ts
export class HeaderComponent
```

defines the component class.

The variable:

```ts
title = 'My Website';
```

contains information that can be displayed by the HTML template.

---

# `@Component` Decorator

The `@Component` decorator provides Angular with information about the
component.

Example:

```ts
@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrl: './header.component.css'
})
```

Some important properties are:

* `selector`
* `template`
* `templateUrl`
* `styles`
* `styleUrl`

---

## Selector

The selector specifies the HTML tag used to place the component.

For example:

```ts
selector: 'app-header'
```

The component can then be used as:

```html
<app-header></app-header>
```

---

## Template

I can write the HTML directly inside the component using `template`.

```ts
@Component({
  selector: 'app-header',
  template: '<h1>Hello Angular</h1>'
})
```

---

## Template URL

Instead of writing HTML inside the TypeScript file, I can keep it in a
separate HTML file:

```ts
templateUrl: './header.component.html'
```

---

## Styles

The component can have its own styling.

For example:

```ts
styleUrl: './header.component.css'
```

---

# Component Template

The HTML file associated with a component is called its **template**.

Example:

```html
<h1>{{ title }}</h1>

<p>Welcome to my website.</p>
```

If the component contains:

```ts
title = 'My Website';
```

Angular can display the value inside the template.

---

# 🔤 Interpolation

Interpolation is used to show component data in HTML.

Syntax:

```html
{{ value }}
```

Example:

```ts
name = 'Divyanshu';
```

HTML:

```html
<h1>Hello {{ name }}</h1>
```

The value of `name` is displayed in the page.

---

# 🔗 Property Binding

Property binding connects a component value with an HTML property.

Example:

```ts
imageUrl = 'profile.jpg';
```

HTML:

```html
<img [src]="imageUrl">
```

The square brackets:

```text
[]
```

indicate property binding.

---

# 🖱️ Event Binding

Event binding allows an action in the HTML template to call a
function from the component.

Example:

```ts
showMessage() {
  console.log('Button clicked');
}
```

HTML:

```html
<button (click)="showMessage()">
  Click Me
</button>
```

The parentheses:

```text
()
```

represent event binding.

### Flow

```text
User Action
     ↓
(click)
     ↓
Component Method
     ↓
TypeScript Logic
```

---

# 🔄 Two-Way Binding

Two-way binding keeps the component value and input field synchronized.

A common syntax is:

```html
[(ngModel)]
```

Example:

```ts
name = '';
```

```html
<input [(ngModel)]="name">

<p>Hello {{ name }}</p>
```

When the user types into the input, the value of `name` changes as
well.

```text
HTML Input
    ↕
Component Data
```

---

# 🏷️ Component Selector

A selector determines how a component is placed inside another
component.

Example:

```ts
selector: 'app-user'
```

Usage:

```html
<app-user></app-user>
```

Components can therefore be combined:

```text
AppComponent
│
├── HeaderComponent
├── UserComponent
└── FooterComponent
```

---

# 👨‍👦 Parent and Child Components

Angular components can have parent-child relationships.

Example:

```text
Parent Component
│
├── Child Component
└── Child Component
```

For example:

```text
AppComponent
│
├── NavbarComponent
└── ProductComponent
```

Here, `AppComponent` acts as the parent.

---

# 📥 Passing Data with `@Input`

`@Input` is used when a parent component needs to send information to a
child component.

### Parent

```ts
username = 'Divyanshu';
```

Parent HTML:

```html
<app-user [name]="username"></app-user>
```

### Child

```ts
@Input() name = '';
```

The communication can be represented as:

```text
Parent
  │
  │ @Input
  ↓
Child
```

---

# 📤 Sending Data with `@Output`

`@Output` allows a child component to notify its parent about an event.

Example:

```ts
@Output() selected = new EventEmitter<string>();

selectUser() {
  this.selected.emit('Divyanshu');
}
```

Parent template:

```html
<app-user
  (selected)="handleUser($event)">
</app-user>
```

The direction is:

```text
Child
  │
  │ @Output
  ↓
Parent
```

### Easy Way to Remember

```text
@Input   → Parent → Child

@Output  → Child → Parent
```

---

# 🔄 Component Communication

A simple representation is:

```text
             PARENT
            /      \
           ↓        ↓
       CHILD 1    CHILD 2
```

For parent-child communication:

```text
Parent → Child
   @Input

Child → Parent
   @Output
```

For components that do not have a direct parent-child relationship,
shared services or state-management techniques can be used.

---

# ♻️ Component Lifecycle

Every Angular component goes through different stages.

A simplified lifecycle looks like:

```text
Component Created
       ↓
Initialization
       ↓
Change Detection
       ↓
View Updates
       ↓
Component Removed
```

Some important lifecycle hooks are:

```text
ngOnInit()
ngOnChanges()
ngAfterViewInit()
ngOnDestroy()
```

Example:

```ts
ngOnInit() {
  console.log('Component initialized');
}
```

Lifecycle hooks allow me to execute specific code at different points
during a component's lifetime.

---

# 🧩 Standalone Components

Modern Angular supports **standalone components**.

A standalone component can directly import the dependencies it needs
without depending on an `NgModule` for that purpose.

Example:

```ts
@Component({
  selector: 'app-user',
  standalone: true,
  imports: [],
  templateUrl: './user.component.html'
})
export class UserComponent {

}
```

Standalone components are commonly used in modern Angular application
development.

---

# Component vs HTML Page

A component is more than just an HTML page.

It combines:

```text
TypeScript
    +
HTML Template
    +
CSS
    +
Angular Metadata
```

This combination gives the component both its **appearance and
behaviour**.

---

# 🛒 Real-World Example — E-Commerce Website

Suppose I am creating an online shopping application.

I can divide the interface into:

```text
E-Commerce Application
│
├── NavbarComponent
├── ProductListComponent
├── ProductCardComponent
├── CartComponent
├── LoginComponent
└── FooterComponent
```

A `ProductCardComponent` could contain:

```text
Product Image
Product Name
Price
Add to Cart Button
```

The same component can then be reused for many products:

```text
Product 1 → ProductCard
Product 2 → ProductCard
Product 3 → ProductCard
Product 4 → ProductCard
```

This is one of the main advantages of component-based development.

---

# 📁 Important Component Files

| File                 | Purpose                    |
| -------------------- | -------------------------- |
| `.component.ts`      | Contains component logic   |
| `.component.html`    | Contains the template      |
| `.component.css`     | Contains component styling |
| `.component.spec.ts` | Contains component tests   |

---

# 🔁 Complete Component Flow

The basic structure can be remembered as:

```text
                 COMPONENT
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
     TypeScript     HTML       CSS
       Logic      Template    Styling
          │          │
          └─────┬────┘
                ↓
        Angular Processes It
                ↓
           User Interface
```

When the user interacts with the interface:

```text
User
 ↓
HTML Event
 ↓
Component Method
 ↓
Application Logic
 ↓
Data Changes
 ↓
Angular Updates UI
```

