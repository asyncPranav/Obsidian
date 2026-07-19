

---

Here are **detailed notes on Application Middleware in Express.js** with theory + examples 👇

---

# 📘 Application Middleware in Express.js

## 🔹 What is Middleware?

Middleware is a **function that runs between request and response**.

👉 It has access to:

- `req` (request)
    
- `res` (response)
    
- `next` (function to pass control)
    

### Basic Syntax:

```js
function middleware(req, res, next) {
  // do something
  next();
}
```

---

# 🔹 What is Application Middleware?

👉 **Application middleware** is middleware that is attached to the **main Express app object (`app`)** using:

```js
app.use()
app.METHOD()
```

👉 It runs **for the entire application (globally)** or for specific paths.

Attached to the entire app using `app.use()` or `app.METHOD()`.

```js
app.use((req, res, next) => {
  console.log("Request received");
  next();
});
```

or

```js
app.get("/about", (req, res, next) => {
  console.log("About page");
  next();
});
```

Used for:

- Logging
- Authentication
- Parsing JSON
- CORS

---

# 🔹 Types of Application Middleware

## 1. Global Middleware

Runs for **every request in the app**

### Example:

```js
const express = require("express");
const app = express();

app.use((req, res, next) => {
  console.log("Middleware runs for every request");
  next();
});

app.get("/", (req, res) => {
  res.send("Home Page");
});

app.get("/about", (req, res) => {
  res.send("About Page");
});

app.listen(3000);
```

👉 Output:

- Every route (`/`, `/about`) will trigger middleware
    

---

## 2. Path-specific Middleware

Runs only for **specific routes**

### Example:

```js
app.use("/user", (req, res, next) => {
  console.log("User middleware");
  next();
});

app.get("/user/profile", (req, res) => {
  res.send("User Profile");
});

app.get("/about", (req, res) => {
  res.send("About Page");
});
```

👉 Runs only for:

- `/user/profile`
    

❌ Not for:

- `/about`
    

---

## 3. Route-level Middleware

Applied directly to a specific route

### Example:

```js
function checkAuth(req, res, next) {
  console.log("Checking auth...");
  next();
}

app.get("/dashboard", checkAuth, (req, res) => {
  res.send("Dashboard");
});
```

👉 Runs only when `/dashboard` is accessed

---

# ✍️ Difference among three

The easiest way to understand them is by **where they are attached** and **when they run**.

|Type|Attached To|Runs When|
|---|---|---|
|**Global Application Middleware**|`app.use()`|Every request|
|**Path-specific Application Middleware**|`app.use("/path", ...)`|Any request whose path starts with that prefix|
|**Route-level Middleware**|`app.get()`, `app.post()`, etc.|Only that specific route and HTTP method|

---

# 1. Global Application Middleware

```javascript
app.use((req, res, next) => {
  console.log("Global");
  next();
});
```

Runs for:

```text
GET /
GET /about
POST /login
DELETE /students/1

✅ Everything
```

Think of it as a security guard at the **main entrance** of the building.

---

# 2. Path-specific Application Middleware

```javascript
app.use("/about", (req, res, next) => {
  console.log("About middleware");
  next();
});
```

Runs for:

```text
GET /about
GET /about/team
POST /about
DELETE /about/123

✅ Anything starting with /about
```

Does **not** run for:

```text
GET /
GET /contact
GET /student
```

Think of it as a security guard for the **About department**.

---

# 3. Route-level Middleware

```javascript
app.get("/contact", middleware, (req, res) => {
  res.send("Contact");
});
```

Runs **only** for:

```text
GET /contact
```

Does **not** run for:

```text
POST /contact
GET /contact/team
DELETE /contact

❌ Doesn't run
```

Think of it as a guard standing **outside one specific room**.

---

## Rule to Remember

- **`app.use()`** → Middleware for the whole app or a path prefix.
    
- **`app.use("/path")`** → Middleware for all routes starting with that path.
    
- **`app.get("/path", middleware, handler)`** → Middleware for one specific HTTP method and route.

---

# 🔹 How Middleware Works (Flow)

### Flow Diagram:

```
Request → Middleware 1 → Middleware 2 → Route → Response
```

### Example:

