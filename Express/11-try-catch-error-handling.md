
---

Good — this is exactly where your app becomes “real backend development” instead of just tutorial code.

We’ll build **error handling in Express + MongoDB (Mongoose)** using your **Contact App as reference**, and I’ll explain it step-by-step like a system you can actually reuse in any project.

---

# 🧠 WHAT IS ERROR HANDLING?

In simple words:

👉 Error handling means:

> “What should my app do when something goes wrong?”

Examples:

- database is not connected
    
- wrong id is sent
    
- user submits empty form
    
- page not found
    
- server crashes
    

Without error handling:  
❌ app crashes  
❌ blank screen  
❌ hard to debug

With error handling:  
✔ clean response  
✔ controlled behavior  
✔ no crash  
✔ better UX

---

# 🧱 TYPES OF ERRORS IN EXPRESS

In your Contact app, errors mainly come from:

## 1. Sync Errors (normal JS errors)

```js
let a = b + 1; // b not defined
```

## 2. Async Errors (MOST IMPORTANT in your app)

MongoDB / Mongoose:

```js
await Contact.findById(id)
```

## 3. Validation Errors (wrong data)

```js
email: undefined
```

## 4. Cast Errors (VERY COMMON in MongoDB)

Invalid ObjectId:

```
/show-contact/123abc
```

## 5. Route Errors

Wrong URL:

```
/abc-route
```

---

# ⚡ BASIC ERROR HANDLING IN EXPRESS

## 1. TRY–CATCH (MOST IMPORTANT)

### Example from your Contact App:

```js
app.get("/show-contact/:id", async (req, res) => {
  try {
    const contact = await Contact.findById(req.params.id);

    if (!contact) {
      return res.status(404).send("Contact not found");
    }

    res.render("show-contact", { contact });

  } catch (error) {
    res.status(500).send("Something went wrong");
  }
});
```

---

## 🧠 WHAT IS HAPPENING HERE?

### try block:

👉 “Try to run normal code”

```js
const contact = await Contact.findById(req.params.id);
```

---

### catch block:

👉 “If anything breaks, run this”

```js
catch (error)
```

---

## 💥 REAL SCENARIO

User visits:

```
/show-contact/invalidID
```

MongoDB throws error:

```
CastError: ObjectId failed
```

👉 Without try-catch → app crashes  
👉 With try-catch → error handled safely

---

# 🧠 IMPORTANT PATTERN (YOU SHOULD ALWAYS USE THIS)

```js
try {
   // database logic
} catch (error) {
   console.log(error);
   res.status(500).send("Server Error");
}
```

---

# 🔥 ERROR HANDLING IN YOUR CONTACT APP (REAL CASES)

---

# 1. GET ALL CONTACTS

```js
app.get("/", async (req, res) => {
  try {
    const contacts = await Contact.find();
    res.render("home", { contacts });
  } catch (error) {
    res.status(500).send("Failed to load contacts");
  }
});
```

---

# 2. SHOW SINGLE CONTACT

```js
app.get("/show-contact/:id", async (req, res) => {
  try {
    const contact = await Contact.findById(req.params.id);

    if (!contact) {
      return res.status(404).send("Contact not found");
    }

    res.render("show-contact", { contact });

  } catch (error) {
    res.status(500).send("Invalid ID or server error");
  }
});
```

---

# 3. ADD CONTACT

```js
app.post("/add-contact", async (req, res) => {
  try {
    await Contact.create(req.body);
    res.redirect("/");
  } catch (error) {
    res.status(400).send("Failed to create contact");
  }
});
```

---

# 4. UPDATE CONTACT

```js
app.post("/update-contact/:id", async (req, res) => {
  try {
    await Contact.findByIdAndUpdate(req.params.id, req.body);
    res.redirect("/");
  } catch (error) {
    res.status(400).send("Update failed");
  }
});
```

---

# 5. DELETE CONTACT

```js
app.get("/delete-contact/:id", async (req, res) => {
  try {
    await Contact.findByIdAndDelete(req.params.id);
    res.redirect("/");
  } catch (error) {
    res.status(500).send("Delete failed");
  }
});
```

---

# 💣 WHAT IS `error` OBJECT?

Inside catch:

```js
catch (error)
```

You can see:

```js
console.log(error.message)
```

Example output:

```
CastError: ObjectId is invalid
```

---

# 🚨 STATUS CODES (VERY IMPORTANT)

|Code|Meaning|
|---|---|
|200|OK|
|201|Created|
|400|Bad request|
|404|Not found|
|500|Server error|

---

# 🧠 BEST PRACTICE (REAL INDUSTRY STYLE)

Instead of repeating try-catch everywhere:

👉 use middleware error handler

---

# ⚙️ GLOBAL ERROR HANDLER

### Step 1:

```js
app.use((err, req, res, next) => {
  console.log(err.stack);
  res.status(500).send("Something broke!");
});
```

---

### Step 2: throw error manually

```js
if (!contact) {
  throw new Error("Contact not found");
}
```

---

# 🧠 ASYNC ERROR PATTERN (VERY IMPORTANT)

Instead of writing try-catch everywhere:

### helper function:

```js
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

---

### usage:

```js
app.get("/", asyncHandler(async (req, res) => {
  const contacts = await Contact.find();
  res.render("home", { contacts });
}));
```

---

# 💡 REAL WORLD FLOW

1. Request comes
    
2. Route executes
    
3. Controller runs
    
4. DB query runs
    
5. If error → catch block OR middleware
    
6. Response sent safely
    

---

# ⚠️ COMMON BEGINNER MISTAKES

❌ No try-catch  
❌ App crashes on invalid id  
❌ No 404 handling  
❌ Sending raw errors to user

---

# 🧱 BONUS: 404 ERROR HANDLING

```js
app.use((req, res) => {
  res.status(404).send("Page not found");
});
```

---

# 🧠 FINAL SIMPLE MEMORY

👉 try = normal code  
👉 catch = if something fails  
👉 middleware = global safety net

---

# 🔥 ONE LINE SUMMARY

Error handling = making sure your app NEVER crashes and always responds properly even when something goes wrong.
