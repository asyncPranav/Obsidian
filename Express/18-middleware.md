
---

# Middleware in Express.js — Complete Beginner Notes

---

## 1. What is Middleware? (The Big Picture)

Before jumping into code, let's build the right mental model.

When a request comes into your Express server, it doesn't magically jump straight to your route handler. It passes through a **chain of functions** one by one. Each of those functions in the chain is called **middleware**.

Think of it like an airport security check:

```
You (Request) → Ticket Check → Baggage Scan → ID Verify → Gate (Route Handler)
```

Each checkpoint can:

- Let you **pass through** to the next one
- **Modify** something about you (add a stamp, tag your bag)
- **Stop you** right there and send you back

Middleware works exactly the same way in Express.

```
Incoming Request
      ↓
  Middleware 1  (e.g., log the request)
      ↓
  Middleware 2  (e.g., parse JSON body)
      ↓
  Middleware 3  (e.g., check if user is logged in)
      ↓
  Route Handler (e.g., send back data)
      ↓
Outgoing Response
```

---

## 2. The Anatomy of a Middleware Function

Every middleware function in Express looks like this:

```js
function myMiddleware(req, res, next) {
  // req  → the incoming request object
  // res  → the outgoing response object
  // next → a function that passes control to the NEXT middleware
  
  console.log("I am a middleware!");
  
  next(); // ← VERY IMPORTANT: without this, the chain stops here!
}
```

The three parameters are:

- **`req`** — Contains everything about the incoming request: URL, headers, body, params, etc.
- **`res`** — Used to send a response back to the client.
- **`next`** — A function you call to say _"I'm done, pass control to the next middleware or route."_

> 🔑 **Key Rule:** If you don't call `next()` AND don't send a response with `res.send()` / `res.json()`, the request will just **hang** forever. Always do one of the two.

---

## 3. How to Register / Use Middleware

You register middleware using `app.use()`.

```js
const express = require("express");
const app = express();

// Registering a middleware globally (applies to ALL routes)
app.use(myMiddleware);

function myMiddleware(req, res, next) {
  console.log(`Request received: ${req.method} ${req.url}`);
  next(); // pass to the next middleware or route
}

app.get("/home", (req, res) => {
  res.send("Welcome to Home Page");
});

app.listen(3000);
```

When you hit `GET /home`, the flow is:

1. `myMiddleware` runs → logs the request
2. `next()` is called → moves to the route handler
3. Route handler sends `"Welcome to Home Page"`

---

## 4. Types of Middleware in Express

Express middleware falls into 5 main categories. Let's go through each one.

---

### 4.1 Application-Level Middleware

This middleware is attached to the **entire app** using `app.use()` or `app.METHOD()`. It runs for every request (or a specific path).

```js
const express = require("express");
const app = express();

// Runs for EVERY request, regardless of route
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
});

// Runs ONLY for requests starting with /user
app.use("/user", (req, res, next) => {
  console.log("Request is for /user path");
  next();
});

app.get("/", (req, res) => res.send("Home"));
app.get("/user/profile", (req, res) => res.send("User Profile"));

app.listen(3000);
```

---

### 4.2 Router-Level Middleware

This is the same as application-level middleware, but it's attached to an **Express Router** instance instead of `app`. This is useful when you want to group related routes.

```js
const express = require("express");
const app = express();
const router = express.Router(); // create a router

// Middleware only for this router
router.use((req, res, next) => {
  console.log("Router middleware triggered!");
  next();
});

router.get("/profile", (req, res) => {
  res.send("User Profile");
});

router.get("/settings", (req, res) => {
  res.send("User Settings");
});

// Mount the router on /user
app.use("/user", router);

// Now /user/profile and /user/settings will go through the router middleware
app.listen(3000);
```

---

### 4.3 Built-in Middleware

Express ships with a few built-in middleware functions. You've already used some of them!

```js
const express = require("express");
const app = express();

// 1. Parses incoming requests with JSON payloads
//    Needed when your client sends JSON (e.g., from fetch/axios with body)
app.use(express.json());

// 2. Parses incoming requests with URL-encoded payloads
//    Needed when your HTML <form> submits data (method="POST")
//    extended: false → uses querystring library (simple)
//    extended: true  → uses qs library (supports nested objects)
app.use(express.urlencoded({ extended: false }));

// 3. Serves static files like HTML, CSS, images from a folder
//    If you have a "public" folder with index.html, it will be served at "/"
app.use(express.static("public"));

app.listen(3000);
```