```js
app.use((req, res, next) => {
  console.log("Middleware 1");
  next();
});

app.use((req, res, next) => {
  console.log("Middleware 2");
  next();
});

app.get("/", (req, res) => {
  res.send("Home");
});
```

👉 Output order:

```
Middleware 1
Middleware 2
```

---

# 🔹 Important Concept: `next()`

👉 `next()` is **VERY IMPORTANT**

- It passes control to the next middleware
    
- Without it → request will **hang**
    

### ❌ Wrong:

```js
app.use((req, res, next) => {
  console.log("Stopped");
});
```

👉 Browser will keep loading (no response)

---

### ✅ Correct:

```js
app.use((req, res, next) => {
  console.log("Continue");
  next();
});
```

---

# 🔹 Multiple Middleware in One Route

```js
function m1(req, res, next) {
  console.log("M1");
  next();
}

function m2(req, res, next) {
  console.log("M2");
  next();
}

app.get("/", m1, m2, (req, res) => {
  res.send("Done");
});
```

👉 Output:

```
M1
M2
```

---

# 🔹 Built-in Middleware

Express provides some built-in middleware:

## 1. JSON Parser

```js
app.use(express.json());
```

👉 Parses JSON request body

### Purpose

Parses **JSON data** sent in the request body.

### Without it

```js
POST /students

{
  "name": "Pranav"
}
```

```js
console.log(req.body);
```

Output:

```js
undefined
```

### With it

```js
app.use(express.json());
```

Output:

```js
{
  name: "Pranav"
}
```

**Used for:**

- REST APIs
- React frontend
- Postman
- Mobile apps

---

## 2. URL Encoded

```js
app.use(express.urlencoded({ extended: true }));
```

### Purpose

Parses **form data** (`application/x-www-form-urlencoded`).

Example HTML form:

```
<form method="POST">
  <input name="username">
</form>
```

Then:

```
console.log(req.body);
```

Output:

```
{
  username: "Pranav"
}
```

**Used for:**

- HTML forms
- Traditional websites
---

## 3. Static Files

```js
app.use(express.static("public"));
```

👉 Serves static files like images, CSS, JS

---

# 🔹 Error-handling Middleware

👉 Special middleware with **4 parameters**

```js
app.use((err, req, res, next) => {
  console.error(err.message);
  res.status(500).send("Error occurred");
});
```

---

# 🔹 Middleware Order Matters ⚠️

👉 Order defines execution

### Example:

```js
app.use((req, res, next) => {
  console.log("First");
  next();
});

app.use((req, res, next) => {
  console.log("Second");
  next();
});
```

👉 Always runs in written order

---

# 🔹 Combining Application + Router Middleware

```js
const router = express.Router();

router.use((req, res, next) => {
  console.log("Router middleware");
  next();
});

app.use((req, res, next) => {
  console.log("App middleware");
  next();
});

app.use("/user", router);
```

👉 Order:

1. App middleware
    
2. Router middleware
    
3. Route
    

---

# 🔹 Real-world Example (Logger Middleware)

```js
app.use((req, res, next) => {
  const time = new Date().toLocaleTimeString();
  console.log(`${req.method} ${req.url} at ${time}`);
  next();
});
```

👉 Logs every request

---

# 🔹 Key Differences: app.use vs router.use

|Feature|app.use()|router.use()|
|---|---|---|
|Scope|Entire app|Specific router|
|Usage|Global middleware|Modular middleware|
|Access|All routes|Router routes only|

---

# 🔹 Summary

- Middleware = function between request & response
    
- Application middleware = attached using `app.use()`
    
- Can be:
    
    - Global
        
    - Path-based
        
    - Route-level
        
- `next()` is required to continue flow
    
- Order matters
    
- Helps in:
    
    - logging
        
    - authentication
        
    - validation
        
    - error handling
        

---


Here’s a **complete, detailed example of Application Middleware in Express.js** with **clear comments on every step** 👇

---

# 📘 Full Example: Application Middleware (with Comments)

