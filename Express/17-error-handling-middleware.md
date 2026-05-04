
Now let’s go **deep into Error Handling Middleware in Express.js** — this is one of the most important concepts for real-world apps 👇

---

# 📘 Error Handling Middleware in Express.js

## 🔹 What is Error Handling Middleware?

👉 It is a **special type of middleware** used to handle errors in your application.

Unlike normal middleware, it has **4 parameters**:

```js
(err, req, res, next)
```

👉 This is how Express पहचानता hai ki ye error middleware hai.

---

# 🔹 Why Do We Need It?

Without error handling:

- App crash ho sakta hai ❌
    
- User ko proper response nahi milega ❌
    
- Debugging difficult ho jaata hai ❌
    

With error handling:

- Errors safely handle hote hain ✅
    
- User-friendly messages milte hain ✅
    
- App stable rehta hai ✅
    

---

# 🔹 Basic Syntax

```js
app.use((err, req, res, next) => {
  console.error(err.message);
  res.status(500).send("Something went wrong");
});
```

👉 Must be placed **after all routes**

---

# 🔹 How Errors Occur?

Errors can happen in:

- Middleware
    
- Routes
    
- Async operations
    
- Database calls
    

---

# 🔹 How to Pass Errors

## 1. Using `next(err)`

```js
app.get("/", (req, res, next) => {
  const error = new Error("Something broke!");
  next(error);
});
```

👉 This sends error to error middleware

---

## 2. Throwing Error (sync)

```js
app.get("/", (req, res) => {
  throw new Error("Crash!");
});
```

👉 Express automatically catches sync errors

---

## 3. Async Errors (IMPORTANT ⚠️)

Express **does NOT catch async errors automatically**

### ❌ Wrong:

```js
app.get("/", async (req, res) => {
  throw new Error("Async error");
});
```

👉 App crash ho sakta hai

---

### ✅ Correct:

```js
app.get("/", async (req, res, next) => {
  try {
    throw new Error("Async error");
  } catch (err) {
    next(err);
  }
});
```

---

# 🔹 Full Example

```js
const express = require("express");
const app = express();

// Route with error
app.get("/", (req, res, next) => {
  const err = new Error("Something went wrong!");
  next(err);
});

// Error middleware
app.use((err, req, res, next) => {
  console.log("Error middleware triggered");
  res.status(500).send(err.message);
});

app.listen(3000);
```

---

# 🔹 Multiple Error Middleware

```js
app.use((err, req, res, next) => {
  console.log("First handler");
  next(err);
});

app.use((err, req, res, next) => {
  console.log("Second handler");
  res.status(500).send("Handled");
});
```

---

# 🔹 Custom Error Handling

```js
class CustomError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
  }
}

app.get("/", (req, res, next) => {
  next(new CustomError("Not Found", 404));
});

app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).send(err.message);
});
```

---

# 🔹 404 Error Handling

```js
app.use((req, res, next) => {
  res.status(404).send("Route not found");
});
```

👉 Must be after all routes

---

# 🔹 Order of Middleware ⚠️

```id="2r5m9h"
Routes → 404 handler → Error middleware
```

👉 Wrong order = errors not handled properly

---

# 🔹 Async Wrapper (Best Practice)

```js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get("/", asyncHandler(async (req, res) => {
  throw new Error("Async error");
}));
```

---

# 🔹 Real-world Example (API)

```js
app.get("/user/:id", (req, res, next) => {
  const user = null;

  if (!user) {
    return next(new Error("User not found"));
  }

  res.send(user);
});
```

---

# 🔹 Error Response Format (Best Practice)

```js
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).json({
    success: false,
    message: err.message,
  });
});
```

---

# 🔹 Common Mistakes ❌

### 1. Missing `next`

```js
app.use((err, req, res) => { ... });
```

👉 Must include `next`

---

### 2. Wrong placement

```js
app.use(errorMiddleware);
app.get("/", ...)
```

👉 ❌ Error middleware should be last

---

### 3. Not handling async errors

---

# 🔹 Difference: Normal vs Error Middleware

|Feature|Normal Middleware|Error Middleware|
|---|---|---|
|Parameters|(req, res, next)|(err, req, res, next)|
|Trigger|Every request|Only on error|
|Usage|Logic handling|Error handling|

---

# 🔹 Real-life Analogy

- Normal middleware → worker doing tasks
    
- Error middleware → **complaint department**
    

👉 Jab problem hoti hai → complaint department handle karta hai

---

# 🔹 Final Summary

- Error middleware has **4 params**
    
- Always defined **at the end**
    
- Use `next(err)` to pass errors
    
- Handle async errors properly
    
- Can create custom error classes
    
- Improves stability & debugging
    

---

Below is a **real-world, production-style Error Handling Middleware setup in Express.js**, split into files so you can understand how it is used in actual projects.

---

# 📁 Project Structure

```text
project/
│
├── app.js
├── routes/
│   └── userRoutes.js
└── middleware/
    └── errorHandler.js
```

