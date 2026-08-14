
# Angular Introduction — Learning Notes

**Name:** Divyanshu  
**Topic:** Introduction to Angular  


---

## 📖 What is Angular?

Angular is a **TypeScript-based framework** used to create modern and
structured web applications.

It provides many features that help me develop frontend applications,
such as:

- Components
- Services
- Routing
- Forms
- Dependency Injection
- Directives
- Pipes

Angular is particularly useful when an application becomes large and
needs a clear structure.

---

# 🧩 Angular Architecture

Angular follows a **component-based approach**.

Instead of writing the entire application as one large piece of code, I
can divide the interface into smaller sections.

For example:

```text
Angular Application
│
├── Header
├── Navigation Bar
├── Home
├── Products
├── Login
└── Footer
````

Each part can be developed independently with its own:

* TypeScript logic
* HTML template
* CSS styling

This approach makes the project easier to organize and update.

---

# 🟦 Angular and TypeScript

Angular applications primarily use **TypeScript**.

## What is TypeScript?

TypeScript extends JavaScript by adding features that make code easier
to organize and maintain.

Some important features are:

* Static type checking
* Interfaces
* Classes
* Improved development tools
* Compile-time error detection

Example:

```ts
let username: string = "Divyanshu";
let age: number = 21;
```

Before TypeScript code can run in a browser, it is converted into
JavaScript.

```text
TypeScript
     ↓
Compilation
     ↓
JavaScript
     ↓
Browser
```

---

# 📜 ECMAScript

**ECMAScript** is the standard specification that defines the core
features of JavaScript.

JavaScript follows the ECMAScript standard, and newer versions of the
standard introduce additional language features.

A simple relationship is:

```text
ECMAScript
     ↓
JavaScript
     ↓
TypeScript
     ↓
Angular Application
```

---

# ⚙️ Angular CLI

Angular provides a command-line development tool called **Angular
CLI**.

CLI means:

> Command Line Interface

I can use Angular CLI to create projects, generate components and
services, run applications, build projects, and execute tests.

This saves time because many project files and configurations are
created automatically.

---

# 💻 Installing Angular CLI

Before installing Angular CLI, I can check whether Node.js and npm are
available on my system.

```bash
node -v
```

```bash
npm -v
```

To install Angular CLI globally:

```bash
npm install -g @angular/cli
```

After installation, I can verify it using:

```bash
ng version
```

---

# 🚀 Creating an Angular Application

To start a new Angular project:

```bash
ng new my-first-app
```

Angular CLI may ask me to select some project configuration options.

After the project is created, I move into the project directory:

```bash
cd my-first-app
```

Then I can start the development server:

```bash
ng serve
```

The application can normally be opened at:

```text
http://localhost:4200
```

---

# 🛠️ Useful Angular CLI Commands

| Command                        | Purpose                               |
| ------------------------------ | ------------------------------------- |
| `ng new project-name`          | Creates a new Angular project         |
| `ng serve`                     | Starts the development server         |
| `ng generate component header` | Creates a component                   |
| `ng g c header`                | Short form for generating a component |
| `ng generate service user`     | Creates a service                     |
| `ng g s user`                  | Short form for generating a service   |
| `ng generate pipe custom`      | Creates a custom pipe                 |
| `ng build`                     | Creates a production build            |
| `ng test`                      | Runs project tests                    |
| `ng version`                   | Displays Angular CLI information      |

---

# 📁 Angular Project Structure

When I create a new Angular application, Angular CLI generates a
structured project.

A simplified example is:

```text
my-first-app/
│
├── src/
│   ├── app/
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   └── app.component.css
│   │
│   ├── assets/
│   ├── index.html
│   └── main.ts
│
├── angular.json
├── package.json
├── tsconfig.json
└── README.md
```

## Important Files and Folders

### `src/`

This is where most of the application's source code is kept.

### `app/`

This contains the main application components and related Angular code.

### `main.ts`

This is an important starting point used to bootstrap the Angular
application.

### `index.html`

This acts as the main HTML document loaded by the browser.

### `package.json`

This file contains project information, scripts, and package
dependencies.

### `angular.json`

This file contains configuration used by Angular CLI.

### `tsconfig.json`

This contains the TypeScript compiler configuration for the project.

---

# 🧱 Components

Components are the main building blocks of an Angular application.

A component normally brings together:

```text
Component
│
├── TypeScript → Logic
├── HTML       → Template
└── CSS        → Styling
```

Example:

```ts
export class AppComponent {
  name = "Divyanshu";
}
```

The value can be displayed in the HTML template:

```html
<h1>Hello {{ name }}</h1>
```

Here, the `name` variable comes from the TypeScript class and is shown
inside the HTML.

---

# 🔗 Data Binding

Angular provides different techniques for connecting component data
with the template.

One simple method is **interpolation**:

```html
<h1>{{ name }}</h1>
```

If the component contains:

```ts
name = "Divyanshu";
```

Angular displays the value in the browser.

Angular also supports:

* Interpolation
* Property binding
* Event binding
* Two-way binding

These features allow the component logic and the user interface to
work together.

---

# 🔄 Angular Application Flow

A simplified way to understand the Angular application flow is:

```text
User
 ↓
