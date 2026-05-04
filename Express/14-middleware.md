

---

# 🧠 1. WHAT IS MIDDLEWARE (CORE IDEA)

## 🔹 Definition (simple)

👉 Middleware is a **function that runs between request and response**

---

## 🔹 Real Flow

```text
Browser → Request → Middleware → Route → Response → Browser
```

---

## 🔹 Even simpler (like you're 8)

Imagine:

- You ask for food 🍔
    
- Before giving food:
    
    - someone checks your entry (middleware)
        
    - someone takes order (route)
        
    - chef cooks (logic)
        
    - food served (response)
        

👉 Middleware = “helper step before final result”

---

# 🧱 2. BASIC SYNTAX OF MIDDLEWARE

```js
function middleware(req, res, next) {
  // do something
  next();
}
```

---

## 🔍 EXPLAIN EACH PARAMETER (VERY IMPORTANT)

---

## 1. `req` (Request Object)

👉 Contains **data from user**

Example:

```js
req.body
req.params
req.query
req.url
```

### Example:

```js
app.get("/user/:id", (req, res) => {
  console.log(req.params.id);
});
```

---

## 2. `res` (Response Object)

👉 Used to **send data back**

Example:

```js
res.send("Hello")
res.json({ name: "Pranav" })
res.render("home")
```

---

## 3. `next` (MOST IMPORTANT 🔥)

👉 It tells Express:

> “Move to next step”

---

### ❗ If you DON'T call `next()`:

```js
app.use((req, res, next) => {
  console.log("Hello");
  // next() missing ❌
});
```

👉 Request will **hang forever** (no response)

---

### ✅ Correct:

```js
app.use((req, res, next) => {
  console.log("Hello");
  next(); // move forward
});
```

---

# 🔁 3. MIDDLEWARE EXECUTION FLOW

```js
app.use(mw1);
app.use(mw2);

app.get("/", routeHandler);
```

---

## Flow:

```text
Request →
mw1 →
mw2 →
route →
response
```

---

# 🧠 4. TYPES OF MIDDLEWARE (VERY IMPORTANT)

---

# 🔹 TYPE 1: BUILT-IN MIDDLEWARE

You already used these 👇

---

## 1. express.urlencoded()

```js
app.use(express.urlencoded({ extended: false }));
```

### What it does:

👉 Converts form data → JavaScript object

---

### Without it:

```js
req.body = undefined ❌
```

---

### With it:

```js
req.body = {
  first_name: "Pranav"
}
```

---

## 2. express.json()

```js
app.use(express.json());
```

👉 Used when sending JSON (API)

---

## 3. express.static()

```js
app.use(express.static("public"));
```

👉 Allows browser to access:

```text
/public/style.css
/public/image.png
```

---

# 🔹 TYPE 2: APPLICATION-LEVEL MIDDLEWARE

Runs for **every request**

---

## Example:

```js
app.use((req, res, next) => {
  console.log("Request received:", req.method, req.url);
  next();
});
```

---

### Output:

```text
GET /
GET /add-contact
POST /add-contact
```

---

# 🔹 TYPE 3: ROUTE-SPECIFIC MIDDLEWARE

Runs only for a specific route

---

## Example:

```js
function checkSomething(req, res, next) {
  console.log("Checking...");
  next();
}

app.get("/add-contact", checkSomething, (req, res) => {
  res.render("add-contact");
});
```

---

# 🔹 TYPE 4: ERROR-HANDLING MIDDLEWARE 🔥

Special middleware

---

## Syntax:

```js
app.use((err, req, res, next) => {
  res.status(500).send("Error happened");
});
```

---

## ⚠️ IMPORTANT RULE

👉 Must have **4 parameters**

```js
(err, req, res, next)
```

---

# 🧠 5. HOW `next()` WORKS

---

## Case 1: Normal flow

```js
next();
```

➡ goes to next middleware / route

---

## Case 2: Error flow

```js
next(error);
```

➡ skips normal middleware  
➡ goes directly to **error middleware**

---

# 🔥 FLOW VISUAL

### Normal:

```text
Request → Middleware → Route → Response
```

---

### Error:

```text
Request → Route → ERROR → next(error) → Error Middleware → Response
```

---

# 💥 6. REAL EXAMPLE (CONTACT APP)

---

## ❌ WITHOUT MIDDLEWARE

```js
app.get("/show-contact/:id", async (req, res) => {
  const contact = await Contact.findById(req.params.id);
  res.render("show-contact", { contact });
});
```