> 💡 **Why do we need `express.json()`?** By default, Express doesn't know how to read the body of a request. This middleware parses the raw body and puts the result in `req.body`. Without it, `req.body` is `undefined`.

---

### 4.4 Third-Party Middleware

These are middleware packages installed via npm. They extend Express with extra functionality.

Some popular ones:

|Package|Purpose|
|---|---|
|`morgan`|HTTP request logger|
|`cors`|Enable Cross-Origin Resource Sharing|
|`helmet`|Set security-related HTTP headers|
|`express-validator`|Validate and sanitize form/body data|
|`multer`|Handle file uploads|
|`cookie-parser`|Parse cookies from requests|

**Example with `morgan` (request logger):**

```js
const express = require("express");
const morgan = require("morgan"); // npm install morgan

const app = express();

// "dev" is a predefined format: METHOD URL STATUS TIME
app.use(morgan("dev"));

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000);
// Console output: GET / 200 3.456 ms - 11
```

**Example with `cors`:**

```js
const cors = require("cors"); // npm install cors

// Allow ALL origins (useful during development)
app.use(cors());

// OR allow only a specific origin
app.use(cors({
  origin: "https://myfrontend.com"
}));
```

---

### 4.5 Error-Handling Middleware

This is a **special** type of middleware. It has **4 parameters** instead of 3: `(err, req, res, next)`.

Express automatically calls this middleware when you pass an error to `next(err)` or when an unhandled error is thrown.

```js
const express = require("express");
const app = express();

app.get("/crash", (req, res, next) => {
  const err = new Error("Something went wrong!");
  err.status = 500;
  next(err); // ← pass error to error-handling middleware
});

// Error-handling middleware MUST be registered LAST (after all routes)
// It MUST have exactly 4 parameters — Express identifies it this way
app.use((err, req, res, next) => {
  console.error(err.message);
  res.status(err.status || 500).json({
    error: err.message
  });
});

app.listen(3000);
```

> ⚠️ **Important:** Express identifies error-handling middleware by the presence of **exactly 4 parameters**. If you write 3, it won't work as an error handler.

---

## 5. The `next()` Function — Deeply Understood

`next()` is the glue that holds the middleware chain together. Here are all the ways you can use it:

```js
// 1. Move to the next middleware / route handler
next();

// 2. Skip ALL remaining middleware and go straight to error handler
next(new Error("Something failed"));

// 3. Skip to the next ROUTE (rarely used)
next("route");
```

**Practical example — stopping the chain early:**

```js
// Authentication middleware
function isAuthenticated(req, res, next) {
  const token = req.headers["authorization"];
  
  if (!token) {
    // Stop the chain, send 401 response directly
    return res.status(401).json({ message: "Not authorized" });
  }
  
  // Token exists, move on
  next();
}

// Apply only to /dashboard route
app.get("/dashboard", isAuthenticated, (req, res) => {
  res.send("Welcome to dashboard!");
});
```

Notice that `isAuthenticated` is passed directly inside the route — this is called **route-level middleware** and it only applies to that one route.

---

## 6. Middleware Execution Order Matters!

Express runs middleware in the **exact order** you register them. This is a very common source of bugs for beginners.

```js
const express = require("express");
const app = express();

// ❌ WRONG ORDER — req.body will be undefined in the route
app.get("/submit", (req, res) => {
  console.log(req.body); // undefined!
  res.send("ok");
});
app.use(express.json()); // Too late! Already past the route

// ✅ CORRECT ORDER
app.use(express.json());  // Parse body FIRST
app.get("/submit", (req, res) => {
  console.log(req.body); // { name: "Pranav" } ✓
  res.send("ok");
});
```

**Golden Rule:** Register global middleware (body parsers, loggers, CORS) **before** your routes. Register error-handling middleware **after** all routes.

```js
// Correct order template:
app.use(cors());
app.use(morgan("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// → then your routes
app.use("/api/users", userRoutes);
app.use("/api/products", productRoutes);

// → then error handler LAST
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});
```

---

## 7. Passing Data Between Middleware Using `req`

Middleware can **attach custom data** to the `req` object, which subsequent middleware and route handlers can read.

