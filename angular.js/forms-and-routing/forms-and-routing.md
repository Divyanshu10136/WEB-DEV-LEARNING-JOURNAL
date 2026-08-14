# Angular Components — My Learning Notes

**Name:** Divyanshu
**Subject:** Angular
**Topic:** Components
---

## 📖 Introduction

While learning Angular, I understood that **components are the main building blocks of an Angular application**.

Instead of developing the complete interface as one large piece of code, Angular allows me to split it into smaller sections. Each section can have its own logic, HTML, and styling.

For example:

```text
Angular Application
│
├── Header
├── Navigation
├── Home
├── Products
├── Login
└── Footer
```

Each of these can be developed as a separate component.

---

## ⭐ Why I Use Components

Components make Angular projects easier to work with because they:

* Break large interfaces into smaller sections
* Allow code to be reused
* Make maintenance simpler
* Make individual parts easier to test
* Help teams work on different parts of an application

For example, I can create one navigation component and use it across
multiple pages instead of writing the navigation code again and again.

---

# 🧱 Basic Component Structure

An Angular component generally brings together three main files:

```text
Component
│
├── TypeScript → Application logic
├── HTML       → User interface
└── CSS        → Appearance
```

### TypeScript

The TypeScript file contains variables, functions, and other logic
required by the component.

### HTML

The HTML template defines the elements that appear on the screen.

### CSS

CSS controls how those elements look.

---

# 🛠️ Creating a Component

I can use Angular CLI to generate a component.

```bash
ng generate component header
```

A shorter command is:

```bash
ng g c header
```

Angular then creates the required files.

A typical component may look like:

```text
header/
│
├── header.component.ts
├── header.component.html
├── header.component.css
└── header.component.spec.ts
```

---

# 📄 Component TypeScript File

The `.ts` file contains the class and logic for the component.

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

Here, `HeaderComponent` is the component class.

The variable:

```ts
title = 'My Website';
```

stores information that can later be displayed in the HTML template.

---

# 🏷️ `@Component` Decorator

Angular uses the `@Component` decorator to identify a class as a
component and provide its configuration.

Example:

```ts
@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrl: './header.component.css'
})
```

Some commonly used properties are:

* `selector`
* `template`
* `templateUrl`
* `styles`
* `styleUrl`

---

## Selector

The selector determines the HTML element used to display the component.

For example:

```ts
selector: 'app-header'
```

I can then write:

```html
<app-header></app-header>
```

---

## Inline Template

Instead of creating a separate HTML file, I can place a small template
directly inside the component.

```ts
@Component({
  selector: 'app-header',
  template: '<h1>Hello Angular</h1>'
})
```

---

## External Template

For larger templates, I can keep the HTML in a separate file:

```ts
templateUrl: './header.component.html'
```

This keeps the TypeScript file cleaner.

---

## Component Styles

A component can have its own CSS file:

```ts
styleUrl: './header.component.css'
```

This allows me to keep the component's styling organized.

---

# 🖥️ Component Template

The HTML associated with a component is called its **template**.

Example:

```html
<h1>{{ title }}</h1>

<p>Welcome to my website.</p>
```

If the TypeScript class contains:

```ts
title = 'My Website';
```

Angular can display that value in the template.

---

# 🔤 Interpolation

Interpolation lets me display values from the component inside HTML.

The syntax is:

```text
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

Angular replaces `{{ name }}` with the value stored in the component.

---

# 🔗 Property Binding

Property binding connects a component property with a property of an
HTML element.

Example:

```ts
imageUrl = 'profile.jpg';
```

HTML:

```html
<img [src]="imageUrl">
```

The square brackets indicate property binding:

```text
[ ]
```

---

# 🖱️ Event Binding

Event binding is used when I want an action performed in the HTML to
call a method in the component.

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

The parentheses represent event binding:

```text
( )
```

### Event Flow

```text
User clicks button
       ↓
   Click event
       ↓
showMessage()
       ↓
TypeScript executes
```

---

# 🔄 Two-Way Data Binding

Two-way binding keeps the value in the component and the value in an
input field synchronized.

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

When the user changes the input, the component variable changes too.

```text
Input Field
     ↕
Component Variable
```

---

# 🏷️ Using a Component Selector

Suppose the component has:

```ts
selector: 'app-user'
```

It can be placed inside another template using:

```html
<app-user></app-user>
```

This makes it possible to build a complete application by combining
multiple components.

```text
AppComponent
│
├── HeaderComponent
├── UserComponent
└── FooterComponent
```

---

# 👨‍👦 Parent and Child Components

Components can be connected in a parent-child structure.

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

Here, `AppComponent` is the parent.

---

# 📥 `@Input` — Parent to Child

`@Input` allows the parent to provide data to a child component.

### Parent TypeScript

```ts
username = 'Divyanshu';
```

### Parent HTML

```html
<app-user [name]="username"></app-user>
```

### Child

```ts
@Input() name = '';
```

The direction is:

```text
Parent
  │
  │ @Input
  ↓