👉 Problem:

- invalid ID → crash
    

---

## ✅ WITH MIDDLEWARE

```js
app.get("/show-contact/:id", async (req, res, next) => {
  try {
    const contact = await Contact.findById(req.params.id);

    if (!contact) {
      throw new Error("Contact not found");
    }

    res.render("show-contact", { contact });

  } catch (err) {
    next(err);
  }
});
```

---

## Global Error Middleware:

```js
app.use((err, req, res, next) => {
  console.log(err.message);

  res.status(500).send({
    message: err.message
  });
});
```

---

# 🧠 7. CHAINING MULTIPLE MIDDLEWARE

```js
app.use((req, res, next) => {
  console.log("Step 1");
  next();
});

app.use((req, res, next) => {
  console.log("Step 2");
  next();
});

app.get("/", (req, res) => {
  res.send("Done");
});
```

---

## Output:

```text
Step 1
Step 2
Done
```

---

# 🧠 8. USING MIDDLEWARE IN ARRAY

```js
const mw1 = (req, res, next) => { next(); };
const mw2 = (req, res, next) => { next(); };

app.get("/", [mw1, mw2], (req, res) => {
  res.send("Hello");
});
```

---

# 🧠 9. REAL USE CASES OF MIDDLEWARE

---

## 🔹 Logging

```js
app.use((req, res, next) => {
  console.log(req.url);
  next();
});
```

---

## 🔹 Authentication

```js
function isLoggedIn(req, res, next) {
  if (!req.user) {
    return res.send("Login required");
  }
  next();
}
```

---

## 🔹 Validation

```js
function validate(req, res, next) {
  if (!req.body.title) {
    return res.send("Title required");
  }
  next();
}
```

---

# 🧠 10. MIDDLEWARE ORDER (VERY IMPORTANT)

Express runs **top to bottom**

---

## Example:

```js
app.use(mw1);
app.use(mw2);

app.get("/", route);

app.use(errorHandler); // ALWAYS LAST
```

---

## ❗ WRONG ORDER

```js
app.use(errorHandler); ❌
app.get("/", route);
```

👉 error handler will not work properly

---

# ⚠️ 11. COMMON MISTAKES

---

❌ Forgetting `next()`  
❌ Sending response twice  
❌ Not handling async errors  
❌ Wrong middleware order  
❌ Missing 4 params in error middleware

---

# 🧠 12. ADVANCED: ASYNC ERROR HANDLER

Avoid writing try-catch everywhere

---

## Helper:

```js
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

---

## Usage:

```js
app.get("/", asyncHandler(async (req, res) => {
  const data = await Contact.find();
  res.render("home", { data });
}));
```

---

# 🧠 FINAL MENTAL MODEL

Think like this:

```text
Middleware = checkpoints
Route = final action
Error middleware = safety system
```

---

# 🔥 FINAL SUMMARY

👉 Middleware is the **pipeline system of Express**

It:

✔ processes requests  
✔ modifies data  
✔ controls flow  
✔ handles errors  
✔ secures app

---

# 🚀 NEXT STEP (HIGHLY RECOMMENDED)

Now that you understand middleware, next powerful topics are:

- Authentication middleware (login system)
    
- JWT middleware
    
- Role-based access (admin/user)
    
- Custom error classes
    

---

Short answer: **yes — a few important middleware-related concepts are still left** if you want _complete developer-level understanding_.

You’ve already covered core middleware (what it is, `req, res, next`, types, basic examples), but a real backend developer should also know the following **missing / advanced but important topics**:

---

# 🔴 1. ERROR-HANDLING MIDDLEWARE (VERY IMPORTANT)

This is **different from normal middleware**.

### Syntax (4 parameters):

```js
app.use((err, req, res, next) => {
  res.status(500).send("Something broke!");
});
```

👉 Notice:

```
(err, req, res, next)
```

---

## 🔥 Why important?

Without this:

- Your app crashes
    
- Errors are messy
    
- No centralized handling
    

---

## Example (Contact App)

```js
app.get("/show-contact/:id", async (req, res, next) => {
  try {
    const contact = await Contact.findById(req.params.id);
    if (!contact) {
      throw new Error("Contact not found");
    }
    res.render("show-contact", { contact });
  } catch (err) {
    next(err); // send error to middleware
  }
});
```

Error middleware:

```js
app.use((err, req, res, next) => {
  console.log(err.message);
  res.status(500).send(err.message);
});
```

---

# 🔴 2. BUILT-IN MIDDLEWARE (DEEP UNDERSTANDING)

You used them, but need deeper clarity.

---

## (a) express.json()

```js
app.use(express.json());
```

👉 Parses JSON body

Used in APIs:

```json
{
  "name": "Pranav"
}
```

---

## (b) express.urlencoded()

```js
app.use(express.urlencoded({ extended: false }));
```

👉 Parses form data (EJS forms)

---

## Difference:

|Middleware|Used For|
|---|---|
|json()|API (JSON data)|
|urlencoded()|HTML forms|

---

# 🔴 3. THIRD-PARTY MIDDLEWARE

You haven’t used much yet.

---

## Examples:

### (a) Morgan (logging)

```bash
npm install morgan
```

```js
const morgan = require("morgan");
app.use(morgan("dev"));
```

👉 Logs every request

---

### (b) CORS

```js
const cors = require("cors");
app.use(cors());
```

👉 Allows frontend-backend communication

---

### (c) Helmet (security)

```js
const helmet = require("helmet");
app.use(helmet());
```

👉 Secures headers

---

# 🔴 4. CUSTOM MIDDLEWARE (ADVANCED USE)

You know basics, but real power is here.

---

## Example: Logger

```js
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});
```

---

## Example: Auth check

```js
function checkAuth(req, res, next) {
  const isLoggedIn = true;

  if (!isLoggedIn) {
    return res.send("Not allowed");
  }

  next();
}

