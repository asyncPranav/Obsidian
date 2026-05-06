

---

## 1. The "Why" — What Problem Does It Solve?

Before learning any library, always ask: **why does this exist?**

Imagine you have a user registration form. A user submits their data and you save it directly to your database. What stops someone from:

- Submitting an empty username?
- Entering `"not-an-email"` as their email?
- Sending a password that is just `"1"`?
- Injecting malicious script tags like `<script>alert('hacked')</script>` into a name field?

Nothing — unless you **validate and sanitize** the input yourself.

**Validation** means checking if the data meets your rules (e.g., "email must be a valid email format").  
**Sanitization** means cleaning/transforming the data before using it (e.g., trimming spaces, converting `"TRUE"` to a boolean `true`).

You _could_ write all this logic manually using `if` statements, but that gets messy and error-prone very quickly. `express-validator` gives you a clean, declarative, and powerful way to handle all of it.

---

## 2. How It Works — The Mental Model

`express-validator` is built on top of a library called `validator.js`, which is a pure string validation library. What `express-validator` does is wrap those validators into **Express middleware functions** so they plug directly into your request pipeline.

The overall flow looks like this:

```
POST /register
      ↓
  Validation Middleware Runs  ← express-validator checks each field
      ↓
  Route Handler Runs
      ↓
  You check: were there any errors?
      ↓
  Yes → send 400 Bad Request with error details
  No  → proceed with clean, safe data
```

The key insight is that `express-validator` **does not automatically block or reject** a request. It runs your validation rules and **collects the errors**, but it's your job to check for those errors in the route handler and decide what to do. This design gives you full control.

---

## 3. Installation

```bash
npm install express-validator
```

You only need this one package. It internally uses `validator.js` so you don't need to install that separately.

---

## 4. The Three Core Building Blocks

Everything in `express-validator` revolves around three things:

**`body()`** — used to target fields coming from `req.body` (form submissions, JSON payloads).  
**`validationResult()`** — used inside the route handler to collect all the errors that were found.  
**`matchedData()`** — a bonus utility that extracts only the validated fields from the request (ignores everything else).

There are also `param()`, `query()`, `header()`, and `cookie()` which work exactly like `body()` but target different parts of the request. We'll focus on `body()` since it's the most commonly used.

---

## 5. Basic Usage — Step by Step

Let's start with the simplest possible example and build up from there.

```js
const express = require("express");
const { body, validationResult } = require("express-validator");

const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// Step 1: Define your validation rules as an array of middleware
const registerValidation = [
  body("username").notEmpty().withMessage("Username is required"),
  body("email").isEmail().withMessage("Please enter a valid email"),
];

// Step 2: Pass the validation array as middleware BEFORE your route handler
app.post("/register", registerValidation, (req, res) => {
  
  // Step 3: Collect the errors inside the route handler
  const errors = validationResult(req);
  
  // Step 4: Check if there are any errors
  if (!errors.isEmpty()) {
    // errors.array() gives you a clean array of error objects
    return res.status(400).json({ errors: errors.array() });
  }
  
  // If we reach here, all validations passed
  res.json({ message: "Registration successful!", data: req.body });
});

app.listen(3000);
```

