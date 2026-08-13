
# 🌐 What Happens When You Open a Website in a Browser?

```text
URL
 ↓
DNS Lookup
 ↓
TCP Connection
 ↓
TLS Handshake
 ↓
HTTP Request
 ↓
HTTP Response
 ↓
HTML + CSS + JavaScript
 ↓
Browser Rendering
 ↓
🌐 Website
````

---

## 📚 Table of Contents

* [1. Full Overview](#1-full-overview)
* [2. DNS Resolution](#2-dns-resolution)
* [3. TCP Connection](#3-tcp-connection)
* [4. TLS Handshake](#4-tls-handshake)
* [5. HTTP Request](#5-http-request)
* [6. HTTP Response](#6-http-response)
* [7. Browser Rendering](#7-browser-rendering)
* [8. Network Requests](#8-network-requests)
* [9. Browser Cache](#9-browser-cache)
* [10. Loading Timeline](#10-loading-timeline)
* [11. Key Concepts](#11-key-concepts)
* [12. Complete Journey](#12-complete-journey)
* [13. What I Learned](#13-what-i-learned)
* [14. Author](#14-author)

---

# 1. Full Overview

Suppose we open:

```text
https://shorterloop.com
```

After pressing **Enter**, the browser roughly follows this process:

```text
You press Enter
      ↓
┌─────────────────────┐
│   DNS Resolution    │
│   Domain → IP       │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   TCP Connection    │
│   3-Way Handshake   │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   TLS Handshake     │
│   Secure Connection │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   HTTP Request      │
│      GET /          │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   HTTP Response     │
│    200 / 304        │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│ Browser Rendering   │
│ HTML + CSS + JS     │
└──────────┬──────────┘
           ↓
       🌐 Website
```

---

# 2. DNS Resolution 🔍

## What is DNS?

**DNS = Domain Name System**

DNS converts a human-readable domain name into an IP address.

For example:

```text
shorterloop.com
       ↓
      DNS
       ↓
199.36.158.100
```

### Easy Example

Think of DNS as the **phonebook of the internet**.

You know a person's name, but the phonebook gives you their phone number.

Similarly:

```text
Domain Name → DNS → IP Address
```

---

## How DNS Resolution Works

The browser may first check its cache.

```text
Browser Cache
     |
     ├── HIT  → IP found ✅
     |
     └── MISS
          ↓
     DNS Resolver
          ↓
     Root DNS Server
          ↓
     TLD DNS Server
        (.com)
          ↓
     Authoritative DNS Server
          ↓
       IP Address
```

### Example Command

```bash
nslookup shorterloop.com
```

Example output:

```text
Server:   reliance.reliance
Address:  2405:201:680d:d2be::c0a8:1d01

Non-authoritative answer:
Name:     shorterloop.com
Address:  199.36.158.100
```

> **Note:** A "Non-authoritative answer" generally means the answer came from a DNS resolver/cache rather than directly from the domain's authoritative DNS server.

---

# 3. TCP Connection 🔗

Once the browser knows the server's IP address, it needs to establish a connection.

For a traditional HTTPS connection using TCP, port **443** is normally used.

TCP uses a **3-way handshake**.

```text
Browser                         Server
   |                               |
   |---------- SYN -------------->|
   |                               |
   |<--------- SYN-ACK ------------|
   |                               |
   |---------- ACK -------------->|
   |                               |
   |     Connection Established    |
```

## Easy Way to Remember

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
Connection Established
```

## Simple Meaning

| Step       | Meaning                                |
| ---------- | -------------------------------------- |
| `SYN`      | Client wants to establish a connection |
| `SYN-ACK`  | Server accepts and responds            |
| `ACK`      | Client confirms                        |
| **Result** | Connection established                 |

---

# 4. TLS Handshake 🔒

After establishing the TCP connection, HTTPS uses **TLS** to secure communication.

**TLS = Transport Layer Security**

TLS helps provide:

* Encryption
* Server authentication
* Secure communication

A simplified TLS handshake looks like this:

```text
Browser                         Server
   |                               |
   |------ ClientHello ---------->|
   |                               |
   |<----- ServerHello + Cert -----|
   |                               |
   |  Verify Certificate           |
   |                               |
   |------ Key Exchange ---------->|
   |                               |
   |<--------- Finished -----------|
   |                               |
   |      Secure Connection 🔒     |
```

After the TLS handshake, HTTPS communication is protected by encryption.

```text
Browser
   ↓
🔒 Encrypted Data
   ↓
Server
```

> **Note:** The exact TLS handshake depends on the TLS version and connection details. This diagram is a simplified explanation.

---

# 5. HTTP Request 📤

Now the browser has established a connection with the server.

It sends an **HTTP request** asking for the webpage.

Example:

```http
GET / HTTP/1.1
Host: shorterloop.com
Accept: text/html,application/xhtml+xml
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cache-Control: max-age=0
User-Agent: Mozilla/5.0
```

---

## Understanding the Request

### `GET /`

```http
GET /
```

Means:

> Request the resource located at `/`.

