

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