When a request comes in, Express runs each item in `registerValidation` one by one (remember, they're just middleware functions). Each one inspects the named field in `req.body` and records any errors it finds. By the time your route handler runs, all the results are already collected and waiting for you inside `validationResult(req)`.

---

## 6. Understanding the Error Object

When you call `errors.array()`, you get an array of objects that look like this:

```json
[
  {
    "type": "field",
    "value": "abc",
    "msg": "Please enter a valid email",
    "path": "email",
    "location": "body"
  }
]
```

Each error object tells you exactly which field failed (`path`), what value was submitted (`value`), and what message you attached (`msg`). This is why `.withMessage()` is so important — without it, you'd get the generic `"Invalid value"` message, which isn't helpful to anyone.

---

## 7. Chaining Validators

The real power comes from **chaining** multiple validators on a single field. You can keep calling validator methods one after another on the same `body()` call:

```js
body("username")
  .notEmpty().withMessage("Username cannot be empty")
  .isLength({ min: 3, max: 20 }).withMessage("Username must be between 3 and 20 characters")
  .isAlphanumeric().withMessage("Username can only contain letters and numbers")
  .trim() // ← sanitization: removes leading/trailing spaces before validating
```

This reads almost like plain English, which makes your validation logic very easy to understand and maintain. Each method in the chain runs in order, and each gets its own `.withMessage()` so the user gets a specific, meaningful error for each rule they broke.

---

## 8. Validation Methods — Explained with Examples

Here are the most important validators you'll use in real projects.

**`notEmpty()`** checks that the field exists and is not an empty string. This is your most basic check and you'll use it on almost every required field.

```js
body("name").notEmpty().withMessage("Name is required")
```

**`isEmail()`** checks that the value is a properly formatted email address. It understands complex email formats so you don't have to write a regex.

```js
body("email").isEmail().withMessage("Invalid email address")
```

**`isLength(options)`** checks the length of the string. You can set `min`, `max`, or both.

```js
body("bio").isLength({ min: 10, max: 500 }).withMessage("Bio must be between 10 and 500 characters")
```

**`isNumeric()`** checks that the value is a number (but the value itself is still a string at this point, since all request data comes in as strings).

```js
body("age").isNumeric().withMessage("Age must be a number")
```

**`isInt(options)`** is more specific — it checks that the value is an integer and optionally within a range.

```js
body("age").isInt({ min: 18, max: 100 }).withMessage("Age must be between 18 and 100")
```

**`isFloat(options)`** works the same way but for decimal numbers.

```js
body("price").isFloat({ min: 0.01 }).withMessage("Price must be a positive number")
```

**`isAlpha()`** checks that the string contains only letters (a–z, A–Z). Useful for name fields where numbers make no sense.

```js
body("firstName").isAlpha().withMessage("First name must contain only letters")
```

**`isAlphanumeric()`** allows both letters and numbers. Perfect for usernames.

```js
body("username").isAlphanumeric().withMessage("Username can only contain letters and numbers")
```

**`isIn(values)`** checks that the submitted value is one of the allowed options you define. This is great for dropdowns, roles, categories, etc.

```js
body("role").isIn(["admin", "editor", "viewer"]).withMessage("Role must be admin, editor, or viewer")
```

**`isURL()`** checks that the value is a valid URL.

```js
body("website").isURL().withMessage("Please enter a valid URL")
```

**`isStrongPassword(options)`** is one of the most useful validators for password fields. You can configure exactly how strong the password must be.

```js
body("password")
  .isStrongPassword({
    minLength: 8,
    minLowercase: 1,  // at least one lowercase letter
    minUppercase: 1,  // at least one uppercase letter
    minNumbers: 1,    // at least one digit
    minSymbols: 1,    // at least one special character like @, #, !
  })
  .withMessage("Password must be at least 8 characters and include uppercase, lowercase, number, and symbol")
```

**`isMobilePhone(locale)`** validates phone numbers. You can pass a locale string like `"en-IN"` for Indian numbers or `"any"` to accept all valid formats globally.

```js
body("phone").isMobilePhone("en-IN").withMessage("Enter a valid Indian mobile number")
```

**`isDate()`** checks if the value is a valid date string.

```js
body("dob").isDate().withMessage("Enter a valid date of birth")
```

**`matches(pattern)`** lets you validate against a custom regular expression when none of the built-in validators cover your case.

```js
// Example: pincode must be exactly 6 digits
body("pincode")
  .matches(/^\d{6}$/)
  .withMessage("Pincode must be exactly 6 digits")
```

---

## 9. Sanitization Methods — Cleaning the Data

Validation checks data, but sanitization **transforms** it. You can and should chain sanitizers right alongside your validators.

**`trim()`** removes whitespace from both ends. This is essential because users often accidentally add spaces, especially on mobile.

```js
body("username").trim().notEmpty().withMessage("Username is required")
// "  pranav  " becomes "pranav" before validation runs
```

**`escape()`** replaces special HTML characters like `<`, `>`, `&`, `"` with their safe HTML entities. This protects against XSS (Cross-Site Scripting) attacks where someone might try to inject `<script>` tags.

```js
body("bio").escape()
// "<script>alert('xss')</script>" becomes "&lt;script&gt;alert(&#x27;xss&#x27;)&lt;&#x2F;script&gt;"
```

**`normalizeEmail()`** standardizes email addresses. For example, Gmail ignores dots and is case-insensitive, so `P.Ranav@Gmail.com` and `pranav@gmail.com` are the same inbox. This sanitizer normalizes them so you don't store duplicates.

```js
body("email").isEmail().normalizeEmail()
// "P.Ranav+spam@Gmail.COM" → "pranav@gmail.com"
```

**`toInt()`** converts a string value to an actual JavaScript integer. Remember, all form/URL data comes in as strings, so `"25"` needs to become `25` if you want to do math or store it as a number.

```js
body("age").isInt().toInt()
// req.body.age is now 25 (number) instead of "25" (string)
```

**`toFloat()`** is the same but for decimal numbers.

```js
body("price").isFloat().toFloat()
// "9.99" → 9.99
```

**`toBoolean()`** converts string values like `"true"`, `"1"`, `"yes"` to `true`, and everything else to `false`.

```js
body("isActive").toBoolean()
// "true" → true, "false" → false
```

**`toLowerCase()` / `toUpperCase()`** converts the string to a specific case.

```js
body("countryCode").toUpperCase()
// "in" → "IN"
```

---

## 10. The `custom()` Validator — Your Escape Hatch

Sometimes the built-in validators don't cover your exact business rule. That's where `.custom()` comes in. You pass it a function that receives the field's value and can throw an error or return a rejected promise if the value is invalid.

```js
body("username")
  .custom(value => {
    // Throw an error to signal validation failure
    if (value.toLowerCase() === "admin") {
      throw new Error("Username 'admin' is not allowed");
    }
    // MUST return true to signal success — this is the bug from your code earlier!
    return true;
  })
```

You can also make it **async**, which is incredibly useful for checking things against your database — like whether a username or email is already taken:

```js
const User = require("./models/User"); // your mongoose model

body("email")
  .isEmail()
  .withMessage("Invalid email")
  .custom(async (value) => {
    // Query the database to check if email already exists
    const existingUser = await User.findOne({ email: value });
    if (existingUser) {
      // Throwing inside an async custom validator works correctly
      throw new Error("An account with this email already exists");
    }
    return true;
  })
```

This is something you simply cannot do with built-in validators, and it's one of the most powerful features of `express-validator`.

> ⚠️ **The Most Common Beginner Bug:** If your `.custom()` function doesn't explicitly `return true` when validation passes, `express-validator` treats it as a failure and shows a generic `"Invalid value"` error. This is exactly what happened in your code earlier. Always `return true` at the end of your custom validator.

---

## 11. The `customSanitizer()` — Your Own Sanitization Logic

Just like `custom()` for validation, `customSanitizer()` lets you transform the value however you want.

```js
body("tags")
  .customSanitizer(value => {
    // Convert a comma-separated string into a proper array
    // "javascript, nodejs, express" → ["javascript", "nodejs", "express"]
    return value.split(",").map(tag => tag.trim().toLowerCase());
  })
```

After this sanitizer runs, `req.body.tags` will be an actual array, ready to use.

---

## 12. Reusing Validation Rules — The Right Pattern

In a real application, you don't want to rewrite the same validation logic in every route. The clean pattern is to define your rules in a **separate file** and import them where needed.

```js
// validators/userValidator.js

const { body } = require("express-validator");

const registerValidation = [
  body("username")
    .trim()
    .notEmpty().withMessage("Username is required")
    .isLength({ min: 3, max: 20 }).withMessage("Username must be 3–20 characters")
    .isAlphanumeric().withMessage("Only letters and numbers allowed")
    .custom(value => {
      if (value.toLowerCase() === "admin") throw new Error("Username 'admin' is reserved");
      return true;
    }),
  
  body("email")
    .isEmail().withMessage("Invalid email address")
    .normalizeEmail(),
  
  body("password")
    .isStrongPassword({ minLength: 8, minLowercase: 1, minUppercase: 1, minNumbers: 1, minSymbols: 1 })
    .withMessage("Password must be strong (8+ chars, upper, lower, number, symbol)"),
  
  body("age")
    .isInt({ min: 18, max: 100 }).withMessage("Age must be between 18 and 100")
    .toInt(), // sanitize: convert string to integer
];

module.exports = { registerValidation };
```

```js
// routes/userRoutes.js

const express = require("express");
const router = express.Router();
const { validationResult } = require("express-validator");
const { registerValidation } = require("../validators/userValidator");

router.post("/register", registerValidation, (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // req.body.age is now a number (because of .toInt())
  res.json({ message: "User registered!", data: req.body });
});

module.exports = router;
```

This separation of concerns makes your code clean, testable, and maintainable.

---

## 13. Extracting a Cleaner Middleware Pattern

A very common pattern in professional codebases is to create a **reusable validation middleware** that handles the error-checking logic so you don't repeat it in every route:

```js
// middleware/validate.js

const { validationResult } = require("express-validator");

// This middleware runs AFTER the validation rules
// It checks if there were errors and handles them automatically
const validate = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  next(); // no errors, pass control to the actual route handler
};

module.exports = validate;
```

```js
// routes/userRoutes.js
const validate = require("../middleware/validate");
const { registerValidation } = require("../validators/userValidator");

// Now your route is super clean — no error-checking clutter inside the handler
router.post("/register", registerValidation, validate, (req, res) => {
  // If we're here, ALL validations passed. Guaranteed.
  res.json({ message: "User registered!", data: req.body });
});
```

This is the pattern used in professional-grade Express applications.

---

## 14. `matchedData()` — Only Get What You Validated

`matchedData()` is a handy utility that extracts only the fields you explicitly validated from the request. This is a security best practice — you prevent unexpected or malicious extra fields from sneaking into your database.

```js
const { body, validationResult, matchedData } = require("express-validator");

app.post("/register", registerValidation, (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
  
  // Instead of using req.body directly (which might have extra fields),
  // use only the fields that were validated
  const data = matchedData(req);
  // data = { username: "pranav", email: "p@gmail.com", age: 21 }
  // Any extra fields the user sent (like "isAdmin: true") are NOT included
  
  res.json({ message: "Success", data });
});
```

---

## 15. Complete Real-World Example

Here is everything put together in one realistic, production-style example:

```js
const express = require("express");
const { body, validationResult, matchedData } = require("express-validator");

const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// ─── Validation Rules ────────────────────────────────────────────────────────

const userValidationRules = [
  body("username")
    .trim()
    .notEmpty().withMessage("Username is required")
    .isLength({ min: 3, max: 20 }).withMessage("Username must be between 3 and 20 characters")
    .isAlphanumeric().withMessage("Username can only contain letters and numbers")
    .custom(value => {
      if (value.toLowerCase() === "admin") throw new Error("This username is reserved");
      return true; // ← always return true on success!
    }),

  body("email")
    .isEmail().withMessage("Please enter a valid email address")
    .normalizeEmail(), // sanitize: lowercase, remove dots for gmail etc.

  body("password")
    .isStrongPassword({ minLength: 8, minLowercase: 1, minUppercase: 1, minNumbers: 1, minSymbols: 1 })
    .withMessage("Password must be at least 8 characters with uppercase, lowercase, number and symbol"),

  body("age")
    .isInt({ min: 18, max: 100 }).withMessage("Age must be a number between 18 and 100")
    .toInt(), // sanitize: convert "21" (string) → 21 (number)

  body("city")
    .isIn(["delhi", "mumbai", "lucknow", "bangalore"]).withMessage("Please select a valid city"),

  body("website")
    .optional() // ← if not provided, skip validation entirely
    .isURL().withMessage("Please enter a valid URL"),
];

// ─── Reusable Validation Error Handler ───────────────────────────────────────

const handleValidationErrors = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ success: false, errors: errors.array() });
  }
  next();
};

// ─── Route ───────────────────────────────────────────────────────────────────

app.post("/register", userValidationRules, handleValidationErrors, (req, res) => {
  // All validations passed — safe to use the data
  const cleanData = matchedData(req); // only get validated fields
  
  // In a real app you'd save cleanData to your database here
  console.log("Clean data to save:", cleanData);
  
  res.status(201).json({
    success: true,
    message: "User registered successfully!",
    user: cleanData,
  });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

## 16. Quick Reference Summary

**The flow you always follow:**

First, you import `body` and `validationResult` from `express-validator`. Then you define an array of validation rules using `body("fieldName").validator().withMessage("...")`. Next you pass that array as middleware in your route. Finally, inside the route handler you call `validationResult(req)` and check `.isEmpty()` — if not empty, return a 400 with the errors; otherwise proceed with the clean data.

**Critical things to remember:**

The `.withMessage()` after each validator is what gives the user a meaningful error instead of a generic one. The `.custom()` validator must always `return true` at the end — forgetting this is the most common bug. Sanitizers like `.trim()`, `.toInt()`, and `.normalizeEmail()` should be placed in the chain alongside validators because they transform the data before it reaches your route handler. The `.optional()` method means "only validate this field if it was actually provided in the request." And always use `matchedData(req)` instead of `req.body` directly when saving to a database, to prevent unexpected fields from being saved.

---

> ✅ **You now have a complete understanding of express-validator. The natural next topic to study is authentication and sessions — specifically how to use `bcrypt` for password hashing and `jsonwebtoken` (JWT) for user authentication, since validation is almost always the entry gate to those flows.**