Usually `/` represents the website's homepage.

---

### `Host`

```http
Host: shorterloop.com
```

Tells the server which website is being requested.

A single server can host multiple websites.

---

### `Accept`

```http
Accept: text/html
```

Tells the server which types of content the browser can handle.

---

### `Cache-Control`

```http
Cache-Control: max-age=0
```

Provides caching instructions for the request.

---

### `User-Agent`

```http
User-Agent: Mozilla/5.0
```

Provides information about the browser/client making the request.

---

# 6. HTTP Response 📥

The server processes the request and sends a response back.

An HTTP response contains things such as:

```text
Status Code
Headers
Response Body
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

The response body may contain HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Website</title>
  </head>

  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

---

## Common HTTP Status Codes

| Code  | Status                | Meaning                     |
| ----- | --------------------- | --------------------------- |
| `200` | OK                    | Request succeeded           |
| `301` | Moved Permanently     | Resource moved permanently  |
| `302` | Found                 | Temporary redirect          |
| `304` | Not Modified          | Use cached version          |
| `404` | Not Found             | Resource doesn't exist      |
| `500` | Internal Server Error | Server encountered an error |

---

## What is `304 Not Modified`?

Suppose your browser already has a CSS file stored in its cache.

The browser asks the server if the file has changed.

The server responds:

```http
HTTP/1.1 304 Not Modified
```

This means:

> "The resource hasn't changed. You can use your cached copy."

```text
Browser
   ↓
"Has this file changed?"
   ↓
Server
   ↓
"304 — No changes."
   ↓
Browser uses cached file
```

This can reduce unnecessary downloads and improve loading performance.

---

# 7. Browser Rendering 🎨

Receiving HTML is not the final step.

The browser now needs to convert HTML, CSS, JavaScript, images, fonts, and other resources into the webpage you see.

```text
HTML Received
      ↓
HTML Parsing
      ↓
DOM Created
      ↓
CSS Found
      ↓
CSSOM Created
      ↓
JavaScript Execution
      ↓
DOM + CSSOM
      ↓
Render Tree
      ↓
Layout
      ↓
Paint
      ↓
Composite
      ↓
🌐 Website
```

---

## 7.1 HTML Parsing

The browser receives HTML:

```html
<body>
  <h1>Hello World</h1>
  <p>Welcome to my website.</p>
</body>
```

It parses the HTML and creates the **DOM**.

**DOM = Document Object Model**

Example:

```text
Document
└── body
    ├── h1
    └── p
```

---

## 7.2 CSS Parsing

Suppose the browser finds:

```css
h1 {
  color: blue;
}

p {
  color: gray;
}
```

The browser parses the CSS and creates a structure called the **CSSOM**.

```text
CSS
 ↓
CSSOM
```

---

## 7.3 JavaScript Execution

If the page contains:

```html
<script src="script.js"></script>
```

The browser may download and execute the JavaScript.

JavaScript can:

* Change HTML
* Change CSS
* Handle clicks
* Fetch data
* Create animations
* Update webpage content
* Communicate with APIs

---

## 7.4 Render Tree

The browser combines the DOM and CSS information.

```text
DOM
 +
CSSOM
 ↓
Render Tree
```

The Render Tree contains information needed to determine what should be displayed.

---

## 7.5 Layout

The browser calculates the position and size of elements.

For example:

```text
Where should the heading appear?
How wide should the paragraph be?
Where should the button be placed?
How much space does each element need?
```

This process is called **Layout** or **Reflow**.

---

## 7.6 Paint

The browser draws visual elements such as:

* Text
* Colors
* Borders
* Images
* Shadows
* Backgrounds

```text
Layout
  ↓
Paint
  ↓
Pixels
```

---

## 7.7 Composite

Finally, the browser combines the rendered layers to produce the final image.

```text
Painted Layers
      ↓
  Composite
      ↓
🖥️ Final Website
```

---

# 8. Network Requests 📊

A website is usually made up of many different files.

For example:

```text
index.html
    |
    ├── style.css
    ├── script.js
    ├── logo.svg
    ├── image.jpg
    ├── font.woff2
    └── analytics.js
```

So when you open one URL, the browser may make many requests.

```text
GET /                  → HTML
GET /style.css         → CSS
GET /script.js         → JavaScript
GET /logo.svg          → Logo
GET /image.jpg         → Image
GET /font.woff2        → Font
```

You can see these requests using:

```text
Browser DevTools
      ↓
Network Tab
```

---

## Example Network Table

| File           | Status | Type     | Cache        |
| -------------- | -----: | -------- | ------------ |
| `/`            |  `304` | Document | Cached       |
| `analytics.js` |  `200` | Script   | Downloaded   |
| `font.woff2`   |  `200` | Font     | Memory Cache |
| `image.jpg`    |  `200` | Image    | Memory Cache |
| `logo.svg`     |  `200` | SVG      | Memory Cache |
| `favicon.png`  |  `200` | Image    | Disk Cache   |

> **Note:** The exact number of requests, file sizes, and loading times depend on the website, browser, network, server, and cache.

---

# 9. Browser Cache ⚡

Browsers store previously downloaded resources so they can sometimes reuse them instead of downloading them again.

## Memory Cache

```text
Memory Cache
     ↓