```js
// Import express
const express = require("express");

// Create app
const app = express();


// ===============================
// 🔹 1. GLOBAL APPLICATION MIDDLEWARE
// ===============================

// This middleware runs for EVERY request
app.use((req, res, next) => {
  console.log("🌍 Global Middleware 1");

  // You can access request data
  console.log(`Method: ${req.method}, URL: ${req.url}`);

  // Always call next() to move forward
  next();
});


// Another global middleware
app.use((req, res, next) => {
  console.log("🌍 Global Middleware 2");

  // Example: adding custom data to request
  req.customData = "Hello from middleware";

  next();
});


// ===============================
// 🔹 2. BUILT-IN APPLICATION MIDDLEWARE
// ===============================

// Parses JSON body from requests
app.use(express.json());

// Parses URL-encoded data (form data)
app.use(express.urlencoded({ extended: true }));


// ===============================
// 🔹 3. PATH-SPECIFIC MIDDLEWARE
// ===============================

// This middleware runs ONLY for routes starting with /user
app.use("/user", (req, res, next) => {
  console.log("👤 User Middleware");

  // Example: simple authentication check
  const isLoggedIn = true;

  if (!isLoggedIn) {
    return res.status(401).send("Unauthorized");
  }

  next();
});


// ===============================
// 🔹 4. ROUTES
// ===============================

// Home route
app.get("/", (req, res) => {
  console.log("🏠 Home Route");

  res.send("Welcome to Home Page");
});


// About route
app.get("/about", (req, res) => {
  console.log("ℹ️ About Route");

  res.send("About Page");
});


// User route (will trigger /user middleware)
app.get("/user/profile", (req, res) => {
  console.log("👤 User Profile Route");

  // Access custom data added in middleware
  res.send(`User Profile - ${req.customData}`);
});


// ===============================
// 🔹 5. ROUTE-LEVEL MIDDLEWARE
// ===============================

// Custom middleware function
function checkAdmin(req, res, next) {
  console.log("🔐 Checking Admin");

  const isAdmin = true;

  if (!isAdmin) {
    return res.status(403).send("Forbidden");
  }

  next();
}

// Apply middleware only to this route
app.get("/admin", checkAdmin, (req, res) => {
  res.send("Admin Dashboard");
});


// ===============================
// 🔹 6. 404 HANDLER (NOT FOUND)
// ===============================

// If no route matches, this runs
app.use((req, res, next) => {
  console.log("❌ 404 Middleware");

  res.status(404).send("Page Not Found");
});


// ===============================
// 🔹 7. ERROR HANDLING MIDDLEWARE
// ===============================

// This handles errors
app.use((err, req, res, next) => {
  console.log("💥 Error Middleware");

  console.error(err.message);

  res.status(500).send("Something went wrong!");
});


// ===============================
// 🔹 8. START SERVER
// ===============================

app.listen(3000, () => {
  console.log("🚀 Server running on http://localhost:3000");
});
```

---

# 🔥 How This Works (Flow)

### When you open:

👉 `http://localhost:3000/user/profile`

### Execution order:

```text
1. Global Middleware 1
2. Global Middleware 2
3. /user Middleware
4. Route Handler (/user/profile)
5. Response sent
```

---

# 🔹 Key Concepts Explained

## ✅ Global Middleware

```js
app.use(...)
```

- Runs for every request
    
- Used for logging, parsing, etc.
    

---

## ✅ Path-specific Middleware

```js
app.use("/user", ...)
```

- Runs only for `/user/*` routes
    

---

## ✅ Route-level Middleware

```js
app.get("/admin", middleware, handler)
```

- Runs only for that route
    

---

## ✅ Built-in Middleware

```js
express.json()
```

- Parses incoming request data
    

---

## ✅ 404 Middleware

- Runs when no route matches
    

---

## ✅ Error Middleware

```js
(err, req, res, next)
```

- Handles errors
    

---

# 🔹 Important Rules ⚠️

1. Always call `next()`
    
2. Order matters
    
3. Error middleware must be last
    
4. Middleware can modify `req` & `res`
    
5. Can stop request by sending response
    

---

# 🔹 Real-world Use Cases

- Logging requests
    
- Authentication
    
- Validation
    
- Parsing data
    
- Error handling
    

---

# 🔹 Simple Analogy

👉 Think of middleware like checkpoints:

```text
Request → Security Check → ID Check → Route → Response
```
