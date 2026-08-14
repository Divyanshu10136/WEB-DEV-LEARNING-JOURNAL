# Angular Lifecycle Hooks — Learning Notes

**Name:** Divyanshu
**Topic:** Angular Lifecycle Hooks


---

## 📖 What is a Component Lifecycle?

I learned that an Angular component passes through different stages during its lifetime. It is created, initialized, updated, and finally removed from the application.

Angular provides **lifecycle hooks** that let me execute code at particular stages of this process. 

A simple flow is:

```text
Component Created
       ↓
Component Initialized
       ↓
Component Updated
       ↓
Component Destroyed
```

---

# 🔧 What are Lifecycle Hooks?

Lifecycle hooks are methods provided by Angular that are automatically
called at specific points while a component is running. Most of their
names begin with `ng`. 

Examples:

```text
ngOnChanges()
ngOnInit()
ngOnDestroy()
```

I can use these methods to perform work at the appropriate stage instead
of trying to manually determine when a component has reached that stage.

---

# 🎯 Why Lifecycle Hooks Are Useful

Lifecycle hooks are useful when I need to:

* Load initial information
* Respond to changes in input data
* Perform work after the view is ready
* Remove timers or subscriptions
* Clean up resources before a component disappears

For example:

```ts
ngOnInit() {
  // Initial setup
}
```

Angular calls `ngOnInit()` during component initialization. 

---

# 📋 Important Lifecycle Hooks

| Hook                      | What I Understand                               |
| ------------------------- | ----------------------------------------------- |
| `ngOnChanges()`           | Responds when an input value changes            |
| `ngOnInit()`              | Performs initial component setup                |
| `ngDoCheck()`             | Runs during change detection                    |
| `ngAfterContentInit()`    | Runs after projected content is initialized     |
| `ngAfterContentChecked()` | Runs after projected content is checked         |
| `ngAfterViewInit()`       | Runs after the component view is initialized    |
| `ngAfterViewChecked()`    | Runs after the view is checked                  |
| `ngOnDestroy()`           | Performs work before the component is destroyed |

These are the lifecycle hooks covered in my notes. 

For quick revision, I am focusing first on:

```text
ngOnChanges()
ngOnInit()
ngAfterViewInit()
ngOnDestroy()
```

---

# 1️⃣ `ngOnChanges()`

`ngOnChanges()` is useful when a component receives data through
`@Input()` and that input changes. 

Example:

```ts
import { Component, Input, OnChanges } from '@angular/core';

@Component({
  selector: 'app-user',
  template: '<h2>{{ name }}</h2>'
})
export class UserComponent implements OnChanges {

  @Input() name = '';

  ngOnChanges() {
    console.log('Name changed:', this.name);
  }

}
```

### Flow

```text
Parent Component
       ↓
@Input value changes
       ↓
Child Component
       ↓
ngOnChanges()
```

### Easy Reminder

```text
ngOnChanges() → Input data changed
```



---

# 2️⃣ `ngOnInit()`

`ngOnInit()` is one of the most commonly used lifecycle hooks. I use it
for work that needs to happen when a component is initialized. 

It can be useful for:

* Setting initial values
* Loading starting data
* Making initial API calls
* Preparing component logic

Example:

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-user',
  template: '<h2>{{ name }}</h2>'
})
export class UserComponent implements OnInit {

  name = '';

  ngOnInit() {
    this.name = 'Divyanshu';
  }

}
```

### Flow

```text
Component Created
       ↓
Angular Initializes Component
       ↓
ngOnInit()
       ↓
Component Ready
```

`ngOnInit()` runs once after Angular initializes the component's
inputs. 

### Easy Reminder

```text
ngOnInit() → Initial setup
```

---

# 3️⃣ `ngDoCheck()`

`ngDoCheck()` is associated with Angular's change-detection process.

Example:

```ts
ngDoCheck() {
  console.log('Change detection running');
}
```

This hook can execute frequently, so I should avoid putting expensive
operations inside it. 

```text
ngDoCheck() → Change detection
```

---

# 4️⃣ `ngAfterContentInit()`

Angular supports **content projection**, where content can be passed
into a component.

`ngAfterContentInit()` runs after that projected content has been
initialized. 

A simple way to visualize it:

```text
Parent
  ↓
Content Projection
  ↓
Child Component
  ↓
ngAfterContentInit()
```

This hook is mainly useful when working with projected content.

---

# 5️⃣ `ngAfterContentChecked()`

`ngAfterContentChecked()` runs after Angular checks the projected
content. It can execute multiple times during change detection, so
heavy operations should be avoided inside it. 

```text
ngAfterContentChecked()
        ↓
Projected content checked
```

---

# 6️⃣ `ngAfterViewInit()`

`ngAfterViewInit()` runs after the component's view and its child views
have been initialized. 

Example:

```ts
ngAfterViewInit() {
  console.log('View initialized');
}
```

### Flow

```text
Component
    ↓
Template Initialized
    ↓
Child Views Initialized
    ↓
ngAfterViewInit()
```

### Easy Reminder

```text
ngAfterViewInit() → View is ready
```

---

# 7️⃣ `ngAfterViewChecked()`

`ngAfterViewChecked()` is called after Angular checks the component's
view.

Example:

```ts
ngAfterViewChecked() {
  console.log('View checked');
}
```

Because it can run frequently, I should avoid expensive work inside
this hook. 

---

# 8️⃣ `ngOnDestroy()`

`ngOnDestroy()` runs just before Angular removes a component.

I mainly use it for **cleanup work**. 

For example, if I create a timer:

```ts
timer = setInterval(() => {
  console.log('Running...');
}, 1000);
```

I can stop it when the component is destroyed:

```ts
ngOnDestroy() {
  clearInterval(this.timer);
}
```

It can be used to clean up:

* Timers
* Subscriptions
* Event listeners
* Other resources

### Flow

```text
Component Running
       ↓
