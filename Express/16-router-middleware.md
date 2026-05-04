
---

Alright—now let’s go **deep into Router Middleware in Express.js** with clear theory + solid examples 👇

---

# 📘 Router Middleware in Express.js

## 🔹 What is a Router?

A **Router** in Express is like a **mini application** that handles a group of related routes.

```js
const express = require("express");
const router = express.Router();
```

👉 It helps you:

- Organize code
    
- Separate features (user, admin, products)
    
- Keep app clean & modular
    

---

# 🔹 What is Router Middleware?

👉 **Router middleware** is middleware applied **only to a specific router**, not the whole app.

Defined using:

```js
router.use()
router.METHOD()
```

👉 It runs **only for routes inside that router**

---

# 🔹 Why Use Router Middleware?

- Modular code
    
- Feature-based logic
    
- Different middleware for different parts of app
    

👉 Example:

- `/admin` → authentication middleware
    
- `/public` → no auth
    

---

# 🔹 Basic Structure

```js
const express = require("express");
const app = express();
const router = express.Router();

// Router middleware
router.use((req, res, next) => {
  console.log("Router middleware running");
  next();
});

// Route
router.get("/", (req, res) => {
  res.send("Router Home");
});

// Attach router
app.use("/user", router);

app.listen(3000);
```

---

# 🔹 Flow of Execution

When user hits:

```
http://localhost:3000/user/
```

👉 Flow:

```id="rf1a0s"
Request → app.use("/user") → router middleware → route → response
```

---

# 🔹 Types of Router Middleware

---

## 1. Router-level Middleware (router.use)

Runs for **all routes in that router**

### Example:

```js
router.use((req, res, next) => {
  console.log("Runs for every route in this router");
  next();
});

router.get("/", (req, res) => {
  res.send("Home");
});

router.get("/about", (req, res) => {
  res.send("About");
});
```

👉 Runs for:

- `/`
    
- `/about`
    

---

## 2. Path-specific Router Middleware

```js
router.use("/admin", (req, res, next) => {
  console.log("Admin middleware");
  next();
});

router.get("/admin/dashboard", (req, res) => {
  res.send("Dashboard");
});
```

👉 Runs only for:

- `/admin/*`
    

---

## 3. Route-level Middleware (inside router)

```js
function checkUser(req, res, next) {
  console.log("Checking user...");
  next();
}

router.get("/profile", checkUser, (req, res) => {
  res.send("Profile Page");
});
```

👉 Runs only for `/profile`

---

# 🔹 Multiple Middleware in Router

```js
function m1(req, res, next) {
  console.log("M1");
  next();
}

function m2(req, res, next) {
  console.log("M2");
  next();
}

router.get("/", m1, m2, (req, res) => {
  res.send("Done");
});
```

---

# 🔹 Router Mounting (VERY IMPORTANT)

```js
app.use("/user", router);
```

👉 This means:

- All routes in router get prefix `/user`
    

### Example:

```js
router.get("/profile", ...)
```

👉 Final route:

```id="cz67wl"
/user/profile
```

---

# 🔹 Middleware Order in Router ⚠️

Order matters inside router

```js
router.use((req, res, next) => {
  console.log("First");
  next();
});

router.use((req, res, next) => {
  console.log("Second");
  next();
});
```

👉 Output:

```id="lmkf4z"
First
Second
```

---

# 🔹 Combining App Middleware + Router Middleware

```js
app.use((req, res, next) => {
  console.log("App middleware");
  next();
});

router.use((req, res, next) => {
  console.log("Router middleware");
  next();
});

app.use("/user", router);
```

---

### Flow:

```id="8zazr2"
Request → App middleware → Router middleware → Route → Response
```

---

# 🔹 Real-world Example (Auth Middleware)

### auth.js

```js
function auth(req, res, next) {
  const isLoggedIn = true;

  if (!isLoggedIn) {
    return res.send("Not authorized");
  }

  next();
}

module.exports = auth;
```

---

### router.js

```js
const express = require("express");
const router = express.Router();
const auth = require("./auth");

// Apply middleware to entire router
router.use(auth);

router.get("/dashboard", (req, res) => {
  res.send("Dashboard");
});

router.get("/settings", (req, res) => {
  res.send("Settings");
});

module.exports = router;
```

---

### app.js

```js
const express = require("express");
const app = express();
const userRouter = require("./router");

app.use("/user", userRouter);

app.listen(3000);
```

---

# 🔹 Router-level Error Handling

```js
router.use((err, req, res, next) => {
  console.error(err.message);
  res.status(500).send("Router Error");
});
```

---

# 🔹 next() in Router Middleware

👉 Same as app middleware:

- Moves to next middleware or route
    

### ❌ Missing next():

```js
router.use((req, res, next) => {
  console.log("Stops here");
});
```

👉 Request will hang

---

# 🔹 Skipping Middleware (Advanced)

```js
router.get("/test", (req, res, next) => {
  next("route"); // skip remaining middleware
}, (req, res) => {
  res.send("Won't run");
});
```

---

# 🔹 Difference: App vs Router Middleware

|Feature|App Middleware|Router Middleware|
|---|---|---|
|Scope|Whole app|Specific router|
|Defined using|app.use()|router.use()|
|Use case|Global logic|Feature-specific logic|

---

# 🔹 When to Use Router Middleware?

Use it when:

- You want **separate logic for modules**
    
- You have:
    
    - `/user`
        
    - `/admin`
        
    - `/products`
        

👉 Example:

- Admin routes → strict auth
    