Stored in RAM
     ↓
Very Fast ⚡
```

## Disk Cache

```text
Disk Cache
     ↓
Stored on Device Storage
     ↓
Usually Slower Than Memory
```

## Network Request

If the resource isn't available locally:

```text
Browser
   ↓
Internet
   ↓
Server
   ↓
Resource
```

---

## Cache Comparison

| Cache        | Stored Where?   | General Speed  |
| ------------ | --------------- | -------------- |
| Memory Cache | RAM             | ⚡ Very Fast    |
| Disk Cache   | Device Storage  | Fast           |
| Network      | Server/Internet | Usually Slower |

---

# 10. Loading Timeline 📈

A website may perform many operations while loading.

A simplified example:

```text
0ms        200ms      400ms      600ms      800ms     1000ms    1300ms
|----------|----------|----------|----------|----------|---------|
[==DNS==]  |          |          |          |          |
    [=TCP+TLS=]       |          |          |          |
           [===HTML===]          |          |          |
                  [===CSS+Fonts==]          |          |
                           [=====JS========]          |
                                    [=====Images======]
                                                [===Analytics===]
```

> **Note:** This is only an example. Real websites can have very different loading timelines.

---

# 11. Key Concepts 🧠

| Concept           | Simple Explanation                       |
| ----------------- | ---------------------------------------- |
| **URL**           | Address of a resource on the web         |
| **DNS**           | Converts domain name into IP address     |
| **IP Address**    | Identifies the destination server/device |
| **TCP**           | Establishes a reliable connection        |
| **TLS**           | Secures communication using encryption   |
| **HTTPS**         | HTTP communication protected by TLS      |
| **HTTP GET**      | Requests a resource                      |
| **HTTP Response** | Server's response to the request         |
| **200 OK**        | Request succeeded                        |
| **301**           | Permanent redirect                       |
| **304**           | Use cached version                       |
| **404**           | Resource not found                       |
| **500**           | Server error                             |
| **DOM**           | Tree representation of HTML              |
| **CSSOM**         | Browser representation of CSS            |
| **Render Tree**   | Information used for rendering           |
| **Layout**        | Calculates element positions and sizes   |
| **Paint**         | Draws visual elements                    |
| **Composite**     | Combines rendered layers                 |
| **Memory Cache**  | Cache stored in RAM                      |
| **Disk Cache**    | Cache stored on device storage           |

---

# 12. Complete Journey 🔁

Here is the complete process in one diagram:

```text
                 Type URL
                    ↓
               Press Enter
                    ↓
             ┌────────────┐
             │ DNS Lookup │
             └─────┬──────┘
                   ↓
              IP Address
                   ↓
          ┌────────────────┐
          │ TCP Handshake  │
          └───────┬────────┘
                  ↓
          ┌────────────────┐
          │ TLS Handshake  │
          └───────┬────────┘
                  ↓
         Secure Connection 🔒
                  ↓
          ┌────────────────┐
          │ HTTP Request   │
          │    GET /       │
          └───────┬────────┘
                  ↓
          ┌────────────────┐
          │ HTTP Response  │
          │   200 / 304    │
          └───────┬────────┘
                  ↓
                 HTML
                  ↓
            Build DOM
                  ↓
             Parse CSS
                  ↓
                CSSOM
                  ↓
        Execute JavaScript
                  ↓
            DOM + CSSOM
                  ↓
            Render Tree
                  ↓
               Layout
                  ↓
                Paint
                  ↓
              Composite
                  ↓
             🌐 Website
```

---

# 13. What I Learned 📝

* DNS converts domain names into IP addresses.
* TCP establishes a network connection.
* TLS secures HTTPS communication.
* HTTP is used for communication between browser and server.
* HTTP status codes tell us what happened to a request.
* HTML is converted into the DOM.
* CSS is converted into the CSSOM.
* JavaScript can modify the webpage and interact with APIs.
* The browser creates a Render Tree.
* Layout calculates element positions and sizes.
* Paint draws visual elements.
* Browser caching can make repeat visits faster.
* One webpage can make many network requests.
* Browser DevTools can be used to inspect network activity.

---

# 💡 Easy Way to Remember

```text
URL
 ↓
DNS
 ↓
TCP
 ↓
TLS
 ↓
HTTP Request
 ↓
HTTP Response
 ↓
HTML + CSS + JS
 ↓
DOM + CSSOM
 ↓
Render Tree
 ↓
Layout
 ↓
Paint
 ↓
🌐 Website
```

> **In short:** You enter a URL → DNS finds the server → TCP connects → TLS secures the connection → HTTP requests the webpage → the server responds → the browser processes HTML/CSS/JS → the browser renders the website.

---

# 👨‍💻 Author

**Divyanshu Shah**

> Learning Web Development — One Concept at a Time 🚀

```
```
