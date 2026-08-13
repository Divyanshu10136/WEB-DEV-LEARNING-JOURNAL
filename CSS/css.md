# CSS Notes 🎨

> **"HTML builds the skeleton. CSS gives it life."**

CSS is what makes a webpage look like a **real website**.

Without CSS, webpages would mostly be plain text and basic HTML elements. CSS adds:

* 🎨 Colors
* 🔤 Fonts
* 📏 Sizes
* 📦 Spacing
* 🧱 Layouts
* ✨ Animations
* 📱 Responsive design

---

## 📚 Table of Contents

1. [Why CSS Exists](#1-why-css-exists)
2. [Where to Put CSS](#2-where-to-put-css)
3. [Three Ways to Write CSS](#3-three-ways-to-write-css)
4. [CSS Syntax](#4-css-syntax)
5. [CSS Selectors](#5-css-selectors)
6. [The DOM](#6-the-dom)
7. [Developer Tools](#7-developer-tools)
8. [CSS File Paths](#8-css-file-paths)
9. [Summary](#9-summary)

---

# 1. Why CSS Exists

HTML is used to create the **structure** of a webpage.

CSS is used to control **how that structure looks**.

For example:

```text
HTML → Structure
CSS  → Styling
JS   → Behaviour
```

### Example

HTML:

```html
<h1>Hello World</h1>
```

Without CSS, it will have the browser's default styling.

With CSS:

```css
h1 {
  color: blue;
  text-align: center;
}
```

Now the heading becomes blue and centered.

---

## 🏠 Simple Example

Think about building a house:

* 🧱 **HTML** → Walls, doors, rooms and structure
* 🎨 **CSS** → Paint, furniture and decoration
* ⚙️ **JavaScript** → Electricity, switches and functionality

HTML tells us **what is on the page**.

CSS tells us **how it looks**.

JavaScript tells us **how it behaves**.

---

# 2. Where to Put CSS

CSS can be added to an HTML page in different ways.

A common and recommended approach is to use an **external CSS file**.

```html
<!DOCTYPE html>
<html>

<head>
  <link rel="stylesheet" href="./style.css">
</head>

<body>

  <h1>Hello World</h1>

</body>

</html>
```

The CSS file:

```css
h1 {
  color: blue;
}
```

### Why put CSS in `<head>`?

The browser reads the HTML document from top to bottom.

Loading the stylesheet early helps the browser apply styles while rendering the page.

This can help avoid a **FOUC (Flash of Unstyled Content)**, where users briefly see unstyled HTML before the CSS loads.

> 💡 **Best practice:** Put your external stylesheet `<link>` inside the `<head>`.

---

# 3. Three Ways to Write CSS

There are three common ways to add CSS.

---

## 3.1 Inline CSS ❌

Inline CSS is written directly inside an HTML element using the `style` attribute.

```html
<p style="color: red;">
  This is red text.
</p>

<h1 style="color: blue;">
  Hello
</h1>
```

### Problems with Inline CSS

* ❌ Difficult to maintain
* ❌ Repeated code
* ❌ Makes HTML messy
* ❌ Not reusable
* ❌ Difficult to update large websites

### When can it be useful?

For quick testing or very specific one-time styles.

---

## 3.2 Internal CSS ⚠️

Internal CSS is written inside a `<style>` tag.

Usually, the `<style>` tag goes inside `<head>`.

```html
<!DOCTYPE html>

<html>

<head>

  <style>

    p {
      color: red;
      font-size: 18px;
    }

    h1 {
      color: blue;
    }

  </style>

</head>

<body>

  <h1>Hello</h1>

  <p>This is red text.</p>

</body>

</html>
```

### Advantages

* Better than inline CSS
* Everything is in one place
* Useful for small projects

### Disadvantages

* Styles are limited to that HTML file
* Not easily reusable across multiple pages
* Large projects become harder to maintain

### Good for

* Small projects
* Practice
* Quick prototypes
* Single-page projects

---

## 3.3 External CSS ✅

External CSS is written in a separate `.css` file.

### `index.html`

```html
<!DOCTYPE html>

<html>

<head>

  <link
    rel="stylesheet"
    href="./style.css"
  >

</head>

<body>

  <h1>Hello</h1>

  <p>This paragraph is styled using CSS.</p>

</body>

</html>
```

### `style.css`

```css
p {
  color: red;
  font-size: 18px;
}

h1 {
  color: blue;
}
```

### Why External CSS is Recommended

* ✅ Easy to maintain
* ✅ Reusable
* ✅ Keeps HTML clean
* ✅ One CSS file can style multiple HTML pages
* ✅ Easier to debug
* ✅ Browsers can cache CSS files

---

## Quick Comparison

| Feature                 | Inline   | Internal     | External    |
| ----------------------- | -------- | ------------ | ----------- |
| Written in              | HTML tag | `<style>`    | `.css` file |
| Reusable                | ❌ No     | ❌ Limited    | ✅ Yes       |
| Easy to maintain        | ❌ No     | ⚠️ Sometimes | ✅ Yes       |
| Good for large projects | ❌ No     | ❌ No         | ✅ Yes       |
| Recommended             | ❌        | ⚠️           | ✅           |

> ⭐ **For most projects, use External CSS.**

---

# 4. CSS Syntax

Every CSS rule follows this basic structure:

```css
selector {
  property: value;
}
```

### Example

```css
h1 {
  color: blue;
  font-size: 32px;
  text-align: center;
}
```

### Parts of a CSS Rule

| Part           | Meaning     |
| -------------- | ----------- |
| `h1`           | Selector    |
| `color`        | Property    |
| `blue`         | Value       |
| `color: blue;` | Declaration |
| Entire block   | CSS Rule    |

---

## CSS Comments

Comments are written like this:

```css
/* This is a CSS comment */
```

Comments are ignored by the browser.

They are useful for explaining your code.

---

## Basic CSS Rules

### Semicolon

Declarations usually end with `;`.

```css
p {
  color: red;
  font-size: 18px;
}
```

### Curly Brackets

CSS declarations are written inside `{ }`.

```css
p {
  color: red;
}
```

### Formatting

CSS ignores extra spaces and line breaks.

This works:

```css
p {
  color: red;
  font-size: 18px;
}
```

This also works:

```css
p{color:red;font-size:18px;}
```

But the first version is much easier for humans to read.

---

# 5. CSS Selectors

Selectors tell CSS **which HTML elements should be styled**.

---

## 5.1 Element Selector

Targets all elements of a particular type.

HTML:

```html
<p>Hello</p>
<p>Welcome</p>
```

CSS:

```css
p {
  color: gray;
  line-height: 1.6;
}
```

Both `<p>` elements will receive the style.

---

## 5.2 Class Selector

A class is used when you want to style **multiple elements**.

HTML:

```html
<div class="card">
  Card 1
</div>

<div class="card">
  Card 2
</div>

<p class="card">
  This can also use the class.
</p>
```

CSS:

```css
.card {
  background: white;
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
}
```

All elements with `class="card"` will receive the style.

### Class Syntax

```css
.class-name {
  property: value;
}
```

> ⭐ **Use classes when the same style needs to be applied to multiple elements.**

---

## 5.3 ID Selector

An ID is used to identify a **specific element**.

HTML:

```html
<header id="main-header">
  My Website
</header>
```

CSS:

```css
#main-header {
  background: #1a1a2e;
  color: white;
  padding: 24px;
}
```

### ID Syntax

```css
#id-name {
  property: value;
}
```

> ⭐ An `id` should be unique within a page.

---

## Class vs ID

| Class                        | ID                                 |
| ---------------------------- | ---------------------------------- |
| Starts with `.`              | Starts with `#`                    |
| Can be used on many elements | Should identify one unique element |
| Used for reusable styles     | Used for a specific element        |
| Example: `.card`             | Example: `#header`                 |

### Easy way to remember

```text
Class → Many elements
ID    → One unique element
```

---

## 5.4 Grouping Selector

You can apply the same CSS to multiple selectors using commas.

### Without grouping

```css
h1 {
  color: #333;
}

h2 {
  color: #333;
}

h3 {
  color: #333;
}
```

### With grouping

```css
h1,
h2,
h3 {
  color: #333;
}
```

Grouping keeps your CSS shorter and cleaner.

---

## 5.5 Descendant Selector

A descendant selector targets an element **inside another element**.

HTML:

```html
<div class="article">

  <p>
    This paragraph is inside .article.
  </p>

</div>

<p>
  This paragraph is outside .article.
</p>
```

CSS:

```css
.article p {
  font-size: 16px;
  line-height: 1.8;
}
```

Only the `<p>` inside `.article` will be styled.

### Syntax

```css
parent child {
  property: value;
}
```

---

# 6. The DOM

**DOM** stands for:

> **Document Object Model**

When the browser reads HTML, it creates a **tree-like structure** called the DOM.

You can think of it like a family tree.

Example HTML:

```html
<body>

  <header>
    <h1>My Website</h1>
  </header>

  <main>

    <div class="card">
      <p>Hello</p>
    </div>

    <p>Outside card</p>

  </main>

</body>
```

The DOM roughly looks like:

```text
Document
└── html
    ├── head
    └── body
        ├── header
        │   └── h1
        └── main
            ├── div.card
            │   └── p
            └── p
```

CSS uses this structure to find elements.

For example:

```css
.card p {
  color: blue;
}
```

This means:

> Find `<p>` elements that are inside `.card`.

---

## What Does "Cascading" Mean?

The **C** in CSS stands for **Cascading**.

CSS rules can flow through the document and can be inherited or overridden by other rules.

For example:

```css
body {
  color: black;
}
```

Text inside the body may inherit this color unless another rule changes it.

CSS also decides which style wins when multiple rules target the same element.

---

# 7. Developer Tools

Browser **Developer Tools (DevTools)** are extremely useful when working with CSS.

They allow you to:

* Inspect HTML
* Check CSS
* Test styles
* Find errors
* Debug layouts
* See computed styles

---

## Opening DevTools

### Windows / Linux

```text
F12
```

or:

```text
Ctrl + Shift + I
```

### Mac

```text
Cmd + Option + I
```

You can also:

> Right-click an element → **Inspect**

---

## Elements Tab

The **Elements** panel shows the webpage's HTML/DOM structure.

You can:

* Select elements
* Check classes
* Check IDs
* See parent/child relationships
* Edit HTML temporarily

---

## Styles Panel

The Styles panel shows the CSS rules applied to an element.

You can:

* Change CSS values
* Disable properties
* Test different colors
* Test spacing
* See which CSS rule is being applied

For example:

```css
color: red;
```

You can click `red` and change it to:

```css
blue
```

and see the result immediately.

---

## Crossed-Out CSS

If a CSS property is crossed out:

```css
color: red;
```

it usually means another CSS rule is taking priority or overriding it.

---

## Computed Tab

The **Computed** panel shows the final value of CSS properties after the browser processes all the CSS rules.

This is useful when you're asking:

> "Why isn't my CSS working?"

---

## ⚠️ Important

Changes made in DevTools are **temporary**.

If you refresh the page, your changes disappear.

So after testing a change:

> Copy the working CSS into your actual `.css` file.

---

# 8. CSS File Paths

When using an external CSS file, the browser needs to know **where the file is located**.

Example:

```html
<link
  rel="stylesheet"
  href="./style.css"
>
```

---

## Same Folder

Project:

```text
project/
├── index.html
└── style.css
```

HTML:

```html
<link
  rel="stylesheet"
  href="./style.css"
>
```

`./` means:

> Start from the current folder.

---

## CSS Inside a Folder

Project:

```text
project/
├── index.html
└── css/
    └── style.css
```

HTML:

```html
<link
  rel="stylesheet"
  href="./css/style.css"
>
```

---

## Going One Folder Up

Project:

```text
project/
├── style.css
└── pages/
    └── about.html
```

From `about.html`, go one folder up:

```html
<link
  rel="stylesheet"
  href="../style.css"
>
```

`../` means:

> Go one folder up.

---

## Common Path Mistakes

| Mistake                    | Problem                                        |
| -------------------------- | ---------------------------------------------- |
| Wrong filename             | CSS file won't load                            |
| Wrong folder path          | Browser can't find the file                    |
| `Style.css` vs `style.css` | Can fail on case-sensitive systems             |
| Missing `rel="stylesheet"` | Browser may not treat the file as a stylesheet |

---

## Debugging CSS That Isn't Loading

If your CSS isn't working:

1. Open DevTools
2. Go to the **Network** tab
3. Refresh the page
4. Find your CSS file
5. Check the status

If you see:

```text
404 Not Found
```

your CSS file path is probably wrong.

---

# 9. Summary

CSS is used to control the **appearance and layout of webpages**.

### Remember:

* 🎨 **CSS = Styling**
* 🧱 **HTML = Structure**
* ⚙️ **JavaScript = Behaviour**
* 📄 **External CSS** is usually the best choice for real projects.
* 🎯 **Selectors** tell CSS which elements to style.
* `.class` → Reusable style for multiple elements.
* `#id` → Unique element.
* 🌳 **DOM** → Tree-like representation of HTML.
* 🛠️ **DevTools** → Helps inspect and debug CSS.
* 📁 `./` → Current folder.
* 📁 `../` → One folder up.
* 🔗 `<link>` → Connects external CSS to HTML.

---

## 💡 Quick CSS Example

### HTML

```html
<!DOCTYPE html>

<html>

<head>

  <link
    rel="stylesheet"
    href="./style.css"
  >

</head>

<body>

  <div class="card">

    <h1>My Website</h1>

    <p>
      I am learning CSS.
    </p>

    <button>
      Click Me
    </button>

  </div>

</body>

</html>
```

### CSS

```css
.card {
  width: 300px;
  padding: 20px;
  background: white;
  border-radius: 10px;
}

.card h1 {
  color: blue;
}

.card p {
  color: gray;
}

.card button {
  background: blue;
  color: white;
  padding: 10px 20px;
  border: none;
}
```

---

## 🚀 Learning Path

```text
HTML
  ↓
CSS
  ↓
JavaScript
  ↓
Git & GitHub
  ↓
Responsive Design
  ↓
Frontend Frameworks
```

---

<p align="center">
  Made while learning Web Development 🚀
</p>

<p align="center">
  <b>Notes by Divyanshu Shah</b>
</p>