- User routes → normal auth
    

---

# 🔹 Real-life Analogy

- `app.use()` → Security at **main gate**
    
- `router.use()` → Security inside **specific department**
    

---

# 🔹 Final Summary

- Router middleware runs **only inside a router**
    
- Defined using `router.use()`
    
- Helps in:
    
    - modular structure
        
    - feature separation
        
- Works with:
    
    - router-level
        
    - path-level
        
    - route-level middleware
        
- Always use `next()` to continue flow
    
- Order matters
    

---


Below is a **clean, real-world style example of Router Middleware in Express.js**, split into **multiple files** like in production projects.

I’ll also add **detailed comments so you understand every line clearly**.

---

# 📁 Project Structure

```text
project/
│
├── app.js
└── routes/
    └── userRoutes.js
```

---

# 📄 1. `routes/userRoutes.js` (Router + Router Middleware)

```js
// Import express
const express = require("express");

// Create router instance
const router = express.Router();


// =====================================
// 🔹 1. ROUTER-LEVEL MIDDLEWARE (GLOBAL FOR THIS ROUTER)
// =====================================

// This middleware runs for ALL routes inside this router
router.use((req, res, next) => {
  console.log("🔵 Router Middleware Running");

  // Logs request method and URL (relative to router)
  console.log(`Method: ${req.method}, URL: ${req.url}`);

  // Example: add custom data to request object
  req.requestTime = new Date().toISOString();

  // Pass control to next middleware/route
  next();
});


// =====================================
// 🔹 2. PATH-SPECIFIC ROUTER MIDDLEWARE
// =====================================

// This runs only for routes starting with /admin
router.use("/admin", (req, res, next) => {
  console.log("🔐 Admin Middleware (Router Level)");

  const isAdmin = true; // pretend auth check

  if (!isAdmin) {
    return res.status(403).send("Access Denied");
  }

  next();
});


// =====================================
// 🔹 3. ROUTES INSIDE ROUTER
// =====================================

// Home route inside router
router.get("/", (req, res) => {
  console.log("🏠 User Home Route");

  res.send(`User Home Page | Time: ${req.requestTime}`);
});


// Profile route
router.get("/profile", (req, res) => {
  console.log("👤 Profile Route");

  res.send(`User Profile Page | Time: ${req.requestTime}`);
});


// Admin route (protected by /admin middleware)
router.get("/admin/dashboard", (req, res) => {
  console.log("🛡️ Admin Dashboard Route");

  res.send("Admin Dashboard");
});


// =====================================
// 🔹 EXPORT ROUTER
// =====================================

module.exports = router;
```

---

# 📄 2. `app.js` (Main Application File)

```js
// Import express
const express = require("express");

// Create app
const app = express();


// =====================================
// 🔹 1. APPLICATION-LEVEL MIDDLEWARE
// =====================================

// Runs for EVERY request in the app
app.use((req, res, next) => {
  console.log("🌍 App-Level Middleware");

  console.log(`Incoming Request: ${req.method} ${req.url}`);

  next();
});


// Built-in middleware (parses JSON body)
app.use(express.json());


// =====================================
// 🔹 2. IMPORT ROUTER
// =====================================

const userRoutes = require("./routes/userRoutes");


// =====================================
// 🔹 3. MOUNT ROUTER
// =====================================

// All routes inside userRoutes will start with /user
app.use("/user", userRoutes);


// =====================================
// 🔹 4. DEFAULT ROUTE
// =====================================

app.get("/", (req, res) => {
  res.send("🏠 Main Home Page");
});


// =====================================
// 🔹 5. 404 MIDDLEWARE (NOT FOUND)
// =====================================

app.use((req, res) => {
  console.log("❌ 404 Middleware");

  res.status(404).send("Route Not Found");
});


// =====================================
// 🔹 START SERVER
// =====================================

app.listen(3000, () => {
  console.log("🚀 Server running on http://localhost:3000");
});
```

---

# 🔥 How This Works (Execution Flow)

### Example request:

👉 `GET /user/profile`

### Flow:

```text
1. App Middleware runs
2. Router Middleware runs
3. /user route middleware runs
4. Route handler executes
5. Response sent
```

---

# 🔹 Key Concepts in This Code

## ✅ 1. Router Middleware

```js
router.use((req, res, next) => { ... });
```

👉 Runs for ALL routes in that router

---

## ✅ 2. Path-based Router Middleware

```js
router.use("/admin", ...)
```

👉 Runs only for `/admin/*`

---

## ✅ 3. App Middleware

```js
app.use((req, res, next) => { ... });
```

👉 Runs for entire application

---

## ✅ 4. Mounting Router

```js
app.use("/user", userRoutes);
```

👉 Adds `/user` prefix to all router routes

---

# 🔥 Real-world Understanding

Think like this:

### 🏢 App = Building

### 🚪 Router = Department

### 👮 Middleware = Security Guards

|Type|Where it works|
|---|---|
|App Middleware|Whole building|
|Router Middleware|Only that department|
|Path Middleware|Specific rooms inside department|

---

# 🔹 Final Summary

- Router middleware = `router.use()`
    
- Works only inside that router
    
- Can modify request object
    
- Can block requests (auth checks)
    
- Helps in modular architecture
    
- Always use `next()` to continue flow
    
- Mounted using `app.use("/path", router)`
    

---

If you want next level, I can give you:  
🔥 JWT authentication using router middleware  
🔥 Real project (User + Admin system)  
🔥 Folder structure used in companies  
🔥 Interview questions on middleware

Just tell 👍