Browser
 ↓
Angular Application
 ↓
Component
 ↓
Template
 ↓
User Interface
```

For example, when a user clicks a button:

```text
User clicks button
       ↓
Event is detected
       ↓
Component method executes
       ↓
Application data changes
       ↓
Angular updates the UI
```

---

# 📚 Important Angular Concepts

These are the major concepts I need to understand while learning
Angular:

| Concept              | My Understanding                        |
| -------------------- | --------------------------------------- |
| Components           | Used to create reusable UI sections     |
| Templates            | Define the structure of the interface   |
| Data Binding         | Connects application data with the UI   |
| Directives           | Control or modify template behavior     |
| Services             | Hold reusable application logic         |
| Dependency Injection | Provides required services/dependencies |
| Pipes                | Transform data before displaying it     |
| Routing              | Handles navigation between views        |
| Forms                | Manage and collect user input           |
| Lifecycle Hooks      | Run code at different component stages  |

---

# ⚖️ Angular vs JavaScript

JavaScript and Angular are not the same thing.

**JavaScript** is a programming language.

**TypeScript** extends JavaScript with additional development
features.

**Angular** is a framework used to build structured web applications,
mainly using TypeScript.

The relationship can be visualized as:

```text
JavaScript
    ↓
Programming Language

TypeScript
    ↓
JavaScript with additional features

Angular
    ↓
Framework for web application development
```

Angular provides many built-in features that would otherwise require
additional setup or development.

---

# 🧠 My Learning

From the introduction to Angular, I understood that Angular provides a
structured way to develop frontend applications.

I learned why Angular uses components and how breaking an application
into smaller sections makes development easier.

I also learned the basic role of TypeScript, Angular CLI, project
structure, data binding, and important Angular features such as
services, routing, forms, directives, and pipes.

Angular CLI is especially useful because it provides commands for
creating and managing different parts of a project.

---

# 🛣️ My Angular Learning Path

I plan to learn Angular step by step:

```text
Angular Introduction
        ↓
TypeScript Basics
        ↓
Components
        ↓
Templates & Data Binding
        ↓
Directives
        ↓
Services
        ↓
Dependency Injection
        ↓
Pipes
        ↓
Forms
        ↓
Routing
        ↓
HTTP / APIs
        ↓
Authentication
        ↓
Advanced Angular
```

---

# ✅ Topics Covered

* [x] Introduction to Angular
* [x] Angular Architecture
* [x] TypeScript
* [x] ECMAScript
* [x] Angular CLI
* [x] Creating Angular Projects
* [x] Angular Project Structure
* [x] Components
* [x] Data Binding
* [x] Application Flow
* [x] Services
* [x] Routing
* [x] Forms
* [x] Directives
* [x] Pipes
* [x] Dependency Injection

---

