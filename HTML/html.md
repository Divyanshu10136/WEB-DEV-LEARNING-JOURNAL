# HTML Notes 🌐

> Beginner-friendly notes for learning **HTML (HyperText Markup Language)**.

HTML is used to create the **structure of a webpage**.

Think of a website like a house:

* 🧱 **HTML** → Structure
* 🎨 **CSS** → Design
* ⚙️ **JavaScript** → Functionality

HTML is **not a programming language**. It is a **markup language** that uses tags to describe webpage content.

---

## 📚 Table of Contents

* [1. Basic HTML Structure](#1-basic-html-structure)
* [2. HTML Tags](#2-html-tags)
* [3. Attributes](#3-attributes)
* [4. Common HTML Tags](#4-common-html-tags)
* [5. Links and Images](#5-links-and-images)
* [6. Lists](#6-lists)
* [7. Div and Span](#7-div-and-span)
* [8. Semantic HTML](#8-semantic-html)
* [9. Forms](#9-forms)
* [10. Tables](#10-tables)
* [11. Accessibility](#11-accessibility)
* [12. Block vs Inline Elements](#12-block-vs-inline-elements)
* [13. HTML Best Practices](#13-html-best-practices)
* [14. Quick Cheat Sheet](#14-quick-cheat-sheet)
* [15. Summary](#15-summary)

---

# 1. Basic HTML Structure

A basic HTML page looks like this:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >

  <title>My Webpage</title>
</head>

<body>

  <!-- Visible content goes here -->

</body>

</html>
```

### What does each part mean?

| Part                     | Purpose                                          |
| ------------------------ | ------------------------------------------------ |
| `<!DOCTYPE html>`        | Tells the browser that this is an HTML5 document |
| `<html>`                 | Root element of the webpage                      |
| `<head>`                 | Contains page information/metadata               |
| `<meta charset="UTF-8">` | Helps display different characters correctly     |
| `<meta name="viewport">` | Makes the page work better on mobile devices     |
| `<title>`                | Title shown in the browser tab                   |
| `<body>`                 | Contains the visible content                     |

---

# 2. HTML Tags

A **tag** tells the browser what a piece of content means.

Example:

```html
<p>Hello World!</p>
```

Here:

* `<p>` → Opening tag
* `Hello World!` → Content
* `</p>` → Closing tag

Together, they form an **HTML element**.

### Another example

```html
<h1>My Website</h1>
```

`<h1>` tells the browser that this is a heading.

---

## Void Elements

Some HTML elements don't have a closing tag.

These are called **void elements**.

```html
<img src="photo.jpg" alt="My photo">

<br>

<hr>

<input type="text">
```

---

# 3. Attributes

**Attributes** provide extra information about an HTML element.

They are written inside the opening tag.

Example:

```html
<a href="https://example.com">
  Visit Website
</a>
```

Here:

* `<a>` → HTML tag
* `href` → Attribute
* `"https://example.com"` → Attribute value

### Image example

```html
<img
  src="photo.jpg"
  alt="A beautiful sunset"
>
```

Here:

* `src` → Image location
* `alt` → Image description

---

# 4. Common HTML Tags

## Headings

HTML provides six levels of headings:

```html
<h1>Main Heading</h1>
<h2>Section Heading</h2>
<h3>Subheading</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

### Heading order

Try to keep your headings logical:

```text
h1
 ├── h2
 │    ├── h3
 │    └── h3
 └── h2
```

Don't choose a heading only because it looks bigger. Use headings according to their **meaning and structure**.

---

## Paragraph

Use `<p>` for normal text:

```html
<p>This is a paragraph.</p>
```

---

## Important Text

Use `<strong>` when the text has strong importance:

```html
<strong>This is important.</strong>
```

---

## Emphasized Text

Use `<em>` for emphasis:

```html
<em>This text is emphasized.</em>
```

---

## Line Break

```html
Hello<br>
World
```

`<br>` moves the next content to a new line.

---

## Horizontal Break

```html
<hr>
```

`<hr>` creates a thematic/horizontal break.

---

# 5. Links and Images

## Links

Use `<a>` to create links.

### Internal Link

```html
<a href="about.html">
  About Us
</a>
```

### External Link

```html
<a
  href="https://example.com"
  target="_blank"
  rel="noopener noreferrer"
>
  Visit Website
</a>
```

### Important Attributes

| Attribute                   | Purpose                              |
| --------------------------- | ------------------------------------ |
| `href`                      | Specifies where the link goes        |
| `target="_blank"`           | Opens the link in a new tab          |
| `rel="noopener noreferrer"` | Good security practice with `_blank` |

---

## Images

Use `<img>` to display an image:

```html
<img
  src="photo.jpg"
  alt="A mountain covered with snow"
  width="300"
  height="200"
>
```

### Important Attributes

| Attribute | Purpose                  |
| --------- | ------------------------ |
| `src`     | Image location           |
| `alt`     | Description of the image |
| `width`   | Image width              |
| `height`  | Image height             |

### Why is `alt` important?

`alt` text helps:

* Screen readers understand the image
* Users when the image cannot load
* Improve accessibility

---

# 6. Lists

HTML provides different types of lists.

## Unordered List

Used when the order doesn't matter.

```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```

Output:

* HTML
* CSS
* JavaScript

---

## Ordered List

Used when the order matters.

```html
<ol>
  <li>Open VS Code</li>
  <li>Create an HTML file</li>
  <li>Write HTML</li>
</ol>
```

Output:

1. Open VS Code
2. Create an HTML file
3. Write HTML

---

## Description List

Used for terms and their descriptions.

```html
<dl>
  <dt>HTML</dt>
  <dd>Used to create webpage structure.</dd>

  <dt>CSS</dt>
  <dd>Used to style webpages.</dd>
</dl>
```

| Tag    | Meaning          |
| ------ | ---------------- |
| `<dl>` | Description list |
| `<dt>` | Term             |
| `<dd>` | Description      |

---

# 7. Div and Span

Both `<div>` and `<span>` are **generic containers**.

## `<div>`

Usually used for grouping larger sections.

```html
<div>
  <h2>About Me</h2>
  <p>I am learning web development.</p>
</div>
```

## `<span>`

Usually used for small pieces of inline content.

```html
<p>
  I am learning <span>HTML</span>.
</p>
```

### Easy Difference

| Element  | Use                      |
| -------- | ------------------------ |
| `<div>`  | Generic block/container  |
| `<span>` | Generic inline container |

---

# 8. Semantic HTML

**Semantic HTML** means using HTML tags that clearly describe the meaning of their content.

For example:

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

This is better than:

```html
<div>
  <a href="/">Home</a>
  <a href="/about">About</a>
</div>
```

when the content is navigation.

---

## Why Use Semantic HTML?

Semantic HTML helps with:

### ♿ Accessibility

Screen readers can better understand the webpage.

### 🔍 SEO

Search engines can better understand the structure and meaning of your content.

### 📖 Readability

Other developers can understand your code more easily.

### 🛠️ Maintainability

Well-structured code is easier to modify later.

---

## Important Semantic Tags

| Tag            | Purpose                          |
| -------------- | -------------------------------- |
| `<header>`     | Top part of a page or section    |
| `<nav>`        | Navigation links                 |
| `<main>`       | Main content of the page         |
| `<section>`    | Group of related content         |
| `<article>`    | Independent content              |
| `<aside>`      | Related/side content             |
| `<footer>`     | Bottom part of a page or section |
| `<figure>`     | Image, diagram, chart, etc.      |
| `<figcaption>` | Caption for a figure             |
| `<time>`       | Represents date/time             |
| `<mark>`       | Highlighted text                 |

---

## Semantic Layout Example

```html
<body>

  <header>

    <h1>My Portfolio</h1>

    <nav>
      <a href="#about">About</a>
      <a href="#projects">Projects</a>
    </nav>

  </header>


  <main>

    <section id="about">

      <h2>About Me</h2>

      <p>
        I am learning web development.
      </p>

    </section>


    <section id="projects">

      <h2>My Projects</h2>

      <article>

        <h3>Portfolio Website</h3>

        <p>
          This is my portfolio project.
        </p>

      </article>

    </section>


    <aside>

      <h3>Quick Links</h3>

      <a href="#">
        GitHub
      </a>

    </aside>

  </main>


  <footer>

    <p>
      &copy; 2026 My Portfolio
    </p>

  </footer>

</body>
```

### Easy way to remember

```text
<header>   → Top
<nav>      → Navigation
<main>     → Main content
<section>  → Content section
<article>  → Independent content
<aside>    → Side/related content
<footer>   → Bottom
```

---

# 9. Forms

Forms are used to **collect information from users**.

Example:

```html
<form action="/submit" method="POST">

  <label for="name">
    Name:
  </label>

  <input
    type="text"
    id="name"
    name="name"
    placeholder="Enter your name"
    required
  >


  <label for="email">
    Email:
  </label>

  <input
    type="email"
    id="email"
    name="email"
    required
  >


  <button type="submit">
    Submit
  </button>

</form>
```

---

## Common Input Types

```html
<input type="text">

<input type="email">

<input type="password">

<input type="number">

<input type="date">

<input type="tel">

<input type="url">

<input type="file">

<input type="checkbox">

<input type="radio">
```

---

## Textarea

For longer text:

```html
<textarea
  rows="5"
  cols="30"
>
</textarea>
```

---

## Dropdown

```html
<select>

  <option>India</option>
  <option>USA</option>
  <option>UK</option>

</select>
```

---

## Button

```html
<button type="submit">
  Submit
</button>
```

---

## Important Form Attributes

### `for` + `id`

The `<label>`'s `for` should match the input's `id`.

```html
<label for="email">
  Email
</label>

<input
  type="email"
  id="email"
>
```

This improves accessibility and allows the user to click the label to focus the input.

---

### `name`

`name` identifies the field when the form is submitted.

```html
<input
  type="text"
  name="username"
>
```

---

### `required`

Makes the field required:

```html
<input
  type="email"
  required
>
```

---

### `placeholder`

Shows a hint inside the input:

```html
<input
  type="text"
  placeholder="Enter your name"
>
```

---

# 10. Tables

Use tables for **data that belongs in rows and columns**.

```html
<table>

  <caption>
    Student Marks
  </caption>

  <thead>
    <tr>
      <th>Name</th>
      <th>Subject</th>
      <th>Marks</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Yashi</td>
      <td>Web Development</td>
      <td>95</td>
    </tr>
  </tbody>

</table>
```

---

## Table Tags

| Tag         | Purpose           |
| ----------- | ----------------- |
| `<table>`   | Creates the table |
| `<caption>` | Table title       |
| `<thead>`   | Header section    |
| `<tbody>`   | Main data         |
| `<tfoot>`   | Footer/summary    |
| `<tr>`      | Table row         |
| `<th>`      | Header cell       |
| `<td>`      | Data cell         |

---

## `colspan`

Used to merge columns:

```html
<td colspan="2">
  Total
</td>
```

This cell covers **2 columns**.

---

## `rowspan`

Used to merge rows:

```html
<td rowspan="2">
  HTML
</td>
```

This cell covers **2 rows**.

> ⚠️ **Important:** Don't use tables for webpage layouts. Use **CSS Flexbox or CSS Grid** for layouts.

---

# 11. Accessibility

Accessibility means making websites usable for **everyone**, including people who use screen readers or keyboards.

## Important Accessibility Rules

### 1. Use meaningful `alt` text

```html
<img
  src="cat.jpg"
  alt="White cat sitting on a chair"
>
```

---

### 2. Use semantic HTML

```html
<nav>
  ...
</nav>
```

is better than using a generic `<div>` when the content is navigation.

---

### 3. Give form inputs labels

```html
<label for="username">
  Username
</label>

<input
  type="text"
  id="username"
>
```

---

### 4. Use `<button>` for actions

```html
<button>
  Delete
</button>
```

Use `<a>` for navigation:

```html
<a href="/about.html">
  About
</a>
```

### Easy rule

> **Going somewhere → `<a>`**
> **Doing something → `<button>`**

---

### 5. Don't depend only on colors

Bad:

```text
Red = Error
Green = Success
```

Better:

```text
❌ Error: Email is required.

✅ Email successfully submitted.
```

---

# 12. Block vs Inline Elements

## Block Elements

Block elements generally start on a new line and take the available width.

Examples:

```text
<div>
<p>
<h1> - <h6>
<section>
<article>
<ul>
<ol>
```

---

## Inline Elements

Inline elements generally stay on the same line and take only the space they need.

Examples:

```text
<span>
<a>
<strong>
<em>
<img>
<label>
```

> 💡 CSS can change how elements behave, so these are their **default behaviors**.

---

# 13. HTML Best Practices

### ✅ 1. Use Semantic HTML

```html
<nav>...</nav>
```

instead of unnecessarily using:

```html
<div>...</div>
```

---

### ✅ 2. Keep Your Code Clean

Use proper indentation:

```html
<section>

  <h2>About Me</h2>

  <p>
    I am learning HTML.
  </p>

</section>
```

---

### ✅ 3. Use Lowercase Tags

Prefer:

```html
<div>
```

instead of:

```html
<DIV>
```

---

### ✅ 4. Use Double Quotes

Prefer:

```html
class="container"
```

---

### ✅ 5. Don't Forget `alt`

```html
<img
  src="profile.jpg"
  alt="Profile photo"
>
```

---

### ✅ 6. Use Headings Properly

Keep a logical structure:

```text
h1
 ├── h2
 │    ├── h3
 │    └── h3
 └── h2
```

---

### ✅ 7. Separate HTML, CSS and JavaScript

A simple project structure:

```text
my-project/
│
├── index.html
├── style.css
└── script.js
```

---

### ✅ 8. Use Comments When Helpful

```html
<!-- Navigation -->
<nav>
  ...
</nav>
```

Use comments for complex sections, not every single line.

---

### ✅ 9. Make Websites Mobile-Friendly

Add the viewport meta tag:

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0"
>
```

---

# 14. Quick Cheat Sheet

| Purpose           | HTML Tag        |
| ----------------- | --------------- |
| Main heading      | `<h1>`          |
| Heading           | `<h2>` - `<h6>` |
| Paragraph         | `<p>`           |
| Link              | `<a>`           |
| Image             | `<img>`         |
| Line break        | `<br>`          |
| Horizontal break  | `<hr>`          |
| Important text    | `<strong>`      |
| Emphasized text   | `<em>`          |
| Bullet list       | `<ul>`          |
| Numbered list     | `<ol>`          |
| List item         | `<li>`          |
| Generic container | `<div>`         |
| Inline container  | `<span>`        |
| Navigation        | `<nav>`         |
| Main content      | `<main>`        |
| Section           | `<section>`     |
| Article           | `<article>`     |
| Side content      | `<aside>`       |
| Footer            | `<footer>`      |
| Form              | `<form>`        |
| Input             | `<input>`       |
| Text area         | `<textarea>`    |
| Dropdown          | `<select>`      |
| Button            | `<button>`      |
| Table             | `<table>`       |

---

# 15. Summary

HTML is used to create the **structure of a webpage**.

### Remember:

* 🧱 **HTML = Structure**
* 🎨 **CSS = Styling**
* ⚙️ **JavaScript = Functionality**
* 🏷️ **Tags** define content.
* ⚙️ **Attributes** provide extra information.
* 🧩 **Semantic HTML** gives meaning to webpage sections.
* 📝 **Forms** collect user input.
* 📊 **Tables** display tabular data.
* ♿ **Accessibility** makes websites easier for everyone to use.
* 📦 Use `<div>` when you need a generic container.
* 🧹 Keep your HTML clean, semantic, and easy to read.

> **Simple Rule:**
> HTML tells the browser **what the content is**.
> CSS tells the browser **how it should look**.
> JavaScript tells the browser **what it should do**.

---

## 🚀 Next Step

After learning HTML, the recommended learning path is:

```text
HTML
 ↓
CSS
 ↓
JavaScript
 ↓
Git & GitHub
 ↓
Responsive Web Design
 ↓
Frontend Frameworks
```