```js
// Middleware that attaches user info to req
function attachUser(req, res, next) {
  // Imagine we decoded a JWT token here
  req.user = {
    id: 42,
    name: "Pranav",
    role: "admin"
  };
  next();
}

app.get("/profile", attachUser, (req, res) => {
  // req.user is now available here
  res.json({ message: `Hello, ${req.user.name}!` });
});
```

This pattern is used everywhere in real-world apps — especially for authentication (attach the logged-in user to `req.user` so every route knows who is making the request).

---

## 8. Writing Reusable Middleware (with Options)

A common pattern is to write a **middleware factory** — a function that returns a middleware function. This lets you configure the middleware from outside.

```js
// A middleware factory that accepts options
function rateLimiter(maxRequests) {
  const requestCounts = {};
  
  // This is the actual middleware function returned
  return function (req, res, next) {
    const ip = req.ip;
    requestCounts[ip] = (requestCounts[ip] || 0) + 1;
    
    if (requestCounts[ip] > maxRequests) {
      return res.status(429).json({ message: "Too many requests!" });
    }
    
    next();
  };
}

// Use it with configuration
app.use(rateLimiter(100)); // allow max 100 requests per IP
```

This is exactly how packages like `cors`, `morgan`, and `express-validator` work internally — they're all factory functions.

---

## 9. Multiple Middleware on a Single Route

You can chain multiple middleware functions on a single route by passing them as additional arguments:

```js
function logRequest(req, res, next) {
  console.log("Logging...");
  next();
}

function validateInput(req, res, next) {
  if (!req.body.name) {
    return res.status(400).json({ error: "Name is required" });
  }
  next();
}

function saveToDatabase(req, res, next) {
  // imagine DB save here
  req.savedId = 101; // attach result
  next();
}

// All three run in order before the final handler
app.post("/create", logRequest, validateInput, saveToDatabase, (req, res) => {
  res.json({ message: "Created!", id: req.savedId });
});

// You can also pass them as an array — same result
const middlewares = [logRequest, validateInput, saveToDatabase];
app.post("/create", middlewares, (req, res) => {
  res.json({ message: "Created!", id: req.savedId });
});
```

---

## 10. Complete Real-World Example

Let's put everything together in one clean, realistic example:

```js
const express = require("express");
const morgan = require("morgan");
const app = express();

// ─── Global Middleware ───────────────────────────────────────────
app.use(morgan("dev"));                         // log all requests
app.use(express.json());                        // parse JSON bodies
app.use(express.urlencoded({ extended: false })); // parse form bodies

// ─── Custom Auth Middleware ──────────────────────────────────────
function isAuthenticated(req, res, next) {
  const token = req.headers["authorization"];
  if (!token || token !== "secret-token") {
    return res.status(401).json({ message: "Unauthorized" });
  }
  req.user = { id: 1, name: "Pranav" }; // attach user to req
  next();
}

// ─── Routes ─────────────────────────────────────────────────────
app.get("/", (req, res) => {
  res.send("Public Home Page");
});

// Protected route — isAuthenticated runs first
app.get("/dashboard", isAuthenticated, (req, res) => {
  res.json({ message: `Welcome, ${req.user.name}!` });
});

app.post("/data", (req, res, next) => {
  try {
    if (!req.body.value) throw new Error("value is required");
    res.json({ received: req.body.value });
  } catch (err) {
    next(err); // forward to error handler
  }
});

// ─── Error Handler (always last) ────────────────────────────────
app.use((err, req, res, next) => {
  console.error("Error:", err.message);
  res.status(err.status || 500).json({ error: err.message });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

## 11. Quick Summary / Cheat Sheet

|Concept|Key Point|
|---|---|
|What is middleware|A function that runs between request and response|
|Signature|`(req, res, next)` — 3 params, or `(err, req, res, next)` for error handlers|
|`next()`|Passes control to the next middleware; skip it = request hangs|
|`app.use()`|Registers global or path-specific middleware|
|Order matters|Register middleware top-to-bottom; parsers before routes; error handler last|
|`req` as a data bus|Attach custom data via `req.user`, `req.someData` etc. to share across middleware|
|Built-in|`express.json()`, `express.urlencoded()`, `express.static()`|
|Third-party|`morgan`, `cors`, `helmet`, `express-validator`, `multer`|
|Error middleware|Must have exactly 4 params; registered after all routes|

---

> ✅ **You now understand middleware deeply. The next natural topic to study is Express Router — which lets you organize your routes and route-level middleware into separate files for cleaner, scalable code.**