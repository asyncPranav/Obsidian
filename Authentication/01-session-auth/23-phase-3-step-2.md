
---


Excellent. **Step 1 — Ownership Authorization is DONE.** ✅

Now we've reached the second type of authorization we planned from the beginning:

# Step 11 — Role-Based Authorization

So far our security system can answer:

> **"Is this user logged in?"** → Authentication

and:

> **"Does this task belong to this user?"** → Ownership authorization

Now we add:

> **"Does this user have the required role?"** → Role-based authorization

Your `User` model already has this:

```js
role: {
  type: String,
  enum: ["user", "admin"],
  default: "user",
}
```

So we're finally going to **use the `role` field for something.**

---

## 1. Authentication vs authorization

Keep this distinction very clear:

```text
AUTHENTICATION
    ↓
Who are you?
    ↓
req.user


AUTHORIZATION
    ↓
What are you allowed to do?
    ↓
req.user.role
    +
    ownership
```

For example:

```text
User A
role = "user"

Admin A
role = "admin"
```

Both can authenticate.

But they don't necessarily have the same permissions.

---

# 2. Our first admin-only endpoint

Let's use your existing User API.

Currently:

```http
GET /api/users
```

is publicly accessible.

That's not what we want.

We want:

```text
GET /api/users
       ↓
authenticate
       ↓
authorize("admin")
       ↓
getAllUsers
```

Therefore:

```text
Regular user → ❌ 403
Admin        → ✅ 200
Not logged in → ❌ 401
```

This gives us a very clean demonstration of role-based authorization.

---

# 3. Create authorization middleware

Create:

```text
src/middlewares/authorize.middleware.js
```

We'll make it reusable.

```js
import ApiError from "../utils/ApiError.js";

const authorize = (...allowedRoles) => {
  return (req, res, next) => {
    if (!allowedRoles.includes(req.user.role)) {
      return next(
        new ApiError(403, "You are not authorized to perform this action"),
      );
    }

    next();
  };
};

export default authorize;
```

Now let's understand this carefully.

---

# 4. Why `...allowedRoles`?

Suppose we write:

```js
authorize("admin")
```

Then:

```js
allowedRoles
```

becomes:

```js
["admin"]
```

If we write:

```js
authorize("admin", "user")
```

then:

```js
allowedRoles
```

becomes:

```js
["admin", "user"]
```

So this middleware can be reused for different situations.

---

# 5. The important line

```js
allowedRoles.includes(req.user.role)
```

Suppose:

```text
req.user.role = "user"
```

and:

```js
allowedRoles = ["admin"]
```

Then:

```js
["admin"].includes("user")
```

is:

```text
false
```

So:

```text
403 Forbidden
```

But if:

```text
req.user.role = "admin"
```

then:

```js
["admin"].includes("admin")
```

is:

```text
true
```

So:

```text
next()
```

and the controller executes.

---

# 6. Why doesn't `authorize` check the session?

Because that's **not its job**.

We already have:

```text
authenticate
    ↓
req.user
```

Authentication middleware answers:

```text
"Who is this?"
```

Authorization middleware answers:

```text
"Is this role allowed?"
```

That's why we'll compose them:

```js
authenticate,
authorize("admin"),
getAllUsers
```

---

# 7. Protect `/api/users`

Your current user routes contain:

```js
router
  .route("/")
  .get(getAllUsers)
  .post(createUserValidator, validate, createUser);
```

Change the GET route:

```js
router
  .route("/")
  .get(authenticate, authorize("admin"), getAllUsers)
  .post(createUserValidator, validate, createUser);
```

Notice registration remains public:

```text
POST /api/users
       ↓
createUser
```

because a person needs to be able to register **before they have authenticated**.

Although your actual auth registration route may already be separate; keep whichever route you're currently using for registration.

---

# 8. What happens now?

### Not logged in

```text
GET /api/users
      ↓
authenticate
      ↓
No session
      ↓
401 Unauthorized
```

### Logged-in normal user

```text
GET /api/users
      ↓
authenticate
      ↓
req.user
      ↓
authorize("admin")
      ↓
role = "user"
      ↓
403 Forbidden
```

### Logged-in admin

```text
GET /api/users
      ↓
authenticate
      ↓
req.user
      ↓
authorize("admin")
      ↓
role = "admin"
      ↓
getAllUsers
      ↓
200 OK
```

This is the complete role-based authorization flow.

---

# 9. How do we test an admin?

Your registration defaults to:

```js
role: "user"
```

That's good.

**Do not allow the client to simply register as admin.**

For example, don't change your registration to accept:

```json
{
  "name": "Hacker",
  "email": "hacker@example.com",
  "password": "Password1!",
  "role": "admin"
}
```

Otherwise anyone could create an admin account. 😬

For our learning project, create an admin directly in MongoDB/Compass, or temporarily change an existing user's role in the database:

```text
role: "user"
```

→

```text
role: "admin"
```

Then login as that user.

---

# 10. Test in Postman

### Test 1 — No login

```http
GET /api/users
```

Expected:

```text
401
```

### Test 2 — Normal user

Login as:

```text
role = user
```

Then:

```http
GET /api/users
```

Expected:

```text
403
```

### Test 3 — Admin

Change/create a user's role:

```text
role = admin
```

Login again.

Then:

```http
GET /api/users
```

Expected:

```text
200
```

---

# 11. One subtle but important thing

You might wonder:

> Why do we need both `authenticate` and `authorize`?

Because `authorize` depends on:

```js
req.user
```

And `req.user` is created by:

```js
authenticate
```

Therefore this is correct:

```js
authenticate,
authorize("admin"),
getAllUsers
```

This is wrong:

```js
authorize("admin"),
authenticate,
getAllUsers
```

because when `authorize` runs:

```js
req.user
```

doesn't exist yet.

---

# 12. Our architecture now

We have three different security concepts:

```text
                  REQUEST
                     │
                     ▼
              AUTHENTICATION
                     │
                "Who are you?"
                     │
                     ▼
                  req.user
                     │
             ┌───────┴────────┐
             │                │
             ▼                ▼
      ROLE AUTHORIZATION   OWNERSHIP
             │                │
        "What role?"     "Whose resource?"
             │                │
             └───────┬────────┘
                     ▼
                  Controller
```

This is the architecture I wanted you to understand when we started this project.

---

# 13. What comes after Step 11?

Once you test role authorization successfully, we'll do a **cleanup + security pass** rather than immediately adding random features.

We'll cover:

```text
Step 12
────────────────────────
401 vs 403 properly
        ↓
Session logout
        ↓
Session destruction
        ↓
Protected routes after logout
```

Then:

```text
Step 13
────────────────────────
Clean up controllers
        ↓
Use req.task
        ↓
Remove duplicate DB queries
        ↓
Review complete request flow
```

Then we'll have a final:

```text
Step 14
────────────────────────
Session security
Cookie settings
Session expiration
Common mistakes
Production considerations
```

And after you've understood all of that, **we can compare this session architecture with JWT**, which will make JWT much easier to understand.

### Your immediate task

Implement only:

```text
☐ authorize.middleware.js
☐ Protect GET /api/users with authenticate + authorize("admin")
☐ Test unauthenticated → 401
☐ Test normal user → 403
☐ Test admin → 200
```

Once that works, **Step 11 is complete.**