Child
```

So I can remember:

```text
@Input = Parent → Child
```

---

# 📤 `@Output` — Child to Parent

`@Output` works in the opposite direction. It allows a child to send
an event to its parent.

Example:

```ts
@Output() selected = new EventEmitter<string>();

selectUser() {
  this.selected.emit('Divyanshu');
}
```

The parent can listen for the event:

```html
<app-user
  (selected)="handleUser($event)">
</app-user>
```

The communication is:

```text
Child
  │
  │ @Output
  ↓
Parent
```

### Easy Revision

```text
@Input   → Parent → Child
@Output  → Child → Parent
```

---

# 🔄 Component Communication

A simple example of component communication is:

```text
              PARENT
             /      \
            ↓        ↓
        CHILD 1    CHILD 2
```

For direct parent-child communication:

```text
Parent → Child
   @Input

Child → Parent
   @Output
```

When components are not directly connected, shared services or state
management can be used to share information.

---

# ♻️ Component Lifecycle

An Angular component goes through different stages from creation to
removal.

A simple representation is:

```text
Component Created
       ↓
Initialization
       ↓
Change Detection
       ↓
View Updates
       ↓
Component Destroyed
```

Some lifecycle hooks I learned are:

```text
ngOnInit()
ngOnChanges()
ngAfterViewInit()
ngOnDestroy()
```

For example:

```ts
ngOnInit() {
  console.log('Component is ready');
}
```

Lifecycle hooks allow me to run code at specific stages of the
component's lifetime.

---

# 🧩 Standalone Components

Modern Angular supports **standalone components**.

A standalone component can import the dependencies it requires directly,
without needing an `NgModule` for that purpose.

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

Standalone components are an important approach in modern Angular
projects.

---

# 🌐 Component vs Normal HTML Page

A component is not just an HTML document.

It combines several parts:

```text
TypeScript
    +
HTML
    +
CSS
    +
Angular Metadata
    ↓
Complete Component
```

Because of this, a component can contain both the appearance and
behaviour of a particular part of the application.

---

# 🛒 Example: E-Commerce Website

If I create an online shopping website, I could divide it into:

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

A product card might contain:

```text
Product Image
Product Name
Price
Add to Cart Button
```

The same `ProductCardComponent` can be reused:

```text
Product 1 → ProductCardComponent
Product 2 → ProductCardComponent
Product 3 → ProductCardComponent
Product 4 → ProductCardComponent
```

This saves development time and avoids repeating the same UI code.

---

# 📁 Component Files

| File                 | What I Use It For        |
| -------------------- | ------------------------ |
| `.component.ts`      | Component logic and data |
| `.component.html`    | User interface/template  |
| `.component.css`     | Component design         |
| `.component.spec.ts` | Component testing        |

---

# 🔁 Complete Component Working

I understand the overall concept like this:

```text
                 COMPONENT
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
     TypeScript     HTML       CSS
       Logic      Template    Styling
          │          │
          └────┬─────┘
               ↓
       Angular Processes Data
               ↓
          User Interface
```

When the user interacts with the page:

```text
User
 ↓
HTML Event
 ↓
Component Function
 ↓
Application Logic
 ↓
Data Updated
 ↓
Angular Refreshes UI
```

---

# 🧠 What I Learned

From this topic, I understood that **components are the foundation of
Angular applications**.

I learned how to create components using Angular CLI and how the
TypeScript, HTML, and CSS files work together.

I also practised interpolation, property binding, event binding, and
two-way binding. Another important concept was component communication
using `@Input` and `@Output`.

The lifecycle hooks helped me understand that an Angular component goes
through different stages during its existence.

Overall, component-based development makes an application more
organized, reusable, and easier to maintain.

---

# ✅ Topics Covered

* [x] Angular Components
* [x] Component Structure
* [x] Angular CLI
* [x] Component Class
* [x] `@Component` Decorator
* [x] Selectors
* [x] Templates
* [x] Interpolation
* [x] Property Binding
* [x] Event Binding
* [x] Two-Way Binding
* [x] Parent and Child Components
* [x] `@Input`
* [x] `@Output`
* [x] Component Communication
* [x] Lifecycle Hooks
* [x] Standalone Components
* [x] Reusable Components
* [x] Real-World Component Structure

---