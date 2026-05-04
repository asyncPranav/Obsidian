

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

---

## 2. URL Encoded

```js
app.use(express.urlencoded({ extended: true }));
```

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

