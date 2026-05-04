
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