---

# 📄 1. `routes/userRoutes.js` (Routes that may throw errors)

```js
// Import express
const express = require("express");

// Create router
const router = express.Router();


// =====================================
// 🔹 ROUTE 1: NORMAL ROUTE
// =====================================

router.get("/", (req, res) => {
  res.send("User Home Page");
});


// =====================================
// 🔹 ROUTE 2: ERROR USING next()
// =====================================

router.get("/manual-error", (req, res, next) => {
  const error = new Error("Manual error triggered!");
  error.statusCode = 400; // custom status code

  // Pass error to error handling middleware
  next(error);
});


// =====================================
// 🔹 ROUTE 3: THROWING ERROR (SYNC)
// =====================================

router.get("/sync-error", (req, res) => {
  // Express will automatically catch this
  throw new Error("Synchronous error occurred!");
});


// =====================================
// 🔹 ROUTE 4: ASYNC ERROR (IMPORTANT)
// =====================================

router.get("/async-error", async (req, res, next) => {
  try {
    // Simulating async failure (like DB call)
    throw new Error("Async operation failed!");
  } catch (err) {
    // Must pass error to next()
    next(err);
  }
});


// Export router
module.exports = router;
```

---

# 📄 2. `middleware/errorHandler.js` (Error Handling Middleware)

```js
// =====================================
// 🔥 ERROR HANDLING MIDDLEWARE
// =====================================

// This is SPECIAL middleware (4 parameters required)
function errorHandler(err, req, res, next) {
  console.log("💥 Error Middleware Triggered");

  // Log full error for debugging
  console.error("Error Message:", err.message);
  console.error("Stack:", err.stack);

  // Default status code if not set
  const statusCode = err.statusCode || 500;

  // Send structured error response
  res.status(statusCode).json({
    success: false,
    message: err.message || "Internal Server Error",
    statusCode: statusCode,
  });
}


// Export middleware
module.exports = errorHandler;
```

---

# 📄 3. `app.js` (Main Application File)

```js
// Import express
const express = require("express");

// Create app
const app = express();


// =====================================
// 🔹 1. GLOBAL MIDDLEWARE
// =====================================

// Parse JSON body
app.use(express.json());

// Logger middleware
app.use((req, res, next) => {
  console.log(`🌍 ${req.method} ${req.url}`);
  next();
});


// =====================================
// 🔹 2. IMPORT ROUTES
// =====================================

const userRoutes = require("./routes/userRoutes");


// Mount router
app.use("/user", userRoutes);


// =====================================
// 🔹 3. 404 MIDDLEWARE (NOT FOUND)
// =====================================

// If no route matches, this runs
app.use((req, res, next) => {
  const error = new Error("Route Not Found");
  error.statusCode = 404;

  next(error); // send to error handler
});


// =====================================
// 🔹 4. ERROR HANDLING MIDDLEWARE (LAST)
// =====================================

const errorHandler = require("./middleware/errorHandler");

// MUST be last middleware
app.use(errorHandler);


// =====================================
// 🔹 5. START SERVER
// =====================================

app.listen(3000, () => {
  console.log("🚀 Server running on http://localhost:3000");
});
```

---

# 🔥 How Error Flow Works

### Example request:

👉 `GET /user/async-error`

### Execution flow:

```text
Request → Route → Error Occurs → next(err) → Error Middleware → Response
```

---

# 🔹 Types of Errors Covered

## 1. Manual Error

```js
next(error);
```

---

## 2. Synchronous Error

```js
throw new Error()
```

---

## 3. Async Error

```js
try/catch + next(err)
```

---

## 4. 404 Error

```js
Route not found → error created → next()
```

---

# 🔹 Important Rules ⚠️

## ❌ Wrong Order

```js
app.use(errorHandler);
app.use(routes);
```

👉 Wrong ❌ (error handler must be last)

---

## ✅ Correct Order

```js
app.use(routes);
app.use(errorHandler);
```

---

# 🔹 Key Features of Error Middleware

✔ Has 4 parameters `(err, req, res, next)`  
✔ Runs only when error is passed  
✔ Must be at the END of file  
✔ Handles all app errors centrally  
✔ Prevents app crash  
✔ Gives clean response to user

---

# 🔹 Real-world Analogy

👉 Think of it like:

- Normal routes = workers
    
- Errors = problems
    
- Error middleware = **complaint department**
    

When something breaks:  
➡ Problem goes to complaint department  
➡ It handles everything properly  
➡ User gets clean response

---

# 🔹 Final Summary

- Error middleware is **central error handler**
    
- Always placed **last**
    
- Used via:
    
    - `next(err)`
        
    - `throw new Error()`
        
- Handles:
    
    - sync errors
        
    - async errors
        
    - route errors
        
    - 404 errors
        
- Keeps app stable and production-ready
    

---

If you want next level:  
🔥 JWT auth error handling  
🔥 Database error handling (MongoDB/Mongoose)  
🔥 Complete backend architecture (like production apps)

Just tell 👍