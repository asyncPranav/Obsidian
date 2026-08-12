
---


Good. Now we build the **first actual piece of Phase 2: registration with password hashing**.

Don't worry about sessions yet. We're only solving:

> **How do we safely create an authenticated user account?**

---

## Step 2 — Create the Auth Controller

Your current `user.controller.js` handles general user CRUD:

```text
/api/users
```

Authentication deserves its own controller:

```text
src/
├── controllers/
│   ├── auth.controller.js    ← NEW
│   ├── task.controller.js
│   └── user.controller.js
```

Create:

```text
src/controllers/auth.controller.js
```

For now, we'll create only **one function**:

```js
import bcrypt from "bcrypt";
import User from "../models/user.model.js";
import ApiError from "../utils/ApiError.js";

const register = async (req, res, next) => {
  try {
    const { name, email, password } = req.body;

    // 1. Check whether email already exists
    const existingUser = await User.findOne({ email });

    if (existingUser) {
      return next(new ApiError(409, "Email is already registered"));
    }

    // 2. Hash the password
    const hashedPassword = await bcrypt.hash(password, 10);

    // 3. Create user with hashed password
    const user = await User.create({
      name,
      email,
      password: hashedPassword,
    });

    // 4. Never send password back to client
    res.status(201).json({
      status: "success",
      message: "User registered successfully",
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role,
          createdAt: user.createdAt,
          updatedAt: user.updatedAt,
        },
      },
    });
  } catch (error) {
    // Handle MongoDB duplicate email race condition
    if (error.code === 11000) {
      return next(new ApiError(409, "Email is already registered"));
    }

    next(error);
  }
};

export { register };
```

Now let's **understand this rather than just paste it.**

---

# 1. Import bcrypt

```js
import bcrypt from "bcrypt";
```

This gives our application access to bcrypt's password hashing functions.

We're mainly interested in:

```js
bcrypt.hash()
bcrypt.compare()
```

We'll use:

```text
hash()
```

during registration.

Later during login we'll use:

```text
compare()
```

---

# 2. Get the registration data

```js
const { name, email, password } = req.body;
```

Suppose Postman sends:

```json
{
  "name": "Pranav",
  "email": "pranav@example.com",
  "password": "Pranav@123"
}
```

Then:

```text
req.body.name
    ↓
"Pranav"

req.body.email
    ↓
"pranav@example.com"

req.body.password
    ↓
"Pranav@123"
```

At this moment, `password` is still the **plain-text password**.

That's okay temporarily.

We haven't stored it anywhere.

---

# 3. Check whether the user already exists

```js
const existingUser = await User.findOne({ email });
```

This asks MongoDB:

> "Is there already a user with this email?"

If yes:

```js
if (existingUser) {
  return next(new ApiError(409, "Email is already registered"));
}
```

The request stops there.

So:

```text
email exists?
   │
 ┌─┴─┐
YES  NO
 │    │
 ▼    ▼
409  hash password
```

---

# 4. The most important line

```js
const hashedPassword = await bcrypt.hash(password, 10);
```

Let's break this down.

### Input

```text
password
    ↓
"Pranav@123"
```

### bcrypt

```text
bcrypt.hash("Pranav@123", 10)
```

### Output

Something resembling:

```text
$2b$10$...
```

The exact output will be different.

And that's important.

---

# 5. Why does bcrypt produce different hashes?

Suppose you register two accounts with:

```text
password = "hello123"
```

You might get:

```text
$2b$10$ABC...
```

for one user and:

```text
$2b$10$XYZ...
```

for another.

Why?

Because bcrypt uses a **salt**.

Conceptually:

```text
password
   +
random salt
   ↓
bcrypt
   ↓
hash
```

The salt makes identical passwords produce different hashes.

The salt information is incorporated into the bcrypt hash, so bcrypt can use it later when checking the password.

---

# 6. What's the `10`?

This:

```js
bcrypt.hash(password, 10)
```

is the **cost factor / salt rounds**.

For now, understand it as:

> How much computational work bcrypt should perform.

Higher cost generally means more work per password hash.

For our learning project:

```js
10
```

is a reasonable starting point.

---

# 7. Now we create the user

Instead of your Phase 1 code:

```js
User.create({
  name,
  email,
  password,
});
```

we now do:

```js
User.create({
  name,
  email,
  password: hashedPassword,
});
```

Notice what happened:

```text
BEFORE

password
   ↓
"Pranav@123"
   ↓
MongoDB ❌


NOW

password
   ↓
"Pranav@123"
   ↓
bcrypt.hash()
   ↓
"$2b$10$..."
   ↓
MongoDB ✅
```

This is the core change.

---

# 8. The response

We're deliberately returning:

```js
user: {
  id: user._id,
  name: user.name,
  email: user.email,
  role: user.role,
  ...
}
```

and **not**:

```js
user: user
```

because the full Mongoose document contains:

```text
password
```

Even though it's now a hash, we still don't want to expose it.

---

# 9. Now we need a route

Create:

```text
src/routes/auth.route.js
```

Put:

```js
import express from "express";

import { register } from "../controllers/auth.controller.js";
import createUserValidator from "../validators/createUser.validator.js";
import validate from "../middlewares/validate.middleware.js";

const router = express.Router();

router.post("/register", createUserValidator, validate, register);

export default router;
```

Now the flow is:

```text
POST /register
      ↓
createUserValidator
      ↓
validate
      ↓
register controller
```

---

# 10. Connect the auth router to `app.js`

Your current `app.js` has:

```js
import userRouter from "./routes/user.route.js";
import taskRouter from "./routes/task.route.js";
```

Add:

```js
import authRouter from "./routes/auth.route.js";
```

Then:

```js
app.use("/api/auth", authRouter);
```

So your routes become:

```text
/api/users
/api/tasks
/api/auth
```

And registration becomes:

```text
POST /api/auth/register
```

---

# 11. Your complete flow now

When you send:

```http
POST /api/auth/register
```

with:

```json
{
  "name": "Pranav",
  "email": "pranav@example.com",
  "password": "Pranav@123"
}
```

the request travels:

```text
                     POST /api/auth/register
                              │
                              ▼
                       auth.route.js
                              │
                              ▼
                 createUserValidator
                              │
                              ▼
                       validate.js
                              │
                         valid?
                       /       \
                     NO         YES
                     ↓           ↓
                  ApiError     register()
                                  │
                                  ▼
                         User.findOne()
                                  │
                         email exists?
                           /       \
                         YES        NO
                         ↓           ↓
                       409       bcrypt.hash()
                                     │
                                     ▼
                              User.create()
                                     │
                                     ▼
                                  MongoDB
                                     │
                                     ▼
                                   201
```

---

# 12. One important change from Phase 1

We are **not removing** your existing:

```text
POST /api/users
```

yet.

For now you technically have two ways to create users:

```text
POST /api/users
POST /api/auth/register
```

But they have different purposes.

### `/api/users`

General user resource CRUD.

### `/api/auth/register`

Authentication-related account creation.

Eventually, we'll decide how we want to structure/secure the user CRUD endpoints once authentication and authorization are fully implemented.

---

## Do this now

Make these changes:

```text
src/controllers/auth.controller.js
src/routes/auth.route.js
app.js
```

Then start your server and test:

```http
POST http://localhost:5000/api/auth/register
```

with:

```json
{
  "name": "Test User",
  "email": "test@example.com",
  "password": "Test@123"
}
```

### Then check MongoDB

**Do not just look at the API response.**

Open the `users` collection and inspect the document.

You should see something conceptually like:

```text
name: "Test User"
email: "test@example.com"
password: "$2b$10$..........."
role: "user"
```

and **NOT**:

```text
password: "Test@123"
```

That's your first real Phase 2 checkpoint.

Once you've tested this, tell me **what happened**, especially what you see in MongoDB. Then we'll move to the next piece rather than blindly stacking code.