User leaves component
       ↓
Angular destroys component
       ↓
ngOnDestroy()
       ↓
Cleanup
```

### Easy Reminder

```text
ngOnDestroy() → Cleanup
```

---

# 🔄 Complete Lifecycle

A simplified lifecycle can be represented as:

```text
Component Created
       ↓
ngOnChanges()
       ↓
ngOnInit()
       ↓
ngDoCheck()
       ↓
View Setup
       ↓
ngAfterContentInit()
       ↓
ngAfterViewInit()
       ↓
Component Running
       ↓
Change Detection
       ↓
Component Destroyed
       ↓
ngOnDestroy()
       ↓
Cleanup
```

The actual sequence can become more detailed when inputs, projected
content, and child views are involved. 

---

# 💻 Example Using Lifecycle Hooks

Here is a simple component that uses initialization and destruction:

```ts
import {
  Component,
  OnInit,
  OnDestroy
} from '@angular/core';

@Component({
  selector: 'app-user',
  template: '<h2>User Component</h2>'
})
export class UserComponent implements OnInit, OnDestroy {

  timer: any;

  ngOnInit() {

    console.log('Component initialized');

    this.timer = setInterval(() => {
      console.log('Running...');
    }, 1000);

  }

  ngOnDestroy() {

    clearInterval(this.timer);

    console.log('Component destroyed');

  }

}
```

The basic idea is:

```text
ngOnInit()
    ↓
Start timer

ngOnDestroy()
    ↓
Stop timer
```

This shows how lifecycle hooks can be used for both setup and cleanup.


---

# 🧩 Lifecycle Interfaces

Angular also provides TypeScript interfaces for lifecycle hooks.

For example:

```ts
import { Component, OnInit } from '@angular/core';

export class UserComponent implements OnInit {

  ngOnInit() {

  }

}
```

For destruction:

```ts
import { Component, OnDestroy } from '@angular/core';

export class UserComponent implements OnDestroy {

  ngOnDestroy() {

  }

}
```

Using the interface makes it clear which lifecycle method the class is
implementing. 

---

# ⭐ Four Hooks I Should Remember First

For quick revision:

```text
ngOnChanges()
       ↓
Input value changed

ngOnInit()
       ↓
Component initialized

ngAfterViewInit()
       ↓
View initialized

ngOnDestroy()
       ↓
Component destroyed / cleanup
```

### Memory Trick

```text
CHANGES → INIT → VIEW → DESTROY
```

```text
ngOnChanges()
ngOnInit()
ngAfterViewInit()
ngOnDestroy()
```



---

# ⚠️ Common Mistakes

## 1. Putting Heavy Work in Frequently Called Hooks

Hooks such as:

```text
ngDoCheck()
ngAfterViewChecked()
ngAfterContentChecked()
```

can run frequently.

I should therefore avoid expensive operations inside them. 

---

## 2. Forgetting Cleanup

If I create resources such as:

* Timers
* Subscriptions
* Event listeners

I should also think about removing them when the component is
destroyed.

```ts
ngOnDestroy() {
  // Cleanup
}
```



---

## 3. Using `ngOnInit()` for Every Situation

`ngOnInit()` is mainly meant for initialization.

If the requirement is to react to changes in an `@Input()` value,
`ngOnChanges()` may be more appropriate. 

---

# 🛒 Real-World Example

Suppose I have a product page:

```text
ProductComponent
      │
      ├── ngOnInit()
      │      ↓
      │   Load product
      │
      ├── ngOnChanges()
      │      ↓
      │   Product ID changed
      │
      ├── ngAfterViewInit()
      │      ↓
      │   View is ready
      │
      └── ngOnDestroy()
             ↓
           Cleanup
```

This makes it easier to decide which lifecycle hook should handle a
particular task. 

---

# 🧠 My Understanding

From this topic, I learned that an Angular component does not simply
appear and disappear. It passes through different stages, and Angular
provides hooks for working with those stages.

The most useful hooks for me to remember are:

* `ngOnChanges()` — respond to input changes
* `ngOnInit()` — perform initial setup
* `ngAfterViewInit()` — work with the initialized view
* `ngOnDestroy()` — perform cleanup

I also learned that hooks that run frequently should not contain
expensive operations, and resources such as timers and subscriptions
should be cleaned up when a component is destroyed. 

---

# 📌 Quick Revision

```text
ngOnChanges()
→ Input value changed

ngOnInit()
→ Initial component setup

ngDoCheck()
→ Change detection

ngAfterContentInit()
→ Projected content initialized

ngAfterViewInit()
→ Component view initialized

ngOnDestroy()
→ Component destroyed and cleanup
```



---

# ✅ Topics Covered

* [x] Component Lifecycle
* [x] Lifecycle Hooks
* [x] `ngOnChanges()`
* [x] `ngOnInit()`
* [x] `ngDoCheck()`
* [x] `ngAfterContentInit()`
* [x] `ngAfterContentChecked()`
* [x] `ngAfterViewInit()`
* [x] `ngAfterViewChecked()`
* [x] `ngOnDestroy()`
* [x] Lifecycle Interfaces
* [x] Component Cleanup
* [x] Common Lifecycle Mistakes
* [x] Real-World Lifecycle Example

---