app.get("/dashboard", checkAuth, (req, res) => {
  res.send("Welcome");
});
```

---

# 🔴 5. MIDDLEWARE CHAINING

Multiple middleware in one route:

```js
app.get("/test",
  (req, res, next) => {
    console.log("Step 1");
    next();
  },
  (req, res, next) => {
    console.log("Step 2");
    next();
  },
  (req, res) => {
    res.send("Done");
  }
);
```

---

# 🔴 6. ROUTER-LEVEL MIDDLEWARE

Used inside routes file.

```js
const router = require("express").Router();

router.use((req, res, next) => {
  console.log("Router middleware");
  next();
});
```

---

# 🔴 7. APPLICATION vs ROUTER MIDDLEWARE

|Type|Where|
|---|---|
|App-level|app.use()|
|Router-level|router.use()|

---

# 🔴 8. ORDER OF MIDDLEWARE (CRITICAL ⚠️)

Middleware runs **top to bottom**

---

## Example:

❌ Wrong:

```js
app.get("/", (req, res) => res.send("Hello"));

app.use(express.json());
```

✔ Correct:

```js
app.use(express.json());

app.get("/", (req, res) => res.send("Hello"));
```

---

# 🔴 9. next() RULES (VERY IMPORTANT)

---

## ✔ Call next()

```js
next();
```

---

## ❌ If you don’t:

Request will hang forever

---

## ❌ Don’t call twice

```js
next();
next(); // ❌ error
```

---

# 🔴 10. next(error)

Special use:

```js
next(new Error("Something went wrong"));
```

👉 skips normal middleware  
👉 goes to error middleware

---

# 🔴 11. ASYNC MIDDLEWARE ERROR (IMPORTANT)

Express doesn’t catch async errors automatically.

---

## ❌ Wrong:

```js
app.get("/", async (req, res) => {
  throw new Error("Crash");
});
```

---

## ✔ Correct:

```js
app.get("/", async (req, res, next) => {
  try {
    throw new Error("Crash");
  } catch (err) {
    next(err);
  }
});
```

---

# 🔴 12. GLOBAL ERROR HANDLER (BEST PRACTICE)

```js
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    message: err.message,
  });
});
```

---

# 🔴 13. 404 MIDDLEWARE

```js
app.use((req, res) => {
  res.status(404).send("Page not found");
});
```

---

# 🔴 14. REAL FLOW (IMPORTANT)

```text
Request
 ↓
Middleware 1
 ↓
Middleware 2
 ↓
Route
 ↓
Controller
 ↓
Response
 ↓
Error Middleware (if error)
```

---

# 🧠 FINAL CHECKLIST (YOU SHOULD KNOW)

✔ What is middleware  
✔ req, res, next  
✔ app.use()  
✔ custom middleware  
✔ route middleware  
✔ error middleware  
✔ async error handling  
✔ middleware order  
✔ next() behavior  
✔ third-party middleware

---

# 🔥 FINAL VERDICT

👉 You are **80–85% done** with middleware knowledge  
👉 After learning above topics → you become **intermediate backend dev